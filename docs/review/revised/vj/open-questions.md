# Open Questions

> **Revision notes (9 Nov 2026)**. Every change from vj's original:
> - Q1 (solo or team): struck through and answered (3 students, 3-4 h a week, senior project to 30 Apr 2027).
> - Q2 (first vertical and city): added research findings per vertical; still a team decision.
> - Q4 (business model): added "(context only for the senior project)".
> - Q5 (Abdul-style reminders in v1): added the need to settle it with omm's obligations-assistant direction.
> - Q6 (partner access): added a finding for Chope, Fresha, Google Reserve and LINE, each with a source or UNVERIFIED.
> - Q7 (legal review): added the current status of AI disclosure, recording and PDPA.
> - Four new questions added at the end: handoff outcome, Phase 1 targets, venue list without copying data, and whether the venue list is a "directory service" under Google's terms.
> - Q3 (tech stack) is unchanged.
> - Tags: [V] primary source we opened; [V, opened by jui 8 Nov 2026]; [S] secondary; [I] our inference; UNVERIFIED. Evidence: `docs/review/validation-vj.md`. Sources accessed 9 Nov 2026 unless stated.

- ~~Who is building this: solo or a team?~~ **Answered:** 3 students, 3-4 h a week each (TEAM-BRIEF). So the plan is a working prototype plus a small Wizard-of-Oz (concierge) pilot, not a company launch.
- First vertical and city (restaurants in Bangkok? salons? courts?)
  - Research: no vertical has an open booking API (validation-vj.md §4).
  - Restaurants: strong incumbents (Grab/Chope [V, grab.com]; Wongnai Reservations [S, via jui]) and a walk-in habit [S, e27.co, via jui].
  - Salons and courts: more repeat bookings and more LINE or phone booking [I]. jui's research suggests barbers, then salons.
  - Still a team decision.
- Tech stack (not chosen)
- Business model: per-booking fee, subscription, or venue commission (context only for the senior project)
- Does Abdul-style daily reminders/assistant functionality ship in v1, or only booking + recommendations? Must be settled with omm's obligations-assistant direction in the combined proposal.
- Partner access: Chope, Fresha, Google Reserve, LINE booking tools
  - Chope: affiliate programme (widgets, cookies, API) and private integrations; no public developer API found [S, chope.co affiliate page; sevenrooms.com].
  - Fresha: no public booking-write API; its MCP server is discovery only [S, cxnetwork.com, via jui].
  - Google Reserve: inbound only; provider approval needed [V, opened by jui 8 Nov 2026, developers.google.com/actions-center].
  - LINE: we found no native LINE OA booking feature in Thailand [UNVERIFIED as a negative]. Third-party LINE booking add-ons are reported [UNVERIFIED: the vendor link checked on 9 Nov 2026 did not resolve]. A LINE MINI App for hotel booking (IHG) is reported [S, eglobaltravelmedia.com.au, accessed 9 Nov 2026].
  - LINE also has an agent Pioneer Program / LINE HACKS 2027 [S, thansettakij.com, via jui]; apply to it.
- Legal review: AI-disclosed calls, recording consent, PDPA
  - No enacted AI-disclosure rule; the draft AI Act would add one [S, tilleke.com, via jui].
  - Recording is personal data under the PDPA; consent rules are unsettled [S, recordinglaw.com].
  - PDPA privacy notice and cross-border notice are needed in Phase 1 [S, tilleke.com, via jui].
  - Lawyer review still needed before any voice pilot.
- **New:** after a LINE handoff, the shop replies in the user's chat, not ours [I, from LINE URL scheme docs]. How does the bot learn the outcome: a follow-up question, a screenshot, or a manual operator step?
- **New:** which Phase 1 targets fit 72-144 person-hours in total (estimate)? See `specs/scope-and-objectives.md`.
- **New:** how do we build the venue list without copying Google or Wongnai data? Answer: place IDs plus manual checks; fetch Places details at display time [V, Google Maps ToS s.3.2.3; Places API policies].
- **New:** does a booking assistant's venue list count as a "listings or directory service", which Google Maps terms s.3.2.3(d)(iii) forbid [V, cloud.google.com/maps-platform/terms]? If yes, recommendations must not use Google Maps Core Services. Ask Google or a lawyer; until then, recommend from our own verified venue data.
