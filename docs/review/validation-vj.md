# Validation of vj's documents

- Date: 9 November 2026. All web sources accessed 9 Nov 2026 unless marked "via jui", which means taken from the jui research files, which accessed them on 8 Nov 2026.
- Targets: `docs/review/source-vj/README.md` and `source-vj/docs/` (`idea-evaluation.md`, `open-questions.md`, `startup-evaluation.md`, `specs/scope-and-objectives.md`).
- Method: desk research only. We read official developer documents, terms of service, press releases and secondary reports. We did no sign-ups, sent no messages to businesses, and did not scrape anything.
- Tags: `[V]` means we opened the primary source ourselves; `[V, opened by jui 8 Nov 2026]` means the jui researchers opened the primary source and we reuse their reading. `[S]` means a secondary source (press, vendor, law-firm blog). `[I]` means our inference, including inferences drawn from primary documents (we say which). `UNVERIFIED` means not checked or no source found. Estimates are labelled.
- Revision 2 (9 Nov 2026, after judge verdict `docs/review/judge/judge-vj.md`): fixed D1-D8. Changes: OA-to-OA claim retagged [I]; Thai ASR paper re-described; Google Maps s.3.2.3(b), (c)(vii), (d)(iii), (e)(i) added with a design rule; agent-to-agent API note added; jui tags reworded; Wongnai clause cited as item (3); competitive confidence unified; '5 extras' title flagged; revised copies' notes made complete.
- Revised copies: `docs/review/revised/vj/`.

## 1. Summary verdict

**The documents are sound in direction but thin on evidence. Several channel claims need correcting.**

- vj's judgement holds up well. The "AI calls the venue is a feature, not a moat" verdict, the decision to put voice AI out of Phase 1, and the anti-goals are all supported by the evidence.
- Three claims are wrong or much weaker than written:
  1. **"Platform APIs (Chope, Fresha, Google Reserve) are reliable with partner access."** Reserve with Google is *inbound only*: venues get a Book button on Google, but another assistant cannot book through it. Fresha has no public booking-write API. Chope has only an affiliate programme and private integrations.
  2. **"The venue database is the main moat."** Google's terms forbid copying and saving business names, addresses or reviews from Places. Wongnai's terms forbid collecting shop data without permission. The database has to be built from our own verification, which is slower and weakens the moat.
  3. **"Booking success rate above 90% (counting concierge)"** and the other Phase 1 targets do not fit 3 students at 3-4 hours a week (section 3).
- The LINE facts are right. A bot cannot write into another business's Official Account (OA). The deep link `line.me/R/oaMessage` only pre-fills the text box, so the user still has to tap Send. vj leaves out one consequence: **the shop's reply goes to the user's own chat, and our bot never sees it.**
- A concierge MVP (humans do the bookings) is acceptable for a senior *software* project only if it is framed as a Wizard-of-Oz study (a person secretly plays part of the system so the rest can be tested) and the team still builds and evaluates real software around it (section 3.3).

Counts: 21 claims checked. 11 CONFIRMED, 6 WEAKER, 1 WRONG, 3 UNVERIFIED. The Phase 1 metrics are a logic finding (section 3.2), not counted here.

## 2. Claim table

| # | Claim (short) | File | Verdict | Evidence | Fix |
|---|---|---|---|---|---|
| 1 | LINE Messaging API cannot message another business's OA | idea-eval, scope, startup-eval | CONFIRMED (as an inference from primary docs) | A bot sends only to users, groups or rooms tied to its own channel [V, opened by jui 8 Nov 2026, developers.line.biz sending-messages]. Only one OA can be in a group [V, opened by jui 8 Nov 2026, group-chats]. No OA-to-OA path is documented, so "cannot message another OA" is an inference [I, from LINE sending-messages docs]. LINE announced agent APIs and agent-to-agent protocols at LINE DEV CONF 2026, with a closed beta through LINE HACKS 2027 [S, thansettakij.com, via jui]; this may relax the limit. | Tag the OA-to-OA statement [I]; keep [V] only for "one OA per group". Add: may change if LINE's agent-to-agent APIs open up; design the handoff as a replaceable channel adapter. |
| 2 | Use a user-tap deep link with prefilled text | idea-eval, scope | CONFIRMED, with limits | `https://line.me/R/oaMessage/{LINE ID}/?{text}` "opens a chat screen with your LINE Official Account" and "enters the text message ... into the message input field". It works on LINE for iOS and Android, not on LINE for PC. The ID and text must be percent-encoded [V, developers.line.biz/en/docs/messaging-api/using-line-url-scheme/]. That the user must tap Send is inferred from "input field" [I]. | Add three limits: (a) mobile only; (b) we need the shop's LINE ID; (c) **the shop replies in the user's own chat, which our bot cannot read**. So "confirmed" needs the user to report back, or a follow-up question from the bot. |
| 3 | Platform APIs (Chope, Fresha, Google Reserve partners): reliable, need partner access | idea-eval | WRONG as written | **Google:** Thailand is listed for Actions Center reservations. It is an inbound integration: scheduling providers apply and are approved, and users book on Google [V, opened by jui 8 Nov 2026, developers.google.com/actions-center]. A third-party assistant cannot book through it. **Fresha:** no public booking-write API. Its MCP server (Aug 2026) hands the user to Fresha to confirm and pay [S via jui, cxnetwork.com; businesswire]. **Chope:** no public developer API found. Its affiliate page mentions "widgets, cookies and API" modes [S, chope.co affiliate programme]. It has private two-way syncs with SevenRooms and TableCheck [S, sevenrooms.com; tablecheck.com]. Grab bought Chope's SEA business in July 2024 [S, techcrunch.com 2024-07-23]. | Rewrite the row: "Partner deals only; none lets a third-party assistant book by default. Google Reserve is a way for *our own* merchants to get a Book button, not a way to book others. Chope affiliate is a link-out, not execution." |
| 4 | Instagram/Facebook DMs "mostly blocked for third parties" | idea-eval | CONFIRMED (stronger: fully blocked for this use) | Instagram: "Only after an Instagram user has sent your app user's Instagram professional account a message can your app send a message". There is a 24-hour window [V, developers.facebook.com Instagram Messaging API]. Messenger: a business replies within 24 hours of a user action, and tags cover only listed cases [V, developers.facebook.com messenger-platform/policy]. No API lets our app open a DM to another business's Page or account [I from both docs]. Automating a user's personal account is not allowed [I, UNVERIFIED wording]. | Change to "Blocked: the APIs only let a business reply to people who messaged it. Use an `m.me` / `ig.me` link plus copy-paste text as a manual handoff." |
| 5 | Web forms via browser agent: works, brittle | idea-eval | CONFIRMED | Web Bench found agents did "surprisingly poorly" on write tasks such as forms and logins [S via jui, skyvern.com]. CAPTCHAs and logins block bots [I]. | Keep. Add "check each site's terms first". |
| 6 | Email: easy, slow | idea-eval | UNVERIFIED | No Thai data on how many venues answer booking emails. TableCheck says Chope booking emails "may be delayed or undelivered" [S, support-restaurants.tablecheck.com]. | Mark as an assumption to measure in the pilot. |
| 7 | Phone (voice AI): works, costly, least reliable | idea-eval | WEAKER | Cost of about ฿15-25 per booking call, as an estimate from vendor per-minute prices [S/I via jui, retellai.com, 11x.ai]. Thai ASR (automatic speech recognition, i.e. speech to text) reaches about 6-11% character error rate (CER) on read speech (Gigaspeech2-Typhoon) and on noisy real-world TV audio (TVSpeech) [S, arxiv.org/abs/2601.13044]. We found **no telephone-audio (narrowband) benchmark**. "Works" is unproven for Thai phone calls. | "Possible in demos; Thai telephone-audio accuracy unmeasured; ฿15-25 per call (estimate)." |
| 8 | Voice AI in Thai on noisy lines is unreliable; easily copied | idea-eval | UNVERIFIED (plausible) | See #7: a noisy-audio benchmark exists, but none for telephone audio. Errors on names, times and Thai numbers matter most [I]. "Easily copied": voice platforms (Retell, Vapi, Bland) are rentable by anyone [S via jui]. | Keep as a risk; label it an assumption. |
| 9 | Many Thai venues use LINE OA rather than phone | idea-eval | WEAKER | LINE Thailand reports 6M+ OAs and 12B OA conversations in 2025 [S via jui, relevantaudience]. Health and beauty brands have opened 380,000 OAs [V, opened by jui 8 Nov 2026, lineforbusiness.com]. **No survey gives the share of venues that take bookings by LINE versus phone** [UNVERIFIED]. | "Many venues have a LINE OA; the booking share by channel is unknown. Measure it while building the venue list." |
| 10 | Thai LINE user base is large (verify the figure) | startup-eval | CONFIRMED, figure added | **54 million monthly active users in Thailand as of 31 Mar 2026** [V, lycorp.co.jp/en/company/global/]. LINE marketing material says 56M "users" [S via jui, relevantaudience]. That higher figure is not monthly active. | Use 54M MAU [V]. |
| 11 | Booking is frequent and easy to hand off | idea-eval, scope | WEAKER for restaurants | Hungry Hub started as a reservation tool. It "failed as there was no real need ... one could walk into a restaurant", and it pivoted to deals (2014-2017) [S via jui, e27.co]. Grab Dine Out "Book Table" lists 500+ brands in Bangkok and Phuket, aimed at premium dining [V, grab.com/th/en/press/others/grabdineout2025, 4 Sep 2025]. So reservations matter for a premium niche, not for everyday meals. | "Frequent for salons and courts; for restaurants, mainly premium or group dining." |
| 12 | Many Thai venues have no online booking | scope | CONFIRMED (qualitative) | Most small barbers, salons and clinics book by LINE chat, phone or a paper book [S/I via jui, hdmall.co.th]. Fresha's Bangkok landing page shows 34 venues [S, fresha.com/lp/en/th-bangkok]. | Keep; cite. |
| 13 | AI call disclosure is a legal issue | idea-eval, startup-eval, open-q | WEAKER | No enacted Thai rule requires a bot to say it is an AI [UNVERIFIED as a negative, via jui]. ETDA's draft AI Act (hearing 2 Jul 2026) adds transparency duties and strict liability. It is not enacted [S via jui, tilleke.com]. NBTC telecom AI guidelines are non-binding [S via jui]. | "Not required by enacted law today; likely under the draft AI Act; disclose anyway." |
| 14 | Call-recording consent is a legal issue | idea-eval, startup-eval | WEAKER (unsettled) | No single Thai statute. In practice a participant may record (close to one-party consent), but the recording is personal data under the PDPA and needs a lawful basis and notice [S, recordinglaw.com; S via jui, tilleke.com]. The sources conflict [S, isitlegaldb, low quality]. | "Recording is personal data under PDPA; give notice before recording. Lawyer review before any voice pilot." Irrelevant in Phase 1 (no voice). |
| 15 | PDPA applies to user data | idea-eval, scope | CONFIRMED | We are the data controller for chats [I]. Processing needs consent or a s.24 basis (contract is one) [S, PDPC consent guideline; law-firm summaries]. Sending chats to foreign LLM APIs is a cross-border transfer (PDPC notifications in force 24 Mar 2024) [S via jui, tilleke.com]. Health data (clinic reasons) needs explicit consent under s.26 [S via jui]. | Add: privacy notice, contract basis for booking, list of processors and countries, avoid health data. |
| 16 | Scraping beyond terms is a risk (Google, Wongnai) | idea-eval, startup-eval, scope | CONFIRMED (stronger) | Google Maps Platform ToS s.3.2.3 [V, cloud.google.com/maps-platform/terms]: (a) no scraping, including "copy and save business names, addresses, or user reviews" and (a)(iv) "use Google Maps Content with text-to-speech services"; (b) no caching except as the Service Specific Terms permit; (c)(vii) no using Google Maps Content "to improve machine learning and artificial intelligence models, including to train, test, validate or fine-tune the models"; (d)(iii) no using the Core Services "in a listings or directory service"; (e)(i) no displaying or using "Places content on a non-Google Map". Place IDs may be stored indefinitely; lat/lng for 30 days [V, developers.google.com/maps/documentation/places/web-service/policies]. Wongnai ToS, user obligations item (3): users may not collect shop data, addresses or reviews "for any purpose" without permission [V, wongnai.com/terms]. | **"Seed from Google Places and Wongnai" (idea-eval step 1) must change.** Design rule: store only Google place IDs plus facts we verify ourselves; fetch Places details at display time by place ID; show Google attribution; never put Places content into evaluation or test datasets; never read Google content out by voice; do not show Places content on a non-Google map; do not copy Wongnai data. **Open question:** is a booking assistant's venue list a "listings or directory service" under (d)(iii)? |
| 17 | Venue channel database is "the main moat" | README, scope | WEAKER | #16 rules out bulk seeding from Google or Wongnai, so the database grows only by our own checks and booking outcomes [I]. Rivals with merchant relationships already hold this data: Grab/Chope, LINE MAN Wongnai (700k merchants, owns JERA Cloud) [S via jui]. | "A possible moat, slow to build, and only if verified by hand and kept fresh." |
| 18 | Competition: booking platforms and other assistants | idea-eval, startup-eval | CONFIRMED (stronger than written) | Grab Dine Out Book Table [V, grab.com 2025]. Wongnai Reservations puts a Reserve button on Google [S via jui, brandinside.asia]. LY Corp runs AI phone booking in Japan (GA 22 Oct 2026) [V, opened by jui 8 Nov 2026, lycorp.co.jp]. ChatGPT books through OpenTable [S via jui]. Google AI Mode booking is not in Thailand yet [S via jui]. | Name these competitors in the startup evaluation. |
| 19 | Plausible acquirers: delivery / booking players | startup-eval | CONFIRMED (as plausibility) | Grab acquired Chope's SEA business in July 2024 [S, techcrunch.com]. LMWN acquired JERA Cloud in Aug 2025 [S via jui]. | Cite as precedent. |
| 20 | Pre-seed investors in SEA value teams and traction, not ideas | startup-eval | UNVERIFIED | No source given or found in this review. A common view, not checked. | Mark as opinion, or drop (out of scope for a university project). |
| 21 | Per-booking cost (voice minutes, retries) is a risk | idea-eval, startup-eval | CONFIRMED | Voice ≈ ฿15-25 per booking (estimate), 50-100 times the LLM cost [S/I via jui]. LINE Reply API above 25,000 messages a month becomes paid from 1 Feb 2027 (฿5,000 a month unlimited plan) [V, opened by jui 8 Nov 2026, lineforbusiness.com/th/news/20261030]. | Add the LINE Reply API change to cost risks. |

Conflicts with the jui research, re-checked:
- **LINE users:** jui uses 56M "users" [S]. LY Corp's own page gives **54M MAU at 31 Mar 2026** [V]. Use 54M MAU.
- **Fresha MCP date:** jui's execution file says Sept 2026; jui's own fact check corrected this to Aug 2026. We use Aug 2026.
- **Reserve with Google in Thailand:** jui's execution file said UNVERIFIED; jui's fact check confirmed Thailand is listed [V]. We use the confirmed version.

## 3. Logic findings

### 3.1 Startup scores against the evidence

The scores are honest and mostly match the evidence. Points to change:

| # | Criterion | vj score | Suggested | Why |
|---|---|---|---|---|
| 1 | Market potential | 3 (Low) | 3 (Low) | Reach is real (54M MAU [V]). Restaurant booking demand is weak (walk-ins) [S]. Willingness to pay is still unknown. |
| 2 | Team quality | ? | 2 (Medium) | Now known: 3 students at 3-4 h a week, ending 30 Apr 2027 (TEAM-BRIEF). Fine for a prototype, too little for a company [I]. |
| 4 | Competitive position | 2 (Medium) | 2 (Medium-High) | The evidence now supports the low score: Grab/Chope, Wongnai Reservations, Fresha, LINE's Japan AI booking. |
| 5 | Product / technology | 2 | 2 | Correct. Add that the "moat" database is limited by data-source terms (#16). |
| 8 | Legal / regulatory | 2 (Low) | 3 for Phase 1, 2 if voice | Phase 1 has no voice and no payments. Bookings without money are legally light; the main duties are PDPA and data-source terms [S/I via jui]. |
| 9 | Exit | 3 | 3 | Grab-Chope and LMWN-JERA are real precedents [S]. |
| 10 | Risk | 2 | 2 | Add the LINE Reply API charge from Feb 2027 [V, opened by jui 8 Nov 2026]. |

The README's "main moat" wording is stronger than the startup evaluation's own score of 2 and its note "Potential moat". The revised README aligns the two.

### 3.2 Are the Phase 1 metrics realistic? No, not as written.

Time budget (estimate): 3 people × 3-4 h = **9-12 person-hours a week**. Over 8-12 weeks that is **72-144 person-hours in total**, for building *and* running the pilot.

Rough cost of each target (estimates, not measured):

| Target | Effort estimate | Problem |
|---|---|---|
| Build the LINE bot, venue database, request log, ops screen | 60-100 h | Takes most of the budget before any user arrives. |
| Channel recorded for 100+ venues | ~15-25 h (about 10-15 min per venue) | Doable alone, but competes with the build. Cannot be seeded from Google or Wongnai (#16). |
| 50-100 users with at least one booking, fulfilled by concierge | ~10-35 h of booking work (10-20 min per booking) plus recruiting | Concierge work happens during venue hours, often when students are in class [I]. |
| Booking success > 90% | — | With 50 bookings, 45 successes gives a 95% interval of about 79-96% (Wilson interval). The sample cannot show ">90%". Counting human-made bookings also measures the team's effort, not the software. |
| 30%+ book again within 30 days | — | Restaurant booking is infrequent in Thailand (#11). A 30-day window with a few dozen users gives a very noisy number. |
| A price users accept | — | Payments are out of scope, so only stated willingness to pay can be measured. That is weak evidence [I]. |

Suggested realistic targets for Phase 1 (estimates for the team to adjust):
- 15-30 pilot users (classmates and staff), 30-60 bookings in total.
- 30-50 venues in one district, with the channel verified by hand.
- Report the success rate with its confidence interval, split into: automated (bot only), handoff (user tapped the deep link), and human (concierge).
- Measure time to confirmation, handoff completion rate, and the share of requests that needed a phone call (this answers vj's own "unknown #3").
- Willingness to pay as a short survey, labelled stated preference.

### 3.3 Is a concierge MVP acceptable for a senior software project?

**Only as one part of the evaluation, not as the product.** If humans make the bookings, the software is just a chat front end. That is too thin for a software deliverable that needs "architecture and overall design finalized" (TEAM-BRIEF).

It becomes acceptable when it is a **Wizard-of-Oz study**. This is an established HCI (human-computer interaction) method: a hidden person plays the part of the system that is not built yet, so the rest can be tested with real users [S, Dahlbäck, Jönsson and Ahrenberg, "Wizard of Oz studies: why and how", IUI 1993 / Knowledge-Based Systems 6(4), 1993]. To make it work as a software project:
1. Build real software for everything except the last mile: request understanding (Thai and English), slot filling, venue lookup, channel router, `oaMessage` handoff generation, confirmation and reminder messages, request log.
2. Build an **operator console**, so the "wizard" works through our software, and log every human step. These logs become the dataset for deciding what to automate next.
3. Automate at least one channel end to end. The LINE handoff is the cheapest: the bot drafts and links, and the user taps Send.
4. Evaluate the software separately: intent and slot accuracy on a labelled test set, router correctness, latency, and failure handling ("never claim success without confirmation").

### 3.4 Other logic gaps

- **Confirmation loop.** With an `oaMessage` handoff, the reply lands in the user's chat with the shop. The bot needs a follow-up ("Did the shop confirm? Yes / No / Changed time") or it cannot know the outcome. This affects every success metric.
- **"Reminder" messages are push messages.** They count toward the LINE OA quota (Free plan 300 a month) [V, opened by jui 8 Nov 2026, lineforbusiness.com/th/news/20240619_1]. Fine for a pilot; note it.
- **Abdul-style features.** Open question 5 asks whether reminders ship in v1, while the scope puts them out of scope. The combined proposal (omm's obligations assistant) may need them in scope. Flag this for the team.
- **Scope says "Thai and English"** but has no target for language accuracy. Add one.
- **Title count.** The startup evaluation's title says "10 criteria + 5 extras" but lists 4 extras (pre-existing in vj's original). The revised copy says 4.
- **Places content and evaluation.** A labelled test set must not contain Google Places content (#16, s.3.2.3(c)(vii)). Build test utterances from our own, hand-written venue data.

## 4. Gaps filled: booking channels in Bangkok by vertical

| Vertical | Channels found | Can a third party book? |
|---|---|---|
| Restaurants | Grab Dine Out "Book Table" (Chope), 500+ brands in Bangkok and Phuket [V, grab.com 2025]. Chope affiliate programme [S]. Wongnai Reservations via Google Search and Maps for Wongnai POS restaurants (฿500-2,390 a month to the restaurant) [S via jui]. Hungry Hub, about 2,000+ partners, no public API [S via jui]. Reserve with Google (Thailand listed) [V, opened by jui 8 Nov 2026]. Plus walk-in, phone, LINE OA. | No open API. Affiliate links and the user-tap handoff only. |
| Salons and spas | Fresha (34 Bangkok venues on its landing page [S]; MCP is discovery only [S]). GoWabi (no public API) [S via jui]. LMWN's JERA Cloud (1,700+ clinic, salon and spa branches) [S via jui]. LINE OA chat, Messenger, phone [I]. | SimplyBook.me only, with each merchant's API key [S via jui]. Otherwise handoff. |
| Courts (tennis, badminton, padel) | Venue-run systems: white-label club apps (e.g. Book and Go, vendor claims) [S, bookandgo.app]. Matchi for padel [S, matchi.se]. Some courts on GoWabi [S, gowabi.com]. Many by LINE or phone [UNVERIFIED]. | No aggregator API found [UNVERIFIED as a negative]. |

Takeaway [I]: no vertical has an open booking API. The LINE handoff plus the operator console is the realistic Phase 1 mix. Salons and courts have more repeat bookings than restaurants. jui's research recommends barbers first, then salons.

## 5. Strengths worth keeping in the combined proposal

1. **Multi-channel router with a confidence score and last-verified date for each venue.** This is a clean, testable architecture component. Each attempt updates the score.
2. **"Voice is a feature, not a moat" and no voice before demand.** It is backed by cost and legal evidence.
3. **The LINE handoff with prefilled text.** It is correct, allowed, and cheap. It is the best fallback for shops that are not on any system. Build it as a replaceable channel adapter, because LINE's announced agent-to-agent APIs [S, thansettakij.com, via jui] may later allow direct OA messaging.
4. **Anti-goals:** never book wrongly or silently; never break LINE, Google or Wongnai terms. These translate directly into requirements.
5. **Confidence column in the startup evaluation.** Separating the score from the evidence behind it is good practice.
6. **Learning from every booking attempt** (success or failure per channel). This gives a natural dataset for the evaluation chapter.
7. **Tight Phase 1 scope** (one vertical, one city, no payments, no merchant dashboard). Right-sized once the targets are reduced.

## 6. Sources (accessed 9 Nov 2026 unless noted)

- LINE URL schemes: https://developers.line.biz/en/docs/messaging-api/using-line-url-scheme/
- LY Corp global (54M MAU Thailand): https://www.lycorp.co.jp/en/company/global/
- Instagram Messaging API: https://developers.facebook.com/docs/instagram-platform/instagram-api-with-instagram-login/messaging-api/
- Messenger Platform policy: https://developers.facebook.com/docs/messenger-platform/policy/policy-overview/
- Google Maps Platform Terms of Service: https://cloud.google.com/maps-platform/terms
- Places API policies: https://developers.google.com/maps/documentation/places/web-service/policies
- Wongnai Terms of Service: https://www.wongnai.com/terms
- Grab Dine Out Thailand (4 Sep 2025): https://www.grab.com/th/en/press/others/grabdineout2025/
- Grab reservation terms (Chope Thailand): https://www.grab.com/th/en/terms-policies/reservation-services-terms/
- Grab acquires Chope: https://techcrunch.com/2024/07/23/grab-acquires-singapores-restaurant-reservation-platform-chope
- Chope affiliate programme: https://www.chope.co/singapore-restaurants/pages/affiliateprogram
- Chope integrations: https://sevenrooms.com/blog/chope-is-now-officially-integrated-with-sevenrooms/ ; https://support-restaurants.tablecheck.com/hc/en-us/articles/36452921067033-Chope-Integration
- Fresha Bangkok: https://www.fresha.com/lp/en/th-bangkok
- Courts: https://www.bookandgo.app/en/booking/tennis-booking-system-bangkok ; https://www.matchi.se/facilities/bangkokpadel ; https://www.gowabi.com/ko/provider/badminton-bd-by-pkk
- Thai ASR: https://arxiv.org/abs/2601.13044
- Recording law: https://www.recordinglaw.com/world-laws/world-recording-laws/thailand-recording-laws/
- PDPA consent guideline (PDPC, machine translation): https://clinregs.niaid.nih.gov/sites/default/files/documents/thailand/PersonalDataConsentGuidelines-GoogleTranslation.pdf
- Wizard of Oz method: Dahlbäck, Jönsson, Ahrenberg (1993), https://www.ida.liu.se/~arnjo82/papers/kbs.pdf
- Reused from jui (accessed 8 Nov 2026), full URLs in `docs/research/ai-assistant-jui/2026-11-08-verify-agent-proposal.md` and `2026-11-08-agent-execution-tech.md`: LINE messaging and pricing docs, Actions Center, Fresha MCP, Hungry Hub, Wongnai Reservations, LMWN/JERA, LY Corp Japan AI booking, voice pricing, ETDA draft AI Act, PDPA cross-border notes.
