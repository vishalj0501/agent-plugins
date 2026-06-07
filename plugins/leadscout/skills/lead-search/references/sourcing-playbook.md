# LeadScout Sourcing Playbook

This reference tells the skill **where on the public web each kind of person is found**, **how to
search for them**, **what their public contact channel usually is**, and **how to verify it**. It is
the operational core of LeadScout. Follow the universal rules first, then the per-bucket playbooks.

## Universal Rules

These apply to every bucket and override any bucket-specific tactic.

1. **Public, intentional, live.** A person enters the ranked list only when at least one contact
   channel is (a) publicly visible without a login, (b) intentionally published by the person for
   professional contact, and (c) currently live. If you cannot confirm all three, the person goes to
   `Identified, No Public Channel`.
2. **People, not agencies.** Confirm each entry is an individual human, not a company, staffing
   agency, or "talent@agency" alias. Agencies and companies are excluded.
3. **Never fabricate a channel.** Do not guess emails (`first.last@company.com`), do not infer from
   patterns, do not use people-data brokers (Apollo, Hunter, RocketReach, ZoomInfo), and do not read
   or extract data from behind a login wall (including LinkedIn member data). A profile URL is a
   public identifier and may be recorded; private data reached *through* that profile may not.
4. **Record provenance.** For every channel, capture the exact source URL where it was found and an
   absolute checked date. "Found on" is part of the record, not optional.
5. **Prefer fetchable sources.** Favor pages a host can actually open and read — personal sites,
   GitHub, company team pages, ATS postings, Reddit, Hacker News, dev.to, conference sites — over
   login-walled aggregators (LinkedIn feed, Wellfound, Upwork). Aggregators are *discovery hints*
   only; confirm the channel on a readable page.
6. **Honor opt-out.** Skip anyone who signals they do not want contact (see "Opt-Out Signals").
7. **Distinguish person channel from role inbox.** `careers@company.com` or `jobs@company.com` is a
   company channel, not a person's. It may be recorded, but label it as a role inbox and do not
   attribute it to an individual as their personal email.

### Channel Quality Ladder (used by the Reachability score)

From strongest to weakest:

1. A person-authored post that explicitly invites contact with a method ("email me at x@y.com",
   "DM me here") — the post *is* the published intent. Strongest.
2. A public email the person lists on a page they control (personal site, GitHub profile,
   conference bio).
3. A public, open DM channel (X with open DMs, a public "contact me" form).
4. A public LinkedIn / X **profile URL** with no published email — the user can reach out, but
   through the platform. Weakest, and the common case for recruiters and managers.

## Bucket 1 — Recruiters / HR / Talent

**Realistic yield: moderate.** Recruiters are often *named* publicly, but their real channel is
usually LinkedIn (profile URL only). Expect many "profile-only" records.

### Where they are public

- **Inside job postings.** Greenhouse, Lever, Ashby, and Workable postings sometimes name a recruiter
  or "hiring contact"; company career pages occasionally list a talent partner per team.
- **Company team / people / about pages** that list Talent, People, or HR staff.
- **"Meet the team" and careers blog posts** crediting the talent team.
- **Public hiring posts** in communities where a recruiter signs with their own name and sometimes an
  email or Calendly link.
- **Conference / meetup recruiting** pages and "we're hiring" community threads.

### How to search

- `site:linkedin.com/in "technical recruiter" "<Company>"` → public profile URL (record the URL only;
  do not attempt to extract gated data).
- `"<Company>" ("talent" OR "recruiter") (site:greenhouse.io OR site:lever.co OR site:ashbyhq.com)`
- `"<Company>" careers team "recruiter"` then open the company page directly.
- `site:x.com ("recruiter" OR "talent") "<industry/skill>"` for recruiters who keep a public bio.

### Typical channel and verification

- Usually a **LinkedIn profile URL**; sometimes a published recruiting email or a Calendly in a post;
  occasionally a personal email in a signed community post.
- Verify the named person actually holds a talent/HR role at the named company (cross-check the
  posting or team page). If the only "contact" is `careers@`, record it as a **role inbox**, not the
  recruiter's personal channel.

### Pitfalls

- Staffing-agency recruiters are common in search results — confirm they are in-house at a target
  company before listing, or exclude if the user only wants in-house contacts.
- Do not treat a generic `careers@` as reaching a specific person.

## Bucket 2 — Hiring Managers

**Realistic yield: thin.** Hiring managers are rarely publicly tied to a specific open role *with* a
public contact. Treat strong finds as a bonus, and route the rest to `Identified, No Public Channel`.
Two sources punch far above the rest.

### Where they are public

- **Hacker News "Who is hiring?" monthly thread.** Managers post their own roles and frequently
  include "email me at …". This is the single richest public source for manager contacts. Each top-
  level comment is usually one company/team with a real contact.
- **Engineering / company blogs.** The author of "how our platform team works" is often the manager;
  bios may link a personal site, X, or email.
- **Public team pages** naming a team lead, EM, or "Head of <function>".
- **Conference talks and podcasts** where the speaker/guest is an EM/Director in the user's field;
  talk and episode pages list their socials.
- **"We're hiring on my team" posts** on X, LinkedIn, or Mastodon authored by the manager.
- **GitHub org READMEs and maintainer lists** where a lead is identifiable.

### How to search

- `site:news.ycombinator.com "Ask HN: Who is hiring" <month> <year>` then scan within for the user's
  stack/role; capture the manager's own posted email.
- `"<Company>" ("engineering manager" OR "head of" OR "team lead") (blog OR engineering)`
- Conference speaker pages: `"<conference>" speakers "<topic>"`.
- `site:x.com "hiring on my team" "<skill>"`.

### Typical channel and verification

- HN who-is-hiring posts: often a **published email** — strongest channel. Confirm the comment is in
  the current/recent month's thread (freshness) and that the poster describes themselves as the
  hiring side.
- Blogs/talks/team pages: usually a **profile URL**, sometimes a personal site with email.
- Confirm the person is plausibly the decision-maker for relevant roles, not just an IC sharing a
  posting.

## Bucket 3 — Freelance Buyers / Clients

**Realistic yield: strong.** These people post *specifically to be contacted*, so the post itself is
the published channel. This is usually LeadScout's most productive bucket for a freelance goal.

### Where they are public

- **Reddit hiring subs:** `r/forhire` (filter for `[Hiring]` flair — `[For Hire]` is the opposite
  side), `r/jobbit`, `r/hiring`, `r/freelance`, and niche subs (`r/gamedev`, `r/web_design`,
  `r/SaaS`, `r/DevelEire`, etc.). Posts typically say "DM me" or "email me at …".
- **Hacker News monthly "Freelancer? Seeking Freelancer?" thread.** The "SEEKING FREELANCER" entries
  are buyers, usually with a contact method inline.
- **Indie Hackers, dev.to, and community forums** with "looking for / need to hire" posts.
- **Public X / Bluesky posts:** "looking to hire a freelance <skill>", "need a contractor for …,
  DM me".
- **Publicly readable Slack/Discord job channels** with public archives or web mirrors.
- **Public briefs / RFP posts** from individuals (not procurement portals).

### How to search

- `site:reddit.com/r/forhire "[Hiring]" "<skill>"` (and repeat for `r/jobbit`, `r/hiring`).
- `site:news.ycombinator.com "Freelancer? Seeking Freelancer?" <month> <year>` then find "SEEKING
  FREELANCER" entries.
- `"looking for a freelance <skill>" ("DM" OR "email" OR "reach out")`.
- `site:x.com ("hiring a freelancer" OR "need a contractor") "<skill>"`.

### Typical channel and verification

- Usually an **explicit in-post contact**: "email me at …" or "DM me" — record it as written and note
  the post URL and date. Open-DM invitations count as a public channel.
- **Freshness matters most here.** A `[Hiring]` post from months ago is likely filled — weight recent
  posts and note the post date.

### Pitfalls

- **Agencies and resellers** fish in these channels — exclude anything that is a company looking to
  subcontract under an agency brand, unless the user wants those.
- Watch for scams / unrealistic offers; note risk in the rationale but it is the user's call.
- `[For Hire]` posts are *sellers*, not buyers — skip them for this bucket.

## Bucket 4 — Peers In Field

**Realistic yield: strong**, and the best bucket for actually finding a **public email**. People
building in public list their contacts on purpose. Most relevant when the goal is networking, but
peers also surface warm intros toward jobs and clients.

### Where they are public

- **GitHub** maintainers and notable contributors of OSS in the user's domain; profiles commonly list
  email, X, and a personal site, all intentionally public.
- **Personal technical blogs** and **dev.to / Medium / Substack / Hashnode** authors with contact in
  their bio.
- **Conference speakers** and **podcast hosts/guests** in the field; talk/episode pages list socials.
- **Newsletter authors** in the niche.
- **Active practitioners on X, Mastodon, and Bluesky** in the domain.
- **Stack Overflow / community top contributors** whose profiles link out.

### How to search

- GitHub by topic/language: find a relevant repo, then open the maintainer's **profile page** (not
  gated data) for their published links.
- `site:dev.to "<topic>"` / `site:hashnode.* "<topic>"` → author bios.
- `"<topic>" conference speaker 2025 OR 2026` → speaker pages with socials.
- `site:github.com <topic>` then profile; many list email openly under their name.

### Typical channel and verification

- Frequently a **published email plus X and personal site** — record all public channels found and
  pick the best for the reachability note.
- Confirm the links are the person's own and currently live. A GitHub profile email shown publicly is
  a published channel; an email reached only by un-gating private data is not.

### Pitfalls

- Distinguish a person from an org account (e.g. a project's bot or company GitHub org).
- For a networking goal, weight influence/connectedness; for a job/client goal, weight whether the
  peer plausibly bridges to a decision-maker.

## Channel Verification Methodology

Apply per channel type before recording:

- **LinkedIn profile URL** — Confirm it resolves to an individual's public profile and matches the
  person's name and role from another source. Record the **URL only**. Do not log in, do not read
  gated profile fields, do not extract an email through the profile.
- **X (or Mastodon / Bluesky) profile URL** — Confirm the handle belongs to the person and the
  account is public and reasonably active. Note if DMs are openly invited. Record the URL.
- **Email** — Record only if it appears on a page the person controls or in a post they authored,
  presented as a way to contact them. Capture the exact source. Mark whether it is a **personal
  channel** or a **role inbox** (`careers@`, `hello@`). Never construct, complete, or guess an
  address.
- **Liveness** — The source page must currently load; the post must not be visibly expired, deleted,
  or marked filled. For time-sensitive buckets (freelance, who-is-hiring), record the post date and
  prefer recent ones.
- **Blocked fetch** — A 403, login wall, CAPTCHA, or timeout means *could not verify*. Do not retry
  repeatedly; route the person to `Identified, No Public Channel` with a note on why.

## Opt-Out Signals (Exclude These People)

Skip a person, regardless of bucket, when you see:

- "No recruiters", "no cold outreach", "please don't DM", "no AI/automated messages".
- "Not looking", "not open to work", "happily employed", "closed to new clients".
- A deleted, locked, or "position filled / found someone" post.
- Any user-defined opt-out or etiquette rule from `.leadscout/profile.md`.
- Minors, or anyone whose public presence indicates they are not engaging professionally.

## Yield Expectations (Set Honest Targets)

| Bucket | Public-channel yield | Most common channel | Best single source |
| --- | --- | --- | --- |
| Recruiters / HR | Moderate | LinkedIn profile URL | Named contacts in postings / team pages |
| Hiring managers | Thin | Profile URL; sometimes email | HN "Who is hiring?" thread |
| Freelance buyers | Strong | In-post "DM me / email me" | `r/forhire` `[Hiring]`, HN freelancer thread |
| Peers in field | Strong | Published email + X + site | GitHub profiles, personal blogs |

When the recruiter and manager buckets come up thin, broaden the freelance-buyer and peer buckets to
reach the run's target count rather than fabricating contacts. Thin buckets are an honest result, not
a failure — report them as such.

## Search-Operator Cookbook

Reusable patterns (substitute `<Company>`, `<skill>`, `<topic>`, `<month>`, `<year>`):

- Public profile by role + company: `site:linkedin.com/in "<role>" "<Company>"`
- Recruiter on ATS posting: `"<Company>" ("recruiter" OR "talent") (site:greenhouse.io OR site:lever.co OR site:ashbyhq.com)`
- HN hiring managers: `site:news.ycombinator.com "Ask HN: Who is hiring" <month> <year>`
- HN freelance buyers: `site:news.ycombinator.com "Freelancer? Seeking Freelancer?" <month> <year>`
- Reddit freelance buyers: `site:reddit.com/r/forhire "[Hiring]" "<skill>"`
- Open invitation to contact: `"<skill>" ("email me at" OR "DM me" OR "reach out to me")`
- Peer authors: `site:dev.to "<topic>"` / `site:github.com "<topic>"` then open the profile page
- Conference speakers: `"<conference>" speakers "<topic>" <year>`

Always open the discovered page to confirm the channel before recording it; a search snippet is a
lead, not a verified contact.
