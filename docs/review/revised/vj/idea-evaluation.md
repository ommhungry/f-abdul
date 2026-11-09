# Idea Evaluation

> **Revision notes (9 Nov 2026)**. Every change from vj's original:
> - Verdict: added "(Holds after review.)"
> - Strengths:
>   - "Booking is frequent": added the restaurant walk-in caveat and the Grab "Book Table" premium note.
>   - LINE bullet: added 54M MAU.
>   - Long-tail bullet: added the source for how small shops book.
> - Risks:
>   - Voice: "on noisy lines is unreliable" became "on phone lines is unproven" [UNVERIFIED]. Added the Thai ASR evidence: a noisy-audio benchmark exists; no telephone-audio benchmark does.
>   - LINE OA: changed "Many Thai venues use LINE OA rather than phone" to "many have a LINE OA; the booking share is unknown".
>   - Per-booking cost: added the voice-cost estimate and the LINE Reply API charge from 1 Feb 2027.
>   - Legal: split into AI disclosure, recording and PDPA, each with its current status.
>   - Competition: named the competitors.
> - Channels table:
>   - Platform API row rewritten: partner deals only; Google Reserve is inbound.
>   - Web forms: "check site terms".
>   - Email: marked as an assumption.
>   - Phone: "human in Phase 1; voice later".
>   - LINE OA row: retagged [I]; added handoff limits and a note on LINE's agent APIs.
>   - Instagram/Facebook: "mostly blocked" became "blocked".
> - Venue learning step 1: rewritten to follow Google Maps terms s.3.2.3 (a), (b), (c)(vii), (d)(iii) and (e)(i), and the Wongnai terms.
> - Note under the list: scraping risk marked confirmed; the text-to-speech rule added.
> - Suggested path step 2: reframed as a Wizard-of-Oz study.
> - Tags: [V] primary source we opened; [V, opened by jui 8 Nov 2026] primary source opened by the jui researchers; [S] secondary; [I] our inference, including inference from primary docs; UNVERIFIED. Evidence: `docs/review/validation-vj.md`. Sources accessed 9 Nov 2026 unless stated.

**Verdict:** real pain point and a workable wedge, but "AI calls the venue" is a feature, not a moat. (Holds after review.)

## Strengths
- Booking is frequent, annoying, easy to hand off. This is true for salons and courts. For restaurants it applies mainly to premium or group dining: Thais often walk in [S, e27.co on Hungry Hub, via jui]. Grab's "Book Table" targets mid-to-premium dining [V, grab.com/th 2025].
- LINE gives distribution and a familiar interface: 54M MAU in Thailand [V, lycorp.co.jp].
- Calling/messaging reaches long-tail venues without online booking. Most small shops book by LINE chat, phone or paper [S, hdmall.co.th, via jui].

## Risks
- Voice AI in Thai on phone lines is unproven [UNVERIFIED]:
  - Thai ASR reaches about 6-11% character error rate on read speech and on noisy TV audio [S, arXiv 2601.13044].
  - No telephone-audio benchmark was found.
  - It is easily copied: voice platforms are rentable [S, via jui].
- Many Thai venues have a LINE OA (6M+ OAs in Thailand) [S, relevantaudience, via jui]. The share that *take bookings* by LINE rather than phone is unknown [UNVERIFIED]; measure it.
- A wrong or missed booking destroys trust: need confirmations, retries, human fallback
- Per-booking cost:
  - Voice ≈ ฿15-25 a call (estimate) [S/I, via jui].
  - LINE Reply API above 25,000 messages a month becomes paid from 1 Feb 2027 [V, opened by jui 8 Nov 2026, lineforbusiness.com/th/news/20261030].
- Legal:
  - AI disclosure: not required by enacted Thai law today. The draft AI Act (hearing Jul 2026) would add transparency rules [S, tilleke.com, via jui]. Disclose anyway.
  - Recording: a voice recording is personal data under the PDPA; give notice. Consent rules are unsettled [S, recordinglaw.com].
  - PDPA: privacy notice, a lawful basis, and cross-border notice for LLM APIs [S, tilleke.com, via jui].
- Competition and other assistants:
  - Grab Dine Out / Chope: 500+ brands in Bangkok and Phuket [V, grab.com].
  - Wongnai Reservations via Google [S, brandinside.asia, via jui].
  - Fresha [S].
  - LINE's own AI booking in Japan [V, opened by jui 8 Nov 2026, lycorp.co.jp].

## Channels
| Channel | Feasibility |
|---|---|
| Platform APIs (Chope, Fresha, Google Reserve) | Partner deals only. Fresha: no public booking API; its MCP hands the user to Fresha [S, cxnetwork.com, via jui]. Chope: affiliate links and private integrations [S, chope.co; sevenrooms.com]. Google Reserve: lets *our own* merchants get a Book button; cannot book other venues [V, opened by jui 8 Nov 2026, developers.google.com/actions-center] |
| Web forms via browser agent | Works, brittle [S, skyvern.com, via jui]; check site terms |
| Email | Easy, slow (assumption; measure) |
| Phone | Human in Phase 1. Voice AI possible later: costly (estimate ฿15-25 a call); Thai telephone-audio accuracy unmeasured |
| Venue LINE OA | LINE Messaging API can't message another business's OA [I, from LINE sending-messages docs]. Only one OA can be in a group [V, opened by jui 8 Nov 2026, developers.line.biz group-chats]. Use the user-tap deep link `line.me/R/oaMessage/{ID}/?{text}`, which pre-fills the text box [V, developers.line.biz]. Mobile only; we need the shop's LINE ID; **the shop replies in the user's chat, so the bot must ask the user for the outcome**. LINE's announced agent-to-agent APIs may relax the limit [S, thansettakij.com, via jui], so build the handoff as a replaceable channel adapter |
| Instagram/Facebook DMs | Blocked: the APIs only let a business reply to people who messaged it first [V, developers.facebook.com]. Manual handoff via an `m.me` / `ig.me` link only |

## How the bot learns each venue's channel
1. Seed only from sources we may use:
   - Google place IDs only (storable indefinitely) [V, Places API policies].
   - Venue sites and public OA profiles, checked by hand.
   - Google Maps terms s.3.2.3 [V, cloud.google.com/maps-platform/terms]:
     - Do not copy names, addresses or reviews (a).
     - No caching beyond what the terms permit (b). Fetch Places details by place ID at display time and show Google attribution.
     - Never put Places content into AI test or evaluation sets (c)(vii).
     - Do not show Places content on a non-Google map (e)(i).
     - Check whether our venue list counts as a "listings or directory service" (d)(iii). This is an open question.
   - Do not collect shop data from Wongnai [V, wongnai.com/terms, user obligations item (3)].
2. Probe and verify top venues; keep confidence score + last-verified date
3. Learn from every booking attempt (success/failure per channel)
4. Ask venues for their preferred channel (merchant onboarding)

Note: use official APIs where possible; scraping beyond terms is a confirmed risk (see step 1). Google content also may not be used with text-to-speech [V, Maps ToS s.3.2.3(a)(iv)], which matters for any voice feature.

## Suggested path
1. One vertical, one city
2. Concierge MVP (human or semi-automated booking) to test demand. For the senior project, run it as a Wizard-of-Oz study: real software for everything except the last mile, with an operator console that logs each human step [S, Dahlbäck et al. 1993].
3. Build the moat: user preferences, venue channel database, merchant relationships
4. Revenue: per successful booking or subscription; venue commission later
