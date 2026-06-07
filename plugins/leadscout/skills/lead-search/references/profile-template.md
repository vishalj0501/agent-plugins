# LeadScout Profile Template

This is the structure for `.leadscout/profile.md`. It is human-readable Markdown so the user can
review and edit it directly. Fill every section. Mark anything the user did not supply as
`*unspecified*` — never convert an unstated preference into a hard rule.

```markdown
# LeadScout Profile

- **Created / last updated:** YYYY-MM-DD
- **Bootstrapped from Jobscout profile:** Yes / No (date, if Yes)

## Outreach goal
- **Primary goal(s):** Job search / Freelance clients / Networking (list all that apply)
- *Goal drives who is relevant and how people are ranked.*

## Me
- **Field / domain:** e.g. Backend engineering, developer tools
- **Core skills or services offered:** e.g. Go, PostgreSQL, distributed systems / "API and platform contract work"
- **Target titles I'm pursuing:** e.g. Senior Backend Engineer (for a job goal)

## People buckets to include
- [ ] Recruiters / HR / talent
- [ ] Hiring managers
- [ ] Freelance buyers / clients
- [ ] Peers in field
- *Default is all four unless the user narrows them.*

## Targets
- **Target companies:** e.g. Northwind Labs, Acme Streams (*unspecified if none*)
- **Target domains / communities:** e.g. fintech, r/forhire, Go OSS, devtools conferences
- **Locations / regions of interest:** *unspecified unless supplied*

## Opt-out and etiquette rules (respected on every search)
- e.g. In-house recruiters only — no staffing agencies
- e.g. No cold DMs to individual contributors
- e.g. Skip anyone who posted "not looking" / "no recruiters"

## Exclusions
- **Excluded companies / communities / keywords:** *unspecified if none*

## Self-introduction context (for the user's own reference)
- A short note on who the user is and what they want, drawn from a resume or bio.
- *LeadScout does not draft outreach from this in v1 — it is context only.*

## Notes
- Optional free-text notes on outreach strategy.
```

## Rules

- Always stamp the creation / update date as an absolute date.
- The goal and the included buckets are the two facts that most change results — never leave them
  blank; ask if unclear.
- Opt-out rules are enforced on every search and remove people before ranking.
- If `.jobscout/profile.md` was used to pre-fill, record that fact and date — but never modify the
  Jobscout file.
- If the user later asks to change the profile, update this file and refresh the date.
