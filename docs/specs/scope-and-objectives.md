# Scope and Objectives

Status: DRAFT. Items marked (guess) are my assumptions, not decisions. Team: 3 people (roles not yet known).

## Objective
Build a LINE-based personal assistant for Thailand that recommends venues and books them for the user (restaurants, tennis courts, salons, spas), through whichever channel each venue accepts.

## Why
Booking is frequent and tedious, and many Thai venues have no online booking. A bot in LINE removes the effort, and the venue channel data we collect becomes the long-term advantage.

## Phase 1 objective (first ~8-12 weeks) (guess)
Prove that people will use and pay for booking-by-chat, before building voice AI.

### Success metrics (guess, adjust to your targets)
- 50-100 users in one city who have made at least one booking
- Booking success rate above 90% (counting concierge fulfilment)
- 30%+ of users book a second time within 30 days
- Known cost per successful booking and a price users accept
- Channel recorded for 100+ venues in the chosen vertical

## In scope: Phase 1
- LINE bot that takes a booking request in chat (Thai and English)
- Venue recommendations for one vertical in one city (guess: restaurants in Bangkok)
- Booking fulfilment via concierge: team members make bookings by phone, LINE handoff, or web form, with the bot handling the conversation
- Booking confirmation and reminder messages to the user
- Venue database: name, location, contact, booking channel, verification date, success history
- Simple tracking of every request and outcome
- Basic user preferences (location, cuisine, budget, party size)

## Out of scope: Phase 1
- Voice AI calling
- Browser-agent form filling and platform API integrations (research only)
- Payments, deposits, and venue commissions
- Additional verticals (courts, salons, spas) and additional cities
- General assistant features (daily reminders, to-dos)
- Venue-facing dashboard or merchant onboarding

## Later phases (direction, not commitment)
- Phase 2: automate the highest-volume channels (web forms, platform APIs, LINE handoff links); add a second vertical
- Phase 3: voice AI for venues with no other channel; merchant onboarding; revenue model (per booking or subscription)
- Phase 4: more cities, assistant features, partnerships

## Anti-goals
- A bot that books wrongly or not at all without telling the user
- Scraping or messaging in ways that break LINE, Google, or Wongnai terms
- Spending on voice AI before demand is proven

## Constraints
- LINE Messaging API cannot message another business's OA, so venue LINE bookings need a user-tap handoff
- PDPA applies to user data
- Team of 3; budget and timeline not yet known

## Open decisions (see open-questions.md)
- Team roles and who owns what
- First vertical and city
- Budget and runway
- Tech stack
- Pricing model
