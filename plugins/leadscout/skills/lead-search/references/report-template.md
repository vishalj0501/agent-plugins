# LeadScout People-List Template

This is the structure for a saved people-list at
`.leadscout/lists/YYYY-MM-DD-HHMM-<search-slug>.md`. The `HHMM` (24-hour, local) keeps multiple
searches on the same day from colliding. Use **absolute dates** everywhere, because public contact
points change quickly.

```markdown
# LeadScout List — <search-slug>

- **Search date:** YYYY-MM-DD
- **Profile used:** .leadscout/profile.md
- **Goal applied:** Job search / Freelance clients / Networking
- **Search refinements:** e.g. Freelance buyers only; devtools domain (none if not applicable)
- **Verification policy:** e.g. Each ranked person had a public contact channel opened and confirmed live on the search date.

## Ranked people (all verified people who passed the gate)

| # | Name | Role | Company / community | Bucket | Best channel | Score | Checked |
| - | ---- | ---- | ------------------- | ------ | ------------ | ----: | ------- |
| 1 | ...  | ...  | ...                 | ...    | Email        |    88 | YYYY-MM-DD |

### 1. <Name> — <Role>, <Company / community>
- **Bucket(s):** Recruiter / Hiring manager / Freelance buyer / Peer
- **LinkedIn:** https://linkedin.com/in/... (URL only) — or "none found"
- **X / social:** https://x.com/... — or "none found"
- **Email (published only):** name@example.com (personal) / careers@example.com (role inbox) — or "none found"
- **Found on:** <source URL where each channel was published> — checked YYYY-MM-DD
- **Why relevant:** How this person connects to the user's goal.
- **Score:** 88 / 100
- **Reachability note:** Best way in, e.g. "Posted 'email me' on HN — use the email; LinkedIn also public."
- **Previously seen:** No / Yes (in <prior list filename>)

(Repeat one detail block per ranked person.)

## Identified, No Public Channel

Relevant people who could not be listed because no public/intentional/live channel was confirmable
(profile-only behind a login wall, blocked fetch, or no published contact). These are **not** ranked.

- <Name> — <Role>, <Company> — relevant because … — reason no usable public channel (e.g. "only LinkedIn, behind login; no public email").

## Excluded

A short summary of who was removed and why, e.g.:
- Removed 3 staffing-agency recruiters (opt-out: in-house only).
- Removed 2 posters who wrote "not looking".
- Removed 1 company/agency account (people only).
```

## Rules

- There is no fixed cap — list every verified person who passed the gate, ordered by score. Aim for
  at least 15; if fewer genuinely exist, say so rather than padding.
- Every ranked person must have at least one public channel with a source URL and an absolute checked
  date showing where it was published.
- Record a LinkedIn/X profile as a URL only; never include data reached by un-gating a private
  profile. Never include a guessed, pattern-inferred, or broker-sourced contact.
- Distinguish a personal email from a role inbox (`careers@`); never attribute a role inbox to an
  individual as their personal channel.
- On a repeat search, mark anyone who appeared in a prior list as previously seen.
- A person whose channel returned a blocked/failed fetch (403, login wall, timeout) belongs in
  **Identified, No Public Channel**, never the ranked table.
- Keep opt-out matches, "not looking" signals, and agency/company entries out of the ranked table and
  summarize them under **Excluded**.
