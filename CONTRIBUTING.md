# Contributing to Jobscout

Thanks for helping improve Jobscout. The plugin's behavior lives in one shared skill
(`plugins/jobscout/skills/job-search/`); the host manifests only handle installation and UI
metadata. Keep that separation when you contribute.

## Proposing changes

- **Workflow or guardrail changes** (onboarding, search, verification, no-fabrication rule): edit
  `skills/job-search/SKILL.md`. Keep it concise and push detail into `references/`.
- **Scoring changes:** edit `references/scoring-rubric.md`. Explain why the dimension weights change.
- **Profile or report format changes:** edit `references/profile-template.md` or
  `references/report-template.md`, and update `examples/example-report.md` to match.
- **Manifest or packaging changes:** edit the host manifest(s). Behavior must not diverge between
  hosts — the shared skill stays the single source of truth. Manifests may differ only where host
  schema or UI metadata requires it.

Open an issue or PR describing the motivation and the user-facing effect.

## Use invented data only

Never include a real resume, candidate profile, job report, credential, access token, or private
contact information in a contribution or issue report. All examples and reproductions must use
clearly fictional data (see `examples/example-report.md`).

## Validation before submitting

**Claude Code**

```shell
claude plugin validate .
claude plugin validate ./plugins/jobscout
```

This checks `marketplace.json`, the plugin manifest, and skill frontmatter.

**Codex**

Validate `plugins/jobscout/.codex-plugin/plugin.json` and the marketplace entry using Codex's
plugin tooling (the `plugin-creator` skill / `codex plugin` commands). Confirm there are no
placeholder, schema, or missing-file errors, and that both hosts discover the same `job-search`
skill.

## Manual check

From a clean test working directory, exercise both workflows end to end:

1. Onboarding creates `.jobscout/profile.md` and ensures `.jobscout/` is git-ignored.
2. A search produces a dated report with no more than ten ranked verified matches, each with a
   source URL, checked date, score, rationale, and gaps.
3. A must-have violation is excluded from the ranking.
4. An unverifiable lead appears under **Needs Verification**, not in the ranked table.
