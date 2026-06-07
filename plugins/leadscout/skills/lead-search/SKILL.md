---
name: lead-search
description: Set up a reusable outreach-target profile, then find, verify, rank, and save real people worth reaching out to — recruiters and HR, hiring managers, freelance buyers, and peers in the user's field. Captures only publicly published contact channels (LinkedIn/X profile URLs and emails the person posted themselves) and never contacts anyone. Use when the user wants to configure who they should reach out to, build a LeadScout profile, find recruiters/hiring managers/freelance clients/peers, or save a ranked list of contactable people. Triggers on requests like "set up my LeadScout profile", "find people I can reach out to", "find recruiters and hiring managers at companies that match my profile", "find people hiring freelancers in my field", or "run a new outreach search and save the results".
---

# LeadScout: Lead Search

LeadScout helps a person find real **people** worth reaching out to — recruiters and HR, hiring
managers, freelance buyers/clients, and peers in their field — verify that each person has a
genuinely public way to be contacted, rank them by how worthwhile the outreach is, and save dated
lists locally. It is the people-finding sibling to Jobscout.

LeadScout collects **only contact channels a person has intentionally published in public** for
professional contact. It does not guess or pattern-infer emails, does not use people-data brokers,
does not scrape behind login walls, and **never contacts anyone** — it produces a list and stops.

## Data home and privacy

All personal data lives in the **invoking user's current working directory**, never inside the
plugin:

```
<working-directory>/
└── .leadscout/
    ├── profile.md
    └── lists/
        └── YYYY-MM-DD-HHMM-<search-slug>.md
```

Before writing any personal file, ensure `.leadscout/` is ignored by Git when a repository is
present:

- If a `.gitignore` exists and does not already ignore `.leadscout/`, add it.
- If you cannot safely update `.gitignore`, stop and either ask the user for approval or tell them
  the exact line to add (`.leadscout/`) before writing the profile.

Never write personal files under `plugins/leadscout/`. Never commit a real resume, profile, generated
people-list, credential, or any third party's contact details. The contact details of the people you
surface are sensitive too — store only the minimal public professional facts needed to decide whether
and how to reach out, keep them local, and never publish them. Use only invented data in examples and
documentation.

## The contact boundary (read before any search)

A person enters the ranked list **only** when at least one contact channel is, all at once:

1. **Public** — visible without a login.
2. **Intentional** — the person published it for professional contact (a "DM me / email me at …"
   post, a personal site, a GitHub profile, a public team page, a conference bio).
3. **Live** — the page currently loads and the post is not expired, deleted, or marked filled.

Allowed channels: a public **LinkedIn profile URL**, a public **X profile URL** (or Mastodon /
Bluesky), and an **email only if the person published it themselves**. Record a LinkedIn/X profile as
a **URL only** — never log in, read gated fields, or extract an email through it.

Never do any of the following:

- Guess or complete an email from a pattern (`first.last@company.com`).
- Use people-data brokers or enrichment APIs (Apollo, Hunter, RocketReach, ZoomInfo).
- Scrape or extract data from behind a login wall.
- Attribute a role inbox (`careers@`, `hello@`) to an individual as their personal channel — record
  it, but label it a role inbox.
- Draft, send, or schedule any outreach. LeadScout lists people and stops.

`references/sourcing-playbook.md` is the operational core: where each kind of person is found, how to
search, and how to verify each channel. Follow it.

## Workflow 1 — Set up my LeadScout profile

Trigger: "set up my LeadScout profile", "help me configure who I should reach out to", "use my
resume and goals to build an outreach profile".

1. **Establish the goal first.** Ask the user's primary outreach goal — landing a job, winning
   freelance/contract clients, or professional networking — because it changes who is relevant and how
   they rank. More than one goal is allowed.
2. **Bootstrap from Jobscout when available.** If `.jobscout/profile.md` exists in the working
   directory, read it to pre-fill shared facts (field, skills, target titles, locations, employment
   types). Do **not** modify the Jobscout profile. Otherwise, gather these by asking, optionally from
   a resume or bio the user pastes or points to in their workspace (extract facts only — never copy
   the raw document into `.leadscout/`).
3. **Pick the people buckets** to include: recruiters/HR, hiring managers, freelance buyers, peers.
   Default to all four unless the user narrows them.
4. **Capture opt-out and etiquette rules** the user wants respected — e.g. "no cold DMs to individual
   contributors", "in-house recruiters only", "no one who posted 'not looking'". Record them.
5. Tell the user the output is private local data **before** writing it.
6. Ensure `.leadscout/` is git-ignored (see "Data home and privacy" above).
7. Write a complete profile to `.leadscout/profile.md` using `references/profile-template.md`. Mark
   any missing optional field as *unspecified* — never infer an unstated preference as a hard rule.
8. Summarize the goal, buckets, and opt-out rules back to the user and ask them to correct any
   inaccurate extracted facts.

## Workflow 2 — Find people for me

Trigger: "find people I can reach out to", "find recruiters and hiring managers at companies that
match my profile", "find people hiring freelancers in my field", "run a new outreach search and save
the results".

1. Load `.leadscout/profile.md`. If it does not exist, run **Set up my LeadScout profile** first.
2. **Check prior lists for novelty.** Read existing files in `.leadscout/lists/` and collect the
   names and source URLs already surfaced. Prefer people not already listed; if you re-surface someone
   still relevant, mark them as previously seen so the user can tell who is new.
3. Accept temporary refinements for this search (e.g. "only freelance buyers", "only at these five
   companies") without overwriting the saved profile, unless the user asks to update it.
4. **Search per included bucket using `references/sourcing-playbook.md`.** Cover each bucket the
   profile includes rather than narrowing to one. Use the playbook's per-bucket sources and search
   operators. Expect the recruiter and hiring-manager buckets to be thinner on public channels and the
   freelance-buyer and peer buckets to be richer — broaden the rich buckets to reach the target count
   rather than fabricating contacts.
5. **For each candidate person, find and verify public contact channels.** Open the source page,
   confirm the channel meets the public/intentional/live test, and capture each channel with the exact
   **source URL** and an **absolute checked date**. Confirm the entry is an individual, not an agency
   or company.
6. **Treat a blocked or failed fetch (403, login wall, CAPTCHA, timeout) as "could not verify."** Do
   not retry repeatedly. Route that person to **Identified, No Public Channel** with the reason.
7. **Exclude** anyone matching an opt-out/etiquette rule or a "not looking"/"no outreach" signal, and
   any agency or company entry (see the playbook's "Opt-Out Signals").
8. Rank **all** verified people using `references/scoring-rubric.md`. There is no fixed cap — include
   every person you could verify, ordered by score. **Aim for at least 15 verified ranked people**;
   broaden across buckets, companies, and communities until you reach that target or genuinely exhaust
   public sources. If fewer exist, say so plainly rather than padding.
9. Put relevant people with **no confirmable public channel** (or only a login-walled one) in a
   separate **Identified, No Public Channel** section — never present them as ranked, contactable
   leads.
10. Save the list to `.leadscout/lists/YYYY-MM-DD-HHMM-<search-slug>.md` using
    `references/report-template.md`. Include the time (`HHMM`, 24-hour, local) so multiple searches on
    the same day do not collide. Use absolute dates throughout, because public contact points change
    fast.

## Hard guardrail: no fabrication

If the active session cannot perform live web research, say plainly that current verified people
results cannot be produced in this session. Do **not** invent people, profiles, emails, URLs,
verification status, dates, or scores. Honesty about an unverifiable lead always beats a confident
guess.

## References

- `references/sourcing-playbook.md` — where each bucket is found, how to search, how to verify a channel.
- `references/profile-template.md` — the saved profile structure.
- `references/scoring-rubric.md` — the 100-point ranking rubric.
- `references/report-template.md` — the saved people-list structure.
- `examples/example-list.md` — a complete people-list built from invented data.
