# Jobscout Report Template

This is the structure for a saved search report at
`.jobscout/reports/YYYY-MM-DD-HHMM-<search-slug>.md`. The `HHMM` (24-hour, local) keeps multiple
searches on the same day from colliding. Use **absolute dates** everywhere, because job availability
changes quickly.

```markdown
# Jobscout Report — <search-slug>

- **Search date:** YYYY-MM-DD
- **Profile used:** .jobscout/profile.md
- **Search refinements:** e.g. Remote only; fintech preferred (none if not applicable)
- **Verification policy:** e.g. Each ranked listing was opened and confirmed active on the search date.

## Ranked matches (all verified matches that passed the filters)

| # | Title | Company | Location | Arrangement | Score | Checked |
| - | ----- | ------- | -------- | ----------- | ----: | ------- |
| 1 | ...   | ...     | ...      | Remote      |    87 | YYYY-MM-DD |

### 1. <Title> — <Company>
- **Location:** ...
- **Work arrangement:** Remote / Hybrid / Onsite
- **Source URL:** https://...
- **Checked date:** YYYY-MM-DD
- **Score:** 87 / 100
- **Fit rationale:** Why this matches the profile, by rubric dimension.
- **Gaps or risks:** What is missing, uncertain, or potentially disqualifying.
- **Recommended next action:** e.g. Apply directly; tailor resume to X; confirm Y.

(Repeat one detail block per ranked job.)

## Needs Verification

Leads that look promising but could not be confirmed as active (inaccessible page, possibly closed,
stale, or insufficiently verified). These are **not** ranked.

- <Title> — <Company> — <URL> — reason it could not be verified.

## Rejected categories

A short summary of what strict profile constraints eliminated, e.g.:
- Removed 4 onsite-only roles (must-have: fully remote).
- Removed 2 roles requiring relocation.
```

## Rules

- There is no fixed cap — list every verified match that passed the filters, ordered by score. Aim
  for at least 20 verified ranked matches; if fewer genuinely exist, say so rather than padding.
- Every ranked job must include a source URL and an absolute checked date.
- On a repeat search, mark any listing that appeared in a prior report as previously seen, so the
  user can tell what is new.
- A listing whose source returned a blocked/failed fetch (403, login wall, timeout) belongs in
  **Needs Verification**, never the ranked table.
- Confirm each ranked role's employment type (exclude internships when the profile excludes them) and
  note company-stage fit.
- Keep unverifiable leads out of the ranked table and inside **Needs Verification**.
- If strict constraints removed results, summarize the rejected categories so the user understands
  coverage.
