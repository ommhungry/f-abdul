# Scope and Objectives

> **Revision notes (9 Nov 2026)**. Every change from vj's original:
> - Status line: "Team: 3 people" became "3 students, 3-4 h a week each".
> - Why: dropped "frequent" (restaurant booking is weak in Thailand; see validation-vj.md #11). Added sources for "no online booking" and 54M MAU. "the long-term advantage" became "a possible long-term advantage".
> - Phase 1 objective: "Prove that people will use and pay for booking-by-chat" became "Show that booking-by-chat works and that people use it". Payment is out of scope, so willingness to pay is now a survey metric. Added the Wizard-of-Oz design.
> - Success metrics, all replaced:
>   - "50-100 users" became "15-30 users, 30-60 bookings".
>   - ">90% success" became "success rate with 95% confidence interval, split by channel".
>   - "30%+ repeat" became "report repeat use; no fixed target".
>   - "100+ venues" became "30-50 venues".
>   - Added time to confirmation, share needing a phone call, team time per booking, and software-quality metrics.
>   - The reason: the old targets exceed about 72-144 person-hours (estimate), and 50 bookings cannot show ">90%".
> - In scope:
>   - Vertical bullet: added the research note.
>   - Fulfilment bullet: rewritten (LINE handoff link + operator console, every human step logged).
>   - Added the outcome check.
>   - Reminder bullet: added the push-quota note.
>   - Venue database bullet: place ID instead of name/location; Google display-time and attribution rule; no Places content in test sets.
>   - Added the PDPA notice bullet.
> - Out of scope: "Additional verticals (courts, salons, spas)" lost its list, because the first vertical is still open. Added the merge note to "General assistant features".
> - Later phases: Phase 2 "platform APIs" became "partner APIs where a deal exists"; Phase 3 voice now needs lawyer review.
> - Constraints:
>   - OA-to-OA limit retagged [I]; added one OA per group [V] and the note on LINE's agent APIs and the channel adapter.
>   - Added the Instagram/Messenger, Google Maps terms and LINE Reply API bullets.
>   - Team line now has hours and dates.
> - Open decisions: "Budget and runway" became "Budget" (runway is a company concept); "Pricing model" marked "(context only)".
> - Tags: [V] primary source we opened; [V, opened by jui 8 Nov 2026]; [S] secondary; [I] our inference, including inference from primary docs; UNVERIFIED. Evidence: `docs/review/validation-vj.md`.

Status: DRAFT. Items marked (guess) are my assumptions, not decisions. Team: 3 students, 3-4 h a week each (roles not yet known).

## Objective
Build a LINE-based personal assistant for Thailand that recommends venues and books them for the user (restaurants, tennis courts, salons, spas), through whichever channel each venue accepts.

## Why
Booking is tedious, and many Thai venues have no online booking: small shops book by LINE chat, phone or paper [S, hdmall.co.th, via jui]. A bot in LINE removes the effort (54M MAU in Thailand [V, lycorp.co.jp]), and the venue channel data we verify becomes a possible long-term advantage.

## Phase 1 objective (first ~8-12 weeks) (guess)
Show that booking-by-chat works and that people use it, before building voice AI. Use a Wizard-of-Oz design: the software does everything except the last booking step, which a team member does through an operator console [S, Dahlbäck et al. 1993].

### Success metrics (revised estimates; adjust to your targets)
- 15-30 pilot users in one district, 30-60 bookings in total
- Booking success rate reported **with its 95% confidence interval**, split into three groups:
  - automated
  - LINE handoff (the user tapped Send)
  - human (concierge)

  For scale: 45 of 50 is 90%, with an interval of about 79-96%.
- Repeat use: the share of users who book again within 30 days (report it; no fixed target with this sample)
- Median time from request to confirmation; the share of requests that needed a phone call
- Known cost and team time per successful booking; stated willingness to pay (survey)
- Channel verified by hand for 30-50 venues in the chosen vertical
- Software quality:
  - Intent and slot accuracy (Thai and English) on a labelled test set built from our own data, with no Google Places content.
  - Zero bookings reported as confirmed without a confirmation.

## In scope: Phase 1
- LINE bot that takes a booking request in chat (Thai and English)
- Venue recommendations for one vertical in one city. Guess: restaurants in Bangkok. Research favours salons, barbers or courts for repeat use (validation-vj.md §4).
- Booking fulfilment, in one of two ways. Every human step is logged.
  - The bot generates a LINE handoff (`line.me/R/oaMessage/{ID}/?{text}`, mobile only [V, developers.line.biz]).
  - Or the bot passes the request to a team member in the **operator console**, who books by phone or web form.
- Outcome check: after a handoff, the bot asks the user whether the shop confirmed. The shop's reply goes to the user's chat, not ours [I, from LINE URL scheme docs].
- Booking confirmation and reminder messages to the user. Push messages count toward the OA quota [V, opened by jui 8 Nov 2026, developers.line.biz pricing].
- Venue database: Google place ID, our own verified contact and booking channel, verification date, success history.
  - No names, addresses or reviews copied from Google [V, Maps ToS s.3.2.3(a)].
  - Fetch Places details by place ID at display time and show Google attribution (Places API policies page; s.3.2.3(b) covers caching only).
  - Never use Places content in AI test or evaluation sets (s.3.2.3(c)(vii)).
  - No data collected from Wongnai [V, wongnai.com/terms, user obligations item (3)].
- Simple tracking of every request and outcome
- Basic user preferences (location, cuisine, budget, party size)
- PDPA privacy notice at first use; list of processors and countries [S, tilleke.com, via jui]

## Out of scope: Phase 1
- Voice AI calling
- Browser-agent form filling and platform API integrations (research only)
- Payments, deposits, and venue commissions
- Additional verticals and additional cities
- General assistant features (daily reminders, to-dos). Revisit when merging with the obligations-assistant direction.
- Venue-facing dashboard or merchant onboarding

## Later phases (direction, not commitment)
- Phase 2: automate the highest-volume channels (web forms, partner APIs where a deal exists, LINE handoff links); add a second vertical
- Phase 3: voice AI for venues with no other channel (needs lawyer review on recording and disclosure); merchant onboarding; revenue model (per booking or subscription)
- Phase 4: more cities, assistant features, partnerships

## Anti-goals
- A bot that books wrongly or not at all without telling the user
- Scraping or messaging in ways that break LINE, Google, or Wongnai terms
- Spending on voice AI before demand is proven

## Constraints
- LINE Messaging API cannot message another business's OA [I, from LINE sending-messages docs], and only one OA can be in a group [V, opened by jui 8 Nov 2026, developers.line.biz group-chats]. So venue LINE bookings need a user-tap handoff. This may change if LINE's announced agent-to-agent APIs open up [S, thansettakij.com, via jui]. Build the handoff as a replaceable channel adapter.
- Instagram and Messenger APIs only allow replies to people who messaged the business first [V, developers.facebook.com]
- Google Maps terms s.3.2.3 [V, cloud.google.com/maps-platform/terms]:
  - no copying or caching of Places content, except as the terms permit
  - no Places content in AI training or testing
  - no Places content on a non-Google map
  - no use in a "listings or directory service". Whether our venue list counts is an open question; until it is answered, recommend from our own verified data.
- PDPA applies to user data, including cross-border transfer to LLM APIs [S, tilleke.com, via jui]
- LINE Reply API is paid above 25,000 messages a month from 1 Feb 2027 [V, opened by jui 8 Nov 2026, lineforbusiness.com]
- Team of 3 at 3-4 h a week each; proposal due 22 Nov 2026; project ends 30 Apr 2027

## Open decisions (see open-questions.md)
- Team roles and who owns what
- First vertical and city
- Budget
- Tech stack
- Pricing model (context only)
