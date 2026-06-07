# Contributing to vishalj0501 Plugins

Thanks for helping improve these plugins. This repository ships two plugins — **Jobscout**
(`plugins/jobscout/`) and **LeadScout** (`plugins/leadscout/`). Each plugin's behavior lives in one
shared skill; the host manifests only handle installation and UI metadata. Keep that separation when
you contribute, and keep the two plugins independent — neither should require the other.

## Proposing changes

For either plugin, the same principles apply (paths shown per plugin):

- **Workflow or guardrail changes** (onboarding, search, verification, no-fabrication rule): edit the
  skill's `SKILL.md` — `skills/job-search/SKILL.md` or `skills/lead-search/SKILL.md`. Keep it concise
  and push detail into `references/`.
- **Scoring changes:** edit `references/scoring-rubric.md`. Explain why the dimension weights change.
- **Profile or report/list format changes:** edit `references/profile-template.md` or
  `references/report-template.md`, and update the matching example (`examples/example-report.md` for
  Jobscout, `examples/example-list.md` for LeadScout).
- **LeadScout sourcing changes:** edit `plugins/leadscout/skills/lead-search/references/sourcing-playbook.md`
  to add or adjust where each people bucket is found and how a contact channel is verified.
- **Manifest or packaging changes:** edit the host manifest(s). Behavior must not diverge between
  hosts — the shared skill stays the single source of truth. Manifests may differ only where host
  schema or UI metadata requires it.

Open an issue or PR describing the motivation and the user-facing effect.

## Hard boundaries (do not weaken these)

These are core to what the plugins are; contributions must not erode them:

- **Jobscout** finds the opportunity and stops — it never collects a poster's personal contact
  details and never drafts or sends outreach.
- **LeadScout** captures only contact channels a person **intentionally published in public**, and
  **never contacts anyone**. No guessing or pattern-inferring emails, no people-data brokers
  (Apollo, Hunter, RocketReach, ZoomInfo), no scraping behind login walls, no agencies/company
  accounts, no dossiers.
- **Neither plugin fabricates results.** If a session cannot do live web research, it says so and
  invents nothing.

A change that adds scraping, email-guessing, broker lookups, or auto-outreach will not be accepted.

## Use invented data only

Never include a real resume, candidate profile, job report, generated people-list, credential, access
token, or any third party's contact details in a contribution or issue report. All examples and
reproductions must use clearly fictional data — for both candidate users and any people surfaced (see
`examples/example-report.md` and `examples/example-list.md`, where every name, handle, and URL is
invented).

## Validation before submitting

**Claude Code**

```shell
claude plugin validate .
claude plugin validate ./plugins/jobscout
claude plugin validate ./plugins/leadscout
```

This checks `marketplace.json`, each plugin manifest, and skill frontmatter.

**Codex**

Validate `plugins/jobscout/.codex-plugin/plugin.json`, `plugins/leadscout/.codex-plugin/plugin.json`,
and the marketplace entries using Codex's plugin tooling (the `plugin-creator` skill / `codex plugin`
commands). Confirm there are no placeholder, schema, or missing-file errors, and that both hosts
discover the same shared skill (`job-search`, `lead-search`).

## Manual check

From a clean test working directory, exercise each plugin's workflows end to end.

**Jobscout**

1. Onboarding creates `.jobscout/profile.md` and ensures `.jobscout/` is git-ignored.
2. A search produces a dated report of verified ranked matches, each with a source URL, checked date,
   score, rationale, and gaps.
3. A must-have violation is excluded from the ranking.
4. An unverifiable lead appears under **Needs Verification**, not in the ranked table.

**LeadScout**

1. Onboarding creates `.leadscout/profile.md` (pre-filling from `.jobscout/profile.md` if present),
   and ensures `.leadscout/` is git-ignored.
2. A search produces a dated list of verified people, each with at least one public channel, a source
   URL showing where it was published, a checked date, score, and reachability note.
3. A person with no confirmable public channel appears under **Identified, No Public Channel**, not in
   the ranked table.
4. Opt-out / "not looking" signals and agency/company entries are excluded.
