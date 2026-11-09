# Startup Evaluation (10 criteria + 4 extras)

> **Revision notes (9 Nov 2026)**. Every change from vj's original:
> - Title: "5 extras" became "4 extras". The original lists only 4 unconventional criteria.
> - Stage line: removed "team details" (the team is now known).
> - Row 1: added the 54M MAU figure and the restaurant walk-in caveat.
> - Row 2: "? / None" became "2 / Medium", with the team facts.
> - Row 3: added the voice-cost estimate and the LINE Reply API charge.
> - Row 4: confidence "Medium" became "Medium-High"; named competitors added.
> - Row 5: the OA-to-OA limit tagged [I]; added the data-source terms and the missing telephone-audio benchmark.
> - Row 8: score "2" became "3 (Phase 1) / 2 (with voice)"; confidence "Low" became "Low-Medium"; notes rewritten with the status of each legal item.
> - Row 9: added the Grab-Chope and LMWN-JERA precedents; dropped "travel" and "lifestyle apps" from the acquirer list.
> - Row 10: added the LINE Reply API pricing risk.
> - Unconventional criteria: added the LINE Pioneer Program to "mentors/networks"; added "(Wizard-of-Oz)" to customer feedback.
> - Valuation today: the investor sentence marked as unsourced opinion. The metric list was rewritten as project evaluation metrics: "Paying users, 50-100" became "users who completed a booking, 15-30"; ">90%" became "with confidence interval, split by channel"; "cost per booking versus revenue" became "cost and time per booking".
> - Unknowns: #4 struck through and answered; #5 added.
> - Context: this is a university senior software project. Business scores are motivation, not the main deliverable.
> - Tags: [V] primary source we opened; [V, opened by jui 8 Nov 2026]; [S] secondary; [I] our inference, including inference from primary docs; UNVERIFIED. Evidence: `docs/review/validation-vj.md`.

Stage: idea only. No product, revenue, or users. Scores are 1-5 and reflect what we know now. "Confidence" is how much evidence backs the score.

| # | Criterion | Score | Confidence | Notes |
|---|---|---|---|---|
| 1 | Market potential | 3 | Low | Large LINE user base: 54M MAU in Thailand, 31 Mar 2026 [V, lycorp.co.jp]. Restaurant booking is weaker than it looks: Thais often walk in [S, e27.co, via jui]. Willingness to pay is unproven, and per-booking value is small. Market size not researched. |
| 2 | Team quality | 2 | Medium | 3 students at 3-4 h a week, project ends 30 Apr 2027 (TEAM-BRIEF). Enough for a prototype, not for a company [I]. |
| 3 | Business model | 2 | Low | Options exist (per-booking fee, subscription, venue commission) but none is chosen or tested. Unit economics are unknown: voice ≈ ฿15-25 a call (estimate) [S/I, via jui]. The LINE Reply API is paid above 25,000 messages a month from 1 Feb 2027 [V, opened by jui 8 Nov 2026]. |
| 4 | Competitive position | 2 | Medium-High | Grab Dine Out "Book Table" (Chope) has 500+ brands in Bangkok and Phuket [V, grab.com 2025]. Wongnai Reservations via Google [S, via jui]. Fresha and GoWabi in beauty [S, via jui]. LINE runs AI phone booking in Japan [V, opened by jui 8 Nov 2026]. Calling alone is not a moat. Potential moat: venue channel database plus user preference history. |
| 5 | Product / technology | 2 | Medium | Voice AI and LLM agents are rentable by anyone. Hard parts: reliability in Thai (no telephone-audio benchmark found [S, arXiv 2601.13044]) and LINE's API limits (cannot message another business's OA [I, from LINE docs]). The venue database cannot be seeded from Google or Wongnai [V, their terms]. |
| 6 | Financial situation | n/a | None | No revenue, costs, or runway data. |
| 7 | Customer base | n/a | None | No users yet. Retention and repeat-booking rate are the most important numbers to get. |
| 8 | Legal / regulatory | 3 (Phase 1) / 2 (with voice) | Low-Medium | Phase 1 has no voice and no payments. Main duties: PDPA (notice, lawful basis, cross-border LLM transfer) [S, via jui] and data-source terms (Google, Wongnai) [V]. With voice: recording is personal data, and consent rules are unsettled [S]. AI disclosure is not yet required by enacted law, but the draft AI Act would add it [S]. |
| 9 | Exit strategy | 3 | Low | Plausible acquirers: LINE-related companies, delivery/booking players, bank or retail groups. Precedents: Grab bought Chope (Jul 2024) [S, techcrunch]; LMWN bought JERA Cloud (Aug 2025) [S, via jui]. No plan yet. |
| 10 | Risk assessment | 2 | Medium | Top risks: failed bookings destroy trust; cost per booking; low willingness to pay; platform dependence on LINE (Reply API pricing from Feb 2027 [V, opened by jui 8 Nov 2026]); easy to copy. |

## Unconventional criteria
- **Cultural fit, mentors/networks:** unknown. Local venue and LINE relationships would help a lot. LINE's agent Pioneer Program / LINE HACKS 2027 is one route [S, thansettakij.com, via jui].
- **Customer feedback:** none yet. Cheapest way to get it is the concierge (Wizard-of-Oz) test.
- **Willingness to change:** strong sign so far (idea already evolved from calling-only to a multi-channel router).
- **Sustainability / social:** minor. Can help small venues fill empty slots.

## Valuation today
No reliable valuation is possible. With no product, users, or revenue, the company is worth roughly the founders' time and the quality of the idea. Standard methods (revenue multiples, DCF, comparables) need numbers that do not exist yet.

Opinion, not sourced [UNVERIFIED]: pre-seed investors in Southeast Asia value teams and early traction, not ideas. For this project the same kinds of numbers serve as evaluation metrics:
1. Users who completed a booking (pilot: 15-30 is realistic; see scope)
2. Booking success rate, with its confidence interval, split into automated / handoff / human
3. Repeat-use rate (users who book again within 30 days)
4. Cost and time per successful booking
5. Venue channel database size and verified-accuracy rate

## Biggest unknowns to resolve first
1. Will Thai users pay, and how much?
2. What does one successful booking actually cost to fulfil?
3. How often can the bot book without a phone call?
4. ~~Who is on the team?~~ Answered: 3 students, 3-4 h a week.
5. **New:** how often does a LINE handoff end in a confirmed booking, and how does the bot find out?
