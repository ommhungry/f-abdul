# LINE Booking Assistant (working title)

> **Revision notes (9 Nov 2026)**. Every change from vj's original:
> - Status: added "Team: 3 students, 3-4 h a week each" (TEAM-BRIEF).
> - Core idea, starting market: added the 54M MAU figure with its source.
> - Router: the order is unchanged (API → web form → LINE handoff → email → phone). Each step is now annotated with its current feasibility:
>   - API is rare.
>   - The LINE handoff is a user-tap link.
>   - Phone is done by a human in Phase 1, with voice AI later.
>   - The steps are now a sub-list.
> - Venue database: "this is the main moat" became "a *possible* moat, slow to build". Added the last-verified date and the data-source terms that prevent seeding from Google or Wongnai.
> - Tags: [V] primary source we opened; [S] secondary source; [I] our inference, including inference from primary docs; UNVERIFIED. Evidence: `docs/review/validation-vj.md`. Sources accessed 9 Nov 2026.

A LINE bot that acts as a personal assistant (like Abdul) and also books appointments for you: restaurants, tennis courts, salons, spas. You text the bot; it recommends places and makes the booking through whichever channel the venue accepts.

## Status
Idea stage. No code yet. Stack not chosen. Team: 3 students, 3-4 h a week each.

## Core idea
- Starting market: Thailand. LINE has 54M monthly active users there (31 Mar 2026) [V, lycorp.co.jp/en/company/global].
- Interface: LINE
- Booking router picks the best channel per venue, in this order:
  - partner API: rare. No open Thai booking API was found; Chope, Fresha and Google Reserve are partner-only or inbound-only (see validation-vj.md #3).
  - web form: browser agent; brittle.
  - LINE handoff: the bot opens the shop's OA chat with the text pre-filled, and the user taps Send [V, developers.line.biz URL scheme docs].
  - email
  - phone: a human in Phase 1; voice AI later.
- A venue database records which channel each venue accepts, how reliably, and when it was last verified. It is a *possible* moat, slow to build: it cannot be seeded by copying Google Places or Wongnai data [V, Google Maps Platform ToS s.3.2.3; V, wongnai.com/terms, user obligations item (3)].
- Recommendations factor in taste, budget, location, and bookability

## Folders
- `docs/` - evaluation, open questions
- `docs/specs/` - written designs (to come)
- `research/` - competitor notes, venue channel data, API/terms research
