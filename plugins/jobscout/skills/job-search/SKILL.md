---
name: job-search
description: Set up a reusable job-search profile, then find, verify, rank, and save current public work opportunities — full-time, contract, or freelance. Use when the user wants to configure their job search, build a Jobscout profile from a resume and preferences, find jobs or contract/freelance gigs that match their profile, run a new job search, or save a ranked shortlist of verified openings. Triggers on requests like "set up my Jobscout profile", "help me configure my job search", "find jobs for me", "find remote backend roles that match my profile", "find freelance/contract work for me", or "run a new job search and save the results".
---

# Jobscout: Job Search

Jobscout helps a person build a reusable private search profile, find current public work
opportunities — full-time, contract, and freelance — verify each listing before recommending it,
rank matches, and save dated shortlists locally. It does not log into job boards, submit
applications, or track application status.

## Data home and privacy

All personal data lives in the **invoking user's current working directory**, never inside the
plugin:

```
<working-directory>/
└── .jobscout/
    ├── profile.md
    └── reports/
        └── YYYY-MM-DD-HHMM-<search-slug>.md
```

Before writing any personal file, ensure `.jobscout/` is ignored by Git when a repository is
present:

- If a `.gitignore` exists and does not already ignore `.jobscout/`, add it.
- If you cannot safely update `.gitignore`, stop and either ask the user for approval or tell them
  the exact line to add (`​.jobscout/`) before writing the profile.

Never write personal files under `plugins/jobscout/`. Never commit a real resume, profile, report,
credential, or private contact information. Use only invented data in examples and documentation.

## Workflow 1 — Onboard me

Trigger: "set up my Jobscout profile", "help me configure my job search", "use my resume to build a
profile".

1. **Start with the resume.** Ask the user whether they have a resume to use, and accept it either
   as pasted text or as a file in their workspace (e.g. `resume.pdf`, `resume.md`). If they provide
   one, read it and extract as many profile facts as possible. Extract *facts* only — never copy the
   raw resume into `.jobscout/` or any tracked plugin content.
2. Fill the rest by asking. Cover any fields the resume did not supply: target job titles and
   acceptable adjacent titles, core and secondary skills, seniority, employment type, preferred
   industries, allowed locations and remote/hybrid/onsite policy, work-authorization and
   sponsorship/relocation constraints, compensation expectations, excluded roles/companies/keywords,
   and strict must-have filters. Two questions you must ask explicitly because they change results
   significantly:
   - **Company stage:** startups only, established/large companies (corporate) only, or no
     preference? Record the answer; it materially changes which listings are a good fit.
   - **Employment type:** confirm which of full-time, contract, and freelance the user will accept,
     and whether internships are acceptable. By default, **internships are excluded** unless the
     user explicitly says they want them — record "no internships" as a strict must-have when they
     are excluded. Record the accepted employment types so freelance/contract roles are included
     when the user wants them.
3. Tell the user the output is private local data **before** writing it.
4. Ensure `.jobscout/` is git-ignored (see "Data home and privacy" above).
5. Write a complete profile to `.jobscout/profile.md` using the structure in
   `references/profile-template.md`. Record whether a resume was used in the profile's "Resume
   source" note (the fact and date only, never the resume contents). Mark any missing optional field
   as *unspecified* — never infer an unstated preference as a hard constraint.
6. Summarize the active constraints back to the user and ask them to correct any inaccurate
   extracted facts — especially anything pulled automatically from the resume.

## Workflow 2 — Find jobs for me

Trigger: "find jobs for me", "find remote backend roles that match my profile", "run a new job
search and save the results".

1. Load `.jobscout/profile.md`. If it does not exist, run **Onboard me** first.
2. **Check prior reports for novelty.** Read existing files in `.jobscout/reports/` and collect the
   source URLs and title+company pairs already surfaced. On a repeat search, prefer listings not
   already reported; if a strong match was reported before and is still open, you may include it but
   mark it as previously seen so the user can tell what is new.
3. Accept temporary refinements for this search (e.g. "only remote", "only fintech") without
   overwriting the saved profile, unless the user explicitly asks to update it.
4. **Cover the whole profile.** Search across **all** of the profile's allowed locations and
   acceptable adjacent titles, and respect every accepted employment type (full-time, contract, and
   freelance when the user accepts them) — do not narrow to a single location or title unless the
   user added a refinement that does so.
5. Search public web sources, preferring ones that can actually be opened and verified. The goal is
   to find **paid work**, not only full-time job postings — when the profile accepts contract or
   freelance, actively look for gigs and people seeking contractors, not just permanent roles.
   - **Prefer:** direct employer career pages and ATS-hosted postings (Greenhouse, Lever, Ashby,
     Workable, SmartRecruiters, Recruitee), which are normally fetchable. Include their
     contract/contractor postings, not just full-time ones.
   - **Contract / freelance sources** (search these when the profile accepts contract or freelance):
     company "contract"/"contractor" roles, agency and consultancy openings, and publicly readable
     "who's hiring" / "seeking a freelancer" listings (e.g. Hacker News "Who is hiring" / "Freelancer
     seeking work" threads, public community job boards). Verify each on a readable page before
     ranking it.
   - **Public hiring communities:** publicly readable community boards where people post work they
     need done — e.g. Reddit `r/forhire`, `r/jobbit`, `r/hiring`, and similar. Treat an individual
     post as a lead only if the post itself is public and readable; capture the post URL and date.
     Do **not** collect a poster's personal contact details, and do **not** draft or send outreach —
     Jobscout finds the opportunity and stops there.
   - **Deprioritize (discovery hints only):** aggregators and marketplaces that routinely block
     automated fetches or sit behind login walls (LinkedIn, Wellfound, RemoteRocketship, Glassdoor,
     Indeed, Upwork, Toptal, Fiverr), and social posts on X/LinkedIn that only surface through web
     search. Use these to discover leads, but if a lead can only be confirmed on a blocked or
     login-walled page, route it to **Needs Verification** rather than ranking it.
6. Open source pages where possible and confirm each candidate listing still represents an active,
   available posting. Capture the **source URL** and the **absolute verification date** for each.
   While the page is open, also verify the **employment type and company stage**: confirm the role
   is not an internship when internships are excluded, and note whether the employer matches the
   profile's company-stage preference (startup vs corporate).
7. **Treat a blocked or failed fetch (e.g. 403, login wall, timeout) as "could not verify."** Do not
   retry it repeatedly and do not promote it into the ranked table — move it to **Needs
   Verification** with the reason. A 403 is not a search failure; keep going.
8. Remove any listing that violates a strict must-have constraint (before scoring). This includes
   internships when they are excluded — a role you cannot confirm is non-internship must go to
   **Needs Verification**, not the ranked table.
9. Rank **all** verified matches that pass the filters using `references/scoring-rubric.md`. There
   is no fixed cap on the number of ranked results; include every listing you could verify and that
   passed the must-have filters, ordered by score. **Aim for at least 20 verified ranked matches** —
   broaden across the profile's locations, adjacent titles, and accepted employment types until you
   reach that target or genuinely exhaust available verified postings. If fewer than 20 verified
   matches exist, say so plainly rather than padding with unverified leads.
10. Put promising-but-unconfirmed leads (inaccessible, closed, stale, or insufficiently verified) in
   a separate **Needs Verification** section — never present them as active ranked matches.
11. Save the report to `.jobscout/reports/YYYY-MM-DD-HHMM-<search-slug>.md` using
   `references/report-template.md`. Include the time (`HHMM`, 24-hour, local) so multiple searches on
   the same day do not collide. Use absolute dates throughout, because availability changes fast.

## Hard guardrail: no fabrication

If the active session cannot perform live web research, say plainly that current verified job
results cannot be produced in this session. Do **not** invent postings, URLs, verification status,
dates, or scores. Honesty about an unverifiable listing always beats a confident guess.

## References

- `references/profile-template.md` — the saved profile structure.
- `references/scoring-rubric.md` — the 100-point ranking rubric.
- `references/report-template.md` — the saved search-report structure.
- `examples/example-report.md` — a complete report built from invented data.
