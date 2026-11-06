# LINE Booking Assistant (working title)

A LINE bot that acts as a personal assistant (like Abdul) and also books appointments for you: restaurants, tennis courts, salons, spas. You text the bot; it recommends places and makes the booking through whichever channel the venue accepts.

## Status
Idea stage. No code yet. Stack not chosen.

## Core idea
- Starting market: Thailand
- Interface: LINE
- Booking router picks the best channel per venue: API -> web form -> LINE handoff -> email -> phone (voice AI as fallback)
- A venue database records which channel each venue accepts and how reliably; this is the main moat
- Recommendations factor in taste, budget, location, and bookability

## Folders
- `docs/` - evaluation, open questions
- `docs/specs/` - written designs (to come)
- `research/` - competitor notes, venue channel data, API/terms research
