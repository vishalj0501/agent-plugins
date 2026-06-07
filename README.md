# vishalj0501 Plugins

An open-source plugin marketplace for **Claude Code** and **Codex** with two complementary tools for
finding work and the people behind it. Each plugin is powered by one shared skill; the host-specific
manifests only describe installation and UI metadata.

| Plugin | What it finds | Stops at |
| --- | --- | --- |
| **Jobscout** | Current, verified public job and gig listings | The opportunity — never the poster's contact details |
| **LeadScout** | Real people worth reaching out to, with their publicly published contact channels | The list — never sends or drafts outreach |

Both keep all personal data in your own working directory (`.jobscout/`, `.leadscout/`), never inside
the plugin, and never invent results when a session cannot do live web research.

## Install the marketplace

Add this repository once; then install whichever plugin(s) you want.

**Claude Code**

```shell
/plugin marketplace add vishalj0501/agent-plugins
/plugin install jobscout@vishalj0501-plugins
/plugin install leadscout@vishalj0501-plugins
```

Reload if needed with `/reload-plugins`.

**Codex**

```shell
codex plugin marketplace add vishalj0501/agent-plugins
codex plugin add jobscout@vishalj0501-plugins
codex plugin add leadscout@vishalj0501-plugins
```

---

# Jobscout

Jobscout helps you build a reusable private search profile, find current public job postings, verify
each listing before recommending it, rank matches, and save concise dated shortlists on your own
machine. One shared skill (`plugins/jobscout/skills/job-search/`) powers both hosts.

## What Jobscout does

- Onboards you into a reusable private profile (titles, skills, seniority, locations, must-haves).
- Searches public web sources, including direct employer career pages and public job-board pages.
- Opens source pages to verify a listing is still active before ranking it.
- Ranks verified matches with a transparent 100-point rubric.
- Saves dated reports locally with source URLs, checked dates, scores, and fit rationale.

## What Jobscout does not do

- No logging into LinkedIn, Indeed, Wellfound, or any job board.
- No automatic job applications, and no resume or cover-letter generation.
- No application/interview/offer tracking, and no unattended background searches or alerts.
- No bundled scraper, browser automation, MCP server, or external API service.

## Usage

- "Set up my Jobscout profile." / "Use my resume and preferences to build a job profile."
- "Find jobs for me." / "Find remote backend roles that match my Jobscout profile."
- "Run a new job search and save the results."

The skill is namespaced as `/jobscout:job-search` (Claude Code) or `$job-search` (Codex), and is also
invoked automatically when you ask to set up a profile or find jobs.

---

# LeadScout

LeadScout is the people-finding sibling to Jobscout. It helps you find real **people** worth reaching
out to — recruiters and HR, hiring managers, freelance buyers/clients, and peers in your field —
verify that each has a genuinely public way to be contacted, rank them by how worthwhile the outreach
is, and save dated lists locally. One shared skill (`plugins/leadscout/skills/lead-search/`) powers
both hosts.

## What LeadScout does

- Onboards you into a reusable outreach-target profile (goal, field, target companies/communities,
  which people buckets to include, opt-out rules). Pre-fills from your Jobscout profile if you have
  one, then keeps its own copy.
- Searches public web sources per bucket — job postings, public team pages, Hacker News "Who is
  hiring?"/freelancer threads, Reddit `r/forhire`, GitHub, technical blogs, conference pages.
- Captures **only contact channels the person published in public**: their public LinkedIn/X profile
  URL, and an email **only if they posted it themselves**.
- Opens each source to confirm the channel is public, intentional, and live before listing the person.
- Ranks verified people with a transparent 100-point rubric and saves dated lists locally.

## What LeadScout does not do

- **No contacting anyone** — it produces a list and stops. It does not draft, send, or schedule
  outreach, DMs, or connection requests.
- No guessing or pattern-inferring emails (`first.last@company.com`).
- No people-data brokers or enrichment APIs (Apollo, Hunter, RocketReach, ZoomInfo).
- No scraping or reading data from behind a login wall, including LinkedIn member data.
- No agencies or company accounts — people only. No dossiers: only the minimal public professional
  facts needed to decide whether and how to reach out.

A person appears in the ranked list only when at least one contact channel is **public, intentionally
published for contact, and currently live**. Expect recruiter and hiring-manager results to be thinner
on public channels, and freelance-buyer and peer results to be richer — that is the honest shape of
the public web, and the list says so.

## Usage

- "Set up my LeadScout profile." / "Help me configure who I should reach out to."
- "Find people I can reach out to who match my profile."
- "Find recruiters and hiring managers at companies that match my profile."
- "Find people hiring freelancers in my field."
- "Run a new outreach search and save the results."

The skill is namespaced as `/leadscout:lead-search` (Claude Code) or `$lead-search` (Codex), and is
also invoked automatically when you ask to set up a profile or find people.

A reminder: the people LeadScout surfaces are public, but reaching out is your responsibility — be
respectful and lawful, honor "no recruiters / not looking" signals, and don't spam.

---

## Private local storage (both plugins)

Each plugin stores everything in the working directory you invoke it from — never in the plugin:

```
<your-working-directory>/
├── .jobscout/
│   ├── profile.md
│   └── reports/
│       └── YYYY-MM-DD-HHMM-<search-slug>.md
└── .leadscout/
    ├── profile.md
    └── lists/
        └── YYYY-MM-DD-HHMM-<search-slug>.md
```

Before writing personal files, each plugin ensures its directory (`.jobscout/` or `.leadscout/`) is
listed in your `.gitignore` when a Git repository is present, so your profiles, reports, and people-
lists are not accidentally committed. If it cannot safely update `.gitignore`, it asks you first.

Avoid pasting sensitive personal data (full address, government IDs, etc.) into a search session —
neither plugin needs it. LeadScout lists store other people's public contact details; treat those
files as sensitive and keep them local.

## Uninstall and remove your data

- **Claude Code:** `/plugin uninstall jobscout@vishalj0501-plugins` and/or
  `/plugin uninstall leadscout@vishalj0501-plugins` (and
  `/plugin marketplace remove vishalj0501-plugins` to drop the marketplace).
- **Codex:** remove the `jobscout` and/or `leadscout` plugin through Codex's plugin commands.
- **Your data:** delete the local `.jobscout/` and/or `.leadscout/` directory in any working
  directory where you used the plugin. That removes all saved profiles, reports, and lists.

## Tested hosts

- Claude Code: packaging validated with `claude plugin validate`.
- Codex: see the packaging notes in [CONTRIBUTING.md](./CONTRIBUTING.md).

Specific tested host versions will be listed here per tagged release.

## License

MIT — see [LICENSE](./LICENSE).
