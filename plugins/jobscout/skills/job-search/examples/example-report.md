# Jobscout Report — remote-backend-go (EXAMPLE — fictional data)

> This file is an illustrative example with entirely invented companies, URLs, and listings. It is
> not a real search result. Do not treat any link here as a live posting.

- **Search date:** 2026-05-30
- **Profile used:** .jobscout/profile.md
- **Search refinements:** Remote only; backend / Go roles preferred
- **Verification policy:** Each ranked listing was opened and confirmed active on the search date; leads that could not be opened were moved to Needs Verification.

## Ranked matches (all verified matches that passed the filters)

| # | Title | Company | Location | Arrangement | Score | Checked |
| - | ----- | ------- | -------- | ----------- | ----: | ------- |
| 1 | Senior Backend Engineer | Northwind Labs (fictional) | Remote (India) | Remote | 89 | 2026-05-30 |
| 2 | Platform Engineer | Acme Streams (fictional) | Remote (Global) | Remote | 81 | 2026-05-30 |

### 1. Senior Backend Engineer — Northwind Labs (fictional)
- **Location:** Remote (India)
- **Work arrangement:** Remote
- **Source URL:** https://careers.example-northwind.test/jobs/senior-backend-engineer
- **Checked date:** 2026-05-30
- **Score:** 89 / 100
- **Fit rationale:** Strong title match and explicit Go + PostgreSQL + distributed-systems
  requirements (role/skills 38/40); fully remote within India (location/eligibility 24/25); senior
  scope matches (seniority 14/15); listing posted 6 days ago and page loads cleanly (freshness 9/10).
- **Gaps or risks:** Compensation not stated (unknown, not penalized — profile does not make comp a
  must-have). Mentions occasional on-call.
- **Recommended next action:** Apply directly; tailor resume to emphasize distributed-systems work.

### 2. Platform Engineer — Acme Streams (fictional)
- **Location:** Remote (Global)
- **Work arrangement:** Remote
- **Source URL:** https://jobs.example-acmestreams.test/platform-engineer
- **Checked date:** 2026-05-30
- **Score:** 81 / 100
- **Fit rationale:** Adjacent-title match with Kubernetes + Go (role/skills 32/40); global remote
  fits (location/eligibility 23/25); level fits (seniority 13/15); posting recent (freshness 8/10).
- **Gaps or risks:** Heavier infra/ops focus than pure backend; comp range not published.
- **Recommended next action:** Confirm the split between platform and product backend work before applying.

## Needs Verification

- Backend Engineer — Globex Pay (fictional) — https://example-globexpay.test/careers/be — listing
  page returned a login wall, so active status could not be confirmed.
- Go Developer — Initech Cloud (fictional) — https://example-initech.test/jobs/go-dev — posting date
  not shown and page may be cached; could not confirm it is still open.

## Rejected categories

- Removed 3 onsite-only roles (must-have: fully remote).
- Removed 1 role requiring relocation to another country (must-have: no relocation).
