# Team brief for the review and requirements run (9 November 2026)

Every agent in this run reads this file first.

## Project facts (confirmed by the team)

- Repository: `ommhungry/f-abdul`. Local clone: `the local clone`, branch `jui`.
- This is a **university senior software project** by three students. Each member gives **3–4 hours a week**.
- Proposal due **22 November 2026**, with architecture and overall design finalized. Project ends **30 April 2027**.
- The team is now focused on the **software** deliverable: a small evaluated prototype, not a company launch. Business material is still useful as context and motivation, but requirements and design matter most.
- The product is a LINE-based AI personal assistant for Thailand that goes beyond Abdul (abdul.chat). Abdul mainly reminds (เตือน); we want an assistant that also acts.

## Three member contributions to validate and combine

| Member | Branch | Where the files are | Direction |
|---|---|---|---|
| omm | `main` | `README.md`, `docs/README.md`, `docs/research/business-and-architecture-brief.md`, `docs/governance/project-charter.md`, `docs/planning/timeline-and-development-phases.md`, `docs/meetings/2026-11-07-kickoff.md` | Obligations assistant for working adults in Bangkok (bills, rent, documents, renewals, appointments) with family coordination; booking only where merchant access is dependable; governance, timeline, and architecture. |
| vj | `vj` | copied to `docs/review/source-vj/` | Booking assistant: a booking router (API, web form, LINE handoff, email, phone/voice AI) with a venue channel database as the moat; concierge MVP; startup evaluation. |
| jui | `jui` | `docs/research/ai-assistant-jui/` | "TumHai" broad assistant: 10 actions at 3 levels (does the task itself / sends a request card the business answers by tap / direct booking for opted-in businesses); the user pays; university students and staff first. |

## Rules for every agent

1. Write files in clear, plain English prose. Short sentences. Define jargon on first use.
2. Every factual claim carries a source and an evidence tag: `[V]` verified from a primary source you opened, `[S]` secondary source, `UNVERIFIED` not checked. Give access dates for web sources. Estimates are labelled as estimates.
3. Never edit the files on `main` or in `docs/review/source-vj/`. Write revised copies and reports to the paths you are given.
4. Do not commit, push, or change git branches. The manager integrates.
5. No real personal data in any file. Do not name internal agent IDs.
6. Write only inside your assigned output paths. If you need something outside them, say so in your final report.
7. Final report to the manager: files written, the 5–10 most important findings, and anything you could not verify.

## Folder layout

| Path | Contents |
|---|---|
| `docs/review/` | validation reports per member |
| `docs/review/revised/omm/`, `docs/review/revised/vj/` | revised copies of the members' documents |
| `docs/review/judge/` | the judge's verdicts |
| `docs/research/abdul-recon/` | the deeper Abdul teardown |
| `docs/requirements/` | requirement-type research and the requirements specification |
| `docs/combined/` | the combined proposal |

## Abdul research boundary (applies to every agent)

Abdul's Terms of Service forbid users to "reverse-engineer, disrupt, scrape, probe, or exploit Abdul or the systems behind it". We respect this. Allowed: passive observation of public information only (DNS and certificate-transparency records, response headers and HTML of public marketing pages fetched at a normal browsing rate, robots.txt and sitemap, Wayback Machine snapshots, the public LINE OA profile, help pages, blog, app listings, social posts, news, job ads, interviews, and the technology fingerprint visible in public pages). Not allowed: signing in to app.abdul.chat, intercepting app or LIFF traffic, proxies, decompiling or de-minifying their code, port or directory scanning, subdomain brute force, calling non-public endpoints, or automating chats with the bot.
