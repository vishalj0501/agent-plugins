# Jobscout Public Plugin Implementation Specification

## Status

- Product: `jobscout`
- Marketplace repository: `https://github.com/vishalj0501/agent-plugins`
- Plugin identifier: `jobscout`
- Maintainer display name: `vishalj0501`
- License: MIT
- Target hosts: Codex and Claude Code
- Release target: v1 open-source plugin marketplace repository

## Product Summary

Jobscout is an open-source job-search plugin that helps an individual create a reusable search
profile, find current public job postings, verify listings before recommending them, rank matches,
and save concise private shortlists locally.

The v1 product must be usable by people who clone or install the public repository. Personal
profiles and reports are user-owned local data and must not be committed into the plugin source or
marketplace repository.

## Goals

- Publish an installable marketplace repository for both Codex and Claude Code.
- Maintain one shared job-search workflow implementation for both hosts.
- Onboard a user into a reusable private job-search profile.
- Search public listings using host-provided web research capabilities.
- Verify listing availability from source pages before ranking active opportunities.
- Save reproducible, dated shortlists containing evidence and fit reasoning.
- Make privacy behavior and installation clear to new public users.

## Non-Goals For V1

- Submitting job applications automatically.
- Logging into LinkedIn, Indeed, Wellfound, or other job boards.
- Bundling scraper code, browser automation, an MCP server, or an external API service.
- Tracking application status, interviews, follow-ups, or offers.
- Producing tailored resumes or cover letters.
- Scheduling unattended background searches or alerts.

## Repository Layout

The repository is a marketplace root containing one portable plugin. The shared skill content
lives in `plugins/jobscout/skills/`; host-specific manifests only describe installation and UI
metadata.

```text
agent-plugins/
|-- .agents/
|   `-- plugins/
|       `-- marketplace.json
|-- .claude-plugin/
|   `-- marketplace.json
|-- plugins/
|   `-- jobscout/
|       |-- .codex-plugin/
|       |   `-- plugin.json
|       |-- .claude-plugin/
|       |   `-- plugin.json
|       `-- skills/
|           `-- job-search/
|               |-- SKILL.md
|               |-- agents/
|               |   `-- openai.yaml
|               |-- references/
|               |   |-- profile-template.md
|               |   |-- scoring-rubric.md
|               |   `-- report-template.md
|               `-- examples/
|                   `-- example-report.md
|-- .gitignore
|-- CONTRIBUTING.md
|-- IMPLEMENTATION_SPEC.md
|-- LICENSE
`-- README.md
```

No `.mcp.json`, `.app.json`, job-board integration, or credential configuration is included in
v1.

## Distribution And Host Packaging

### Codex

Codex packaging must include:

- `.agents/plugins/marketplace.json` at the repository root.
- `plugins/jobscout/.codex-plugin/plugin.json`.
- A manifest reference to `./skills/`.

The Codex marketplace entry must expose `jobscout` from `./plugins/jobscout`, use the
`Productivity` category, and declare the supported installation and authentication policy fields.
The plugin manifest must advertise onboarding and verified job-search starter prompts without
claiming MCP or app integrations.

The README must document installation from the GitHub marketplace repository and installation of
the `jobscout` plugin through Codex's plugin commands.

### Claude Code

Claude Code packaging must include:

- `.claude-plugin/marketplace.json` at the repository root.
- `plugins/jobscout/.claude-plugin/plugin.json`.
- The same shared `plugins/jobscout/skills/job-search/SKILL.md` used by Codex.

The Claude marketplace manifest must identify the public owner as `vishalj0501`, expose the
`jobscout` plugin directory, and avoid duplicating job-search behavior in host-specific files.

The README must document adding `vishalj0501/agent-plugins` as a Claude Code marketplace and installing
the `jobscout` plugin from that marketplace.

### Compatibility Policy

- Jobscout's behavioral source of truth is the shared skill and its references.
- Codex and Claude manifests may differ only where host schema or UI metadata requires it.
- The README must state which hosts are tested for each tagged release.

## User Workflows

### 1. Onboard Me

The skill must trigger for requests such as:

- "Set up my Jobscout profile."
- "Help me configure my job search."
- "Use my resume and preferences to build a job profile."

The onboarding workflow must:

1. Ask for or extract target job titles, core skills, seniority, preferred industries, location
   constraints, remote or hybrid preferences, work authorization constraints, compensation
   preferences, excluded roles or companies, and strict must-have filters. Two questions must be
   asked explicitly because they materially change which listings fit: **company stage** (startups
   only, established/corporate only, or no preference) and **employment type** (which of
   full-time, contract, and freelance the user accepts, and whether internships are acceptable).
   Internships are excluded by default unless the user explicitly accepts them, recorded as a strict
   must-have when excluded.
2. Permit resume input by pasted text or a file provided in the user's active workspace, without
   copying source resumes into tracked plugin content.
3. Explain that the output is private local data before writing it.
4. Ensure `.jobscout/` is ignored by Git in the chosen working directory before storing personal
   content. If the assistant cannot safely update `.gitignore`, it must ask the user for approval
   or provide the exact privacy action needed before writing the profile.
5. Save a complete profile at `.jobscout/profile.md`.
6. Summarize the active constraints and ask the user to correct any inaccurate extracted facts.

### 2. Find Jobs For Me

The skill must trigger for requests such as:

- "Find jobs for me."
- "Find remote backend roles that match my Jobscout profile."
- "Run a new job search and save the results."

The search workflow must:

1. Load `.jobscout/profile.md`; if it does not exist, run onboarding before personalized ranking.
2. Read prior reports in `.jobscout/reports/` and prefer listings not already surfaced; mark any
   previously seen listing as such so the user can tell what is new on a repeat search.
3. Accept temporary refinements for the current search without overwriting the saved profile
   unless the user asks to update it.
4. Cover the whole profile: search across all allowed locations, acceptable adjacent titles, and
   accepted employment types (full-time, contract, freelance) rather than narrowing to one.
5. Search public web sources, preferring fetchable ones (direct employer career pages and
   ATS-hosted postings such as Greenhouse, Lever, Ashby, Workable) and deprioritizing aggregators
   that routinely block automated fetches (LinkedIn, Wellfound, RemoteRocketship, Glassdoor,
   Indeed), using those only as discovery hints to be verified on a fetchable source. The goal is
   to find paid work, not only full-time roles: when the profile accepts contract or freelance, also
   look for contractor gigs and people seeking freelancers (company contract postings, agency and
   consultancy openings, publicly readable "who's hiring"/"seeking a freelancer" listings, and
   public hiring communities such as Reddit `r/forhire`/`r/jobbit`/`r/hiring`), verifying each on a
   readable page. Login-walled marketplaces (Upwork, Toptal, Fiverr) and social posts on X/LinkedIn
   that only surface through search are discovery hints only and route to `Needs Verification` when
   they cannot be confirmed. Jobscout never collects a poster's personal contact details and never
   drafts or sends outreach — it finds the opportunity and stops there.
6. Open source pages when possible and check that each ranked job still represents an active or
   available posting; capture the URL and absolute verification date for each. Treat a blocked or
   failed fetch (403, login wall, timeout) as could-not-verify: do not retry repeatedly and route
   it to `Needs Verification` instead of the ranked table.
7. Remove jobs that fail a strict must-have constraint, including internships when they are
   excluded. While a source page is open, also verify the employment type and note whether the
   employer matches the profile's company-stage preference.
8. Rank all verified matches that pass the filters according to the scoring rubric. There is no
   fixed cap; aim for at least 20 verified ranked matches, broadening across locations, adjacent
   titles, and employment types until that target is met or verified postings are exhausted.
9. Put promising but inaccessible, closed, stale, or insufficiently verified discoveries in a
   separate `Needs Verification` section rather than presenting them as active ranked matches.
10. Save the finished report under `.jobscout/reports/YYYY-MM-DD-HHMM-<search-slug>.md`, including
   the time so multiple searches on the same day do not collide.

If the active host session cannot perform live web research, Jobscout must say that current
verified job results cannot be produced in that session. It must not invent postings, URLs,
verification status, dates, or scores.

## Private Data Model

Jobscout uses the invoking user's current working directory as its data home. This avoids embedding
personal data in the published plugin and allows a user to select the project or folder that owns
their search history.

```text
<chosen-working-directory>/
`-- .jobscout/
    |-- profile.md
    `-- reports/
        `-- YYYY-MM-DD-HHMM-<search-slug>.md
```

Required privacy behavior:

- Add `.jobscout/` to the working directory's `.gitignore` before writing private files when a Git
  repository is present.
- Do not write personal files under `plugins/jobscout/`.
- Do not commit a real resume, candidate profile, job report, credential, access token, or private
  contact information.
- Include only invented example data in public documentation and examples.
- Document removal as deletion of the local `.jobscout/` directory by the user.

## Profile Format

`.jobscout/profile.md` is human-readable Markdown so the user can review and edit it. It must
include:

- Profile creation or update date.
- Target titles and acceptable adjacent titles.
- Seniority and accepted employment types (full-time, contract, freelance), including whether
  internships are acceptable.
- Company-stage preference (startups only, established/corporate only, or no preference).
- Core skills and secondary skills.
- Target industries or domain preferences.
- Allowed locations and remote, hybrid, or onsite policy.
- Work authorization, sponsorship, and relocation constraints when supplied.
- Compensation expectations when supplied.
- Excluded titles, employers, industries, or keywords.
- Must-have rejection filters.
- Optional notes from the user's resume or search strategy.

Missing optional information must be marked as unspecified, not inferred as a hard constraint.

## Report Format

Each saved search report must include:

- Search date and the profile file used.
- Search-specific refinements.
- Verification policy used for the run.
- A ranked table or concise section containing every verified opportunity that passed the filters
  (no fixed cap), ordered by score.
- One detail block per ranked job with title, company, location, work arrangement, source URL,
  checked date, score, fit rationale, gaps or risks, and recommended next action.
- A `Needs Verification` section for leads not suitable for the verified ranking.
- A summary of rejected categories when strict profile constraints eliminated results.

Reports must use absolute dates, because job availability changes quickly.

## Ranking Rubric

Verified listings are scored out of 100:

| Dimension | Points | Evaluation |
| --- | ---: | --- |
| Role and skills match | 40 | Target title alignment and evidence of core required skills. |
| Location and eligibility | 25 | Location, remote policy, authorization, sponsorship, and relocation fit. |
| Seniority fit | 15 | Alignment with experience level and responsibility scope. |
| Compensation and preferences | 10 | Compensation evidence, company-stage fit (startup vs corporate), and additional user preferences, when available. |
| Freshness and source confidence | 10 | Availability verification, posting recency evidence, and source quality. |

Hard must-have violations exclude a listing before scoring. When job descriptions omit optional
information such as compensation, Jobscout must identify the unknown instead of treating it as a
match or rejection unless the profile declares that information mandatory.

## Open-Source Documentation

`README.md` must contain:

- What Jobscout does and does not do.
- Codex installation instructions.
- Claude Code installation instructions.
- Onboarding and search invocation examples for both hosts.
- Explanation of `.jobscout/` private local storage and Git ignore behavior.
- A reminder to verify job availability and avoid submitting sensitive data unnecessarily.
- Instructions for uninstalling the plugin and removing saved local user data.
- Supported host versions or tested release notes when known.

`CONTRIBUTING.md` must contain:

- How to propose workflow, scoring, manifest, or documentation changes.
- The requirement to use invented/test candidate data in contributions and issue reports.
- Validation steps for Codex and Claude Code plugin packaging.

`LICENSE` must contain the MIT License attributed to `vishalj0501` unless the maintainer replaces
that copyright name before release.

## Implementation Sequence

1. Add public repository foundations: `LICENSE`, `.gitignore`, `README.md`, and
   `CONTRIBUTING.md`.
2. Add the dual-host marketplace directories and manifests with `jobscout` metadata and public
   install locations.
3. Add the shared `job-search` skill, keeping its main instructions concise and placing profile,
   report, and ranking specifics in referenced Markdown files.
4. Add non-personal examples demonstrating onboarding output and a verified report format.
5. Run host-specific validation and manually exercise onboarding and search from a clean test
   workspace.
6. Document the tested installation and invocation commands in the README for the public release.

## Validation And Acceptance Criteria

### Packaging

- Codex validates `plugins/jobscout/.codex-plugin/plugin.json` and its skill metadata without
  placeholder, schema, or missing-file errors.
- The Codex marketplace manifest points at `./plugins/jobscout`.
- Claude Code recognizes the repository marketplace and the plugin manifest using its documented
  marketplace and plugin conventions.
- Both hosts discover the same shared `job-search` skill content.

### User Experience

- A new user can install Jobscout from `vishalj0501/agent-plugins` by following only the README.
- Onboarding creates `.jobscout/profile.md` in the user's selected working directory and protects
  it from accidental Git tracking.
- A live search produces a dated report listing every verified match that passed the filters (no
  fixed cap, aiming for at least 20), ordered by score, and includes source URLs, checked dates,
  scores, rationale, and gaps.
- A listing that violates a must-have filter is not ranked.
- A listing that cannot be verified is visibly separated from active recommendations.
- A session without web research does not present fabricated current listings.

### Privacy And Publication

- The public repository contains no real applicant data, private generated search reports,
  credentials, or the maintainer's configured local email.
- All public example data is fictional.
- Users can remove private stored data by deleting their local `.jobscout/` directory.

## Reference Documentation

- Codex plugin scaffold and manifest rules are provided by the local Codex `plugin-creator` skill.
- Claude Code plugins: <https://code.claude.com/docs/en/plugins>
- Claude Code marketplaces: <https://code.claude.com/docs/en/plugin-marketplaces>
- Claude Code skills: <https://code.claude.com/docs/en/skills>
