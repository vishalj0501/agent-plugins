# Jobscout Profile Template

This is the structure for `.jobscout/profile.md`. It is human-readable Markdown so the user can
review and edit it directly. Fill every section. Mark anything the user did not supply as
`*unspecified*` — never convert an unstated preference into a hard constraint.

```markdown
# Jobscout Profile

- **Created / last updated:** YYYY-MM-DD

## Target roles
- **Target titles:** e.g. Backend Engineer, Platform Engineer
- **Acceptable adjacent titles:** e.g. Software Engineer (Infrastructure), SRE

## Level and type
- **Seniority:** e.g. Mid / Senior
- **Employment type:** e.g. Full-time, Contract, Freelance (list all the user accepts)
- **Internships acceptable:** No (default) / Yes — when No, this becomes a strict must-have.

## Company stage
- **Preference:** Startups only / Established (corporate) only / No preference
- *Ask this explicitly during onboarding — it materially changes which listings fit.*

## Skills
- **Core skills:** e.g. Go, PostgreSQL, distributed systems
- **Secondary skills:** e.g. Kubernetes, gRPC

## Industries and domains
- **Target industries / domains:** e.g. Fintech, Developer tools
- *unspecified if none given*

## Location and work arrangement
- **Allowed locations:** e.g. Bengaluru, Hyderabad, UAE (onsite/hybrid OK)
- **Remote / hybrid / onsite policy:** e.g. Onsite or hybrid in the allowed cities; fully remote also fine
- **Remote eligibility region(s):** regions the user can legally work remotely from, used to scope
  the remote search instead of enumerating every country — e.g. India, APAC, EMEA, Global. Remote
  roles locked to a region the user is not eligible for are rejected.

## Eligibility
- **Work authorization:** *unspecified unless supplied*
- **Sponsorship needed:** *unspecified unless supplied*
- **Relocation:** *unspecified unless supplied*

## Compensation
- **Expectations:** *unspecified unless supplied*

## Exclusions
- **Excluded titles / employers / industries / keywords:** e.g. No on-call-heavy roles, no crypto

## Must-have filters (strict — a listing that fails any of these is rejected before scoring)
- e.g. Must be fully remote
- e.g. Must not require relocation
- e.g. No internships (include this by default unless the user accepts internships)
- e.g. Startups only / Established companies only (include only if the user expressed a hard stage requirement)

## Resume source
- **Resume used:** Yes / No
- **Date provided:** YYYY-MM-DD (if a resume was used)
- *Record only that a resume informed this profile and when — never store the resume contents here.*

## Notes
- Optional free-text notes from the resume or search strategy.
```

## Rules

- Always stamp the creation / update date as an absolute date.
- Keep must-have filters and exclusions distinct: must-haves reject before scoring; exclusions are
  applied the same way but describe categories the user never wants to see.
- If the user later asks to change the profile, update this file and refresh the date.
