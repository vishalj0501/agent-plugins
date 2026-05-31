# Jobscout

Jobscout is an open-source job-search plugin for **Claude Code** and **Codex**. It helps you build a
reusable private search profile, find current public job postings, verify each listing before
recommending it, rank matches, and save concise dated shortlists on your own machine.

One shared skill (`plugins/jobscout/skills/job-search/`) powers both hosts. The host-specific
manifests only describe installation and UI metadata.

## What Jobscout does

- Onboards you into a reusable private profile (titles, skills, seniority, locations, must-haves).
- Searches public web sources, including direct employer career pages and public job-board pages.
- Opens source pages to verify a listing is still active before ranking it.
- Ranks up to ten verified matches with a transparent 100-point rubric.
- Saves dated reports locally with source URLs, checked dates, scores, and fit rationale.

## What Jobscout does not do

- No logging into LinkedIn, Indeed, Wellfound, or any job board.
- No automatic job applications, and no resume or cover-letter generation.
- No application/interview/offer tracking, and no unattended background searches or alerts.
- No bundled scraper, browser automation, MCP server, or external API service.

If a session cannot perform live web research, Jobscout says so plainly and does not invent
postings, URLs, dates, or scores.

## Install — Claude Code

Add this repository as a marketplace, then install the plugin:

```shell
/plugin marketplace add vishalj0501/agent-plugins
/plugin install jobscout@vishalj0501-plugins
```

Reload if needed with `/reload-plugins`. The skill is namespaced as `/jobscout:job-search`, and
Claude will also invoke it automatically when you ask to set up a profile or find jobs.

## Install — Codex

> Codex packaging (`.agents/plugins/marketplace.json` and `plugins/jobscout/.codex-plugin/plugin.json`)
> is maintained alongside this repository. Once present, add the marketplace repo and install
> `jobscout` through Codex's plugin commands. The same shared `job-search` skill is used.

## Usage

Onboarding (either host):

- "Set up my Jobscout profile."
- "Use my resume and preferences to build a job profile."

Searching (either host):

- "Find jobs for me."
- "Find remote backend roles that match my Jobscout profile."
- "Run a new job search and save the results."

## Private local storage

Jobscout stores everything in the working directory you invoke it from — never in the plugin:

```
<your-working-directory>/
└── .jobscout/
    ├── profile.md
    └── reports/
        └── YYYY-MM-DD-<search-slug>.md
```

Before writing personal files, Jobscout ensures `.jobscout/` is listed in your `.gitignore` when a
Git repository is present, so your profile and reports are not accidentally committed. If it cannot
safely update `.gitignore`, it asks you first.

A reminder: verify job availability yourself before acting, and avoid pasting sensitive personal
data (full address, government IDs, etc.) into a search session — Jobscout does not need it.

## Uninstall and remove your data

- **Claude Code:** `/plugin uninstall jobscout@vishalj0501-plugins` (and
  `/plugin marketplace remove vishalj0501-plugins` to drop the marketplace).
- **Codex:** remove the `jobscout` plugin through Codex's plugin commands.
- **Your data:** delete the local `.jobscout/` directory in any working directory where you used
  Jobscout. That removes all saved profiles and reports.

## Tested hosts

- Claude Code: packaging validated with `claude plugin validate`.
- Codex: see the Codex packaging notes above.

Specific tested host versions will be listed here per tagged release.

## License

MIT — see [LICENSE](./LICENSE).
