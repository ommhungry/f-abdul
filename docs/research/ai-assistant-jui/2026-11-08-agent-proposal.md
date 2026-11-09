# Project Proposal: an AI assistant in LINE that books, not just reminds

> **Superseded by `2026-11-08-agent-proposal-v2.md`** (broad assistant with request cards; the user is the payer). Kept for history.

- Working name: **JongHai (จองให้, "I'll book it for you")**. This is a placeholder.
- Date: 2026-11-08. Status: **PROCEED WITH CHANGES**. Desk research only; no merchant or user has been interviewed yet.
- Sources (all in `docs/research/`):
  - `2026-11-08-abdul-teardown.md` (baseline)
  - `2026-11-08-agent-execution-tech.md`
  - `2026-11-08-agent-market-competitors.md`
  - `2026-11-08-agent-legal-economics.md`
  - `2026-11-08-verify-agent-proposal.md` (red team: 16 claims, 13 confirmed, 2 weaker, 1 wrong)
  - The comparison with tutoring uses `2026-11-04-project-proposal.md`.
- Evidence tags: [V] primary or official source, [S] secondary source, [I] our inference, UNVERIFIED. Each tag is carried over from the cited research file.

---

## 1. Summary

**Abdul** (abdul.chat) is a Thai AI assistant in LINE with 41,573 friends. It reminds users of things but takes no action outside LINE and their Google Calendar [V, teardown]. Our idea is an assistant that **completes the task**, starting with booking a haircut.

Research found that **no open way exists for an AI to book an arbitrary Thai shop**:
- Booking platforms either have no public API or only hand off to their own checkout [V/S, execution-tech §2a; verify #1-2].
- A LINE bot cannot message another shop's LINE OA [V, verify #4].
- AI phone calls cost ฿15-25 per booking [I, legal-economics §4.4].

So the realistic product has two sides, built in this order:
1. **For barbers (supply):** a free LINE-native booking tool. It gives a booking page inside LINE, a staff calendar, the day's schedule pushed to the owner, and reminders to customers that cut no-shows.
2. **For customers (demand):** an Abdul-style assistant in LINE. It understands "ตัดผมเสาร์บ่ายร้านเดิม" ("haircut Saturday afternoon, same shop as usual"), finds a free slot, shows a confirmation card, books after one tap, writes the booking to the calendar and reminds the user.

For shops that are not on our tool, the assistant **drafts the booking message** and opens the shop's chat for the user to send. It then invites the shop to join.

| Item | Detail |
|---|---|
| First market | Barber shops in one Bangkok district near campus |
| Payer | The shop (paid tier to test at ฿199-299 a month). Customers use it free in the MVP |
| Why now | Abdul proved Thais will use and pay for an assistant in LINE (฿83-409 a month). No one books Thai local services from LINE [S, market §2] |
| Biggest risks | (a) Barbers are walk-in only and won't adopt. (b) LINE, LINE MAN Wongnai (which owns JERA Cloud) or Abdul copies the idea. (c) Users don't want an AI to book for them; there is no Thai data either way |
| AI depth (for the senior project) | High: Thai time and intent parsing, tool-calling agent, a confirmation gate enforced in code, measured on a test set of real Thai booking phrases |

---

## 2. Background: Abdul as the baseline

What Abdul does today [V, teardown §2-4]:
- **Reminders:** natural-language Thai reminders (including recurring ones and Buddhist holy days), with Done and snooze buttons and an escalation mode.
- **Group mode:** reminds tagged members in LINE groups and posts daily summaries.
- **Memory:** notes, files and links, with search.
- **Calendar:** reads and writes Google Calendar.
- **Extras:** image generation, a LIFF mini-app and a web workspace.
- **Pricing:** free, ฿83, ฿125 or ฿409 a month. Payment by PromptPay or Stripe.
- **Stack:** Next.js and Cloudflare; Railway (Singapore); OpenRouter (DeepSeek and Gemini); Voyage AI embeddings; PostHog.

What Abdul cannot do: book, order, buy, or contact a business. **This gap is our opening.**

We assume Abdul could add booking itself, since it already writes to Calendar [I, teardown §6]. So our moat cannot be the chatbot. It has to be the **network of shops that have opted in**.

---

## 3. Problem

**Customer.** Booking a barber means LINE-ing or phoning a shop whose owner is busy cutting hair. The customer waits for a reply, forgets the time, and then gets no reminder. Abdul can remind, but the customer still does all the booking work [I].

**Barber or salon owner:**
- They cannot answer LINE while cutting.
- Booking requests arrive by LINE, phone and walk-in, and live on paper.
- No-shows waste paid chair time.
- LINE OA auto-reply cannot see which barber is free [S, hdmall.co.th, via execution-tech §2a].
- LINE Thailand says over 90% of new OA chats get no follow-up after the first reply [S, market §4].

**Why existing tools don't solve it:**
- Fresha is free but is a separate app and marketplace, and it charges 20% on new clients [V, market §2].
- Thai LINE booking and chat tools cost ฿499-3,300 a month and are aimed at clinics and larger shops [S, market §2; verify].
- None of them has a customer-side assistant that books across shops from the customer's own LINE [I].

**These pains are our inference.** Interviews must confirm them (section 14).

---

## 4. The idea, refined: what changed and why

| Original idea | What research showed | Revised |
|---|---|---|
| An AI that can book anything | No open booking APIs in Thailand; platforms hand off to their own checkout [V/S] | Book only into shops on **our** merchant tool; for every other shop, a drafted message the user sends [I] |
| The bot messages the shop's LINE for you | A LINE bot cannot reach another OA; automating a personal account breaks LINE's terms [V, verify #4; S] | The drafted message opens the shop's chat via its LINE link, and the user taps send [I] |
| The AI phones the shop | ฿15-25 per booking, which is the whole subscription price; scam-call fear; unclear rules on AI calls [I/S] | Out of the MVP. A possible later paid add-on |
| A browser agent fills in web forms | Agents fail most at logins and forms; CAPTCHAs and site terms block them [S] | Out of scope |
| Users pay a subscription | People book about once a month; only about 5% of ChatGPT users pay [S] | **Shops pay.** Users are free in the MVP; a bundled reminder tier for users comes later |
| Every vertical | Clinics bring health data (PDPA s.26) and an incumbent; LMWN/JERA is strong in salons; Thais walk in to restaurants [S, verify #8, market §3] | **Barbers first**, salons only after the pilot works |

---

## 5. Objectives

### 5.1 Project objectives (senior project, measurable)

| # | Objective | Target | When |
|---|---|---|---|
| O1 | Validate before building | 15 barber interviews and 20 customer interviews, plus a fake-door test (section 14), each recorded as pass or fail | Weeks 1-4 |
| O2 | Understand Thai booking requests | ≥90% of slots correct (shop, service, date, time) on a test set of 200 real Thai booking phrases | Month 3 |
| O3 | Book safely | 0 bookings without an explicit user tap; 0 double bookings in 500 scripted race tests; 0 actions caused by prompt-injection test messages | Month 4 |
| O4 | Real supply | 20-30 barbers onboarded and taking bookings in one district | Month 6 |
| O5 | Real use | ≥150 bookings completed through the system in the pilot; ≥40% of users book again within 6 weeks | Month 6-7 |
| O6 | Measurable value to shops | No-show rate and the owner's booking time, before and after, in ≥5 shops | During the pilot |
| O7 | Willingness to pay | ≥5 shops pay or sign a letter of intent at ≥฿199 a month | Month 7 |
| O8 | Protect data | PDPA notice and consent; deletion; audit log; 0 cross-user leaks in testing | Month 5 |

### 5.2 Business objectives (after the project)
- 150-300 paying shops within 12 months of launch, in barbers and then salons. This is a conservative plan [I].
- Decide at month 12, with real data, whether to expand to salons and other cities or stay a small business.

---

## 6. Scope

### 6.1 In scope (MVP)
- **Vertical:** barber shops (one or more chairs) in one Bangkok district.
- **Channels:**
  - LINE OA for customers and owners (Messaging API, LIFF, Rich Menu).
  - LIFF booking page per shop.
  - Web dashboard for the owner.
- **Customer assistant:**
  - Thai booking by chat, a confirmation card, then booking.
  - Reschedule and cancel.
  - Calendar write and reminders (Abdul baseline, booking-related only).
  - The drafted-message fallback.
- **Merchant tool:**
  - Shop setup in under 10 minutes: services, barbers, opening hours.
  - Calendar, walk-in blocking, daily schedule pushed in LINE.
  - Customer reminders and a no-show record.
  - A share link and QR code.

### 6.2 Out of scope (MVP)

| Item | Reason |
|---|---|
| Deposits, prepayment, holding money | Payment Systems Act licence. Later, only through the shop's own PromptPay, with a lawyer's review [S, legal §2.4] |
| AI phone calls | Cost and legal uncertainty [I/S] |
| Browser agents and scraping booking platforms | Unreliable; breaches the platforms' terms [S] |
| Clinics or any medical detail | Health data under PDPA s.26; clinic advertising rules [S] |
| Restaurants | Walk-in culture; Wongnai, Hungry Hub and ChatGPT already cover them [S] |
| General assistant features beyond booking (notes, files, image generation) | Abdul already does these; not our edge [I] |
| Native mobile apps | LINE is the channel |
| Becoming a Google Reserve partner | Possible later (Thailand is listed; needs Google's approval) [V, verify #3] |

### 6.3 Assumptions and constraints
- **Team:** 3 members, about 15-20 hours a week each, for about 7 months.
- **Supply:** 20-30 barbers can be reached near campus (UNVERIFIED).
- **LINE costs:** the free reply quota ends 1 Feb 2027 (25,000 replies a month, then ฿5,000 a month) [V, verify #6].
- **Merchant consent:** every merchant opts in. Every message to a merchant says it was sent by an AI assistant for a named customer [I].

---

## 7. Users and stakeholders

| User | Role | Pays? | Main need |
|---|---|---|---|
| Customer | Asks the assistant to book, confirms, gets reminders | No (MVP) | Book in one message; never forget |
| Barber or shop owner | Sets up the shop, sees bookings, gets the daily schedule | Yes (paid tier) | Fewer chats while cutting, fewer no-shows, full chairs |
| Barber (staff) | Sees own queue | No | Know who's next |
| Project supervisor | Grades scope, AI depth, evaluation | - | Measurable technical contribution |
| LINE (platform) | Channel and pricing owner | - | Platform risk; also the LINE Thailand agent developer program (Pioneer, LINE HACKS 2027) [S, verify] |

---

## 8. Features

### 8.1 Baseline taken from Abdul (only what booking needs)
1. Thai natural-language time parsing ("เสาร์บ่าย", "พรุ่งนี้หลังเลิกงาน" = "tomorrow after work").
2. Push reminders with Done and snooze.
3. Memory of the user's usual shop, barber and service.
4. Google Calendar write.
5. LIFF mini-app; PDPA notice; delete-all.

### 8.2 What we add (the "execute" layer)
1. **Booking agent:** tool calling with narrow typed tools (`search_availability`, `hold_slot`, `confirm_booking`, `cancel_booking`, `draft_line_message`, `create_calendar_event`, `set_reminder`) [I, execution-tech §3].
2. **Confirmation gate:** a Flex card showing shop, barber, service, price, date (with weekday) and time, with **Confirm** and **Edit** buttons. The tap is checked in code, never by the model. The Edit button matters legally: ETA s.17/1 lets a user withdraw an input error when an automated system gave no chance to correct it [V, verify #13].
3. **Booking state machine:** draft → held → confirmed → reminded → done, cancelled or no-show. Idempotency keys; holds expire; no double booking.
4. **Drafted handoff:** for a shop not on the system, the assistant writes the Thai message, opens the shop's LINE chat, sets a follow-up reminder, and attaches an invite link for the shop.
5. **Merchant tool:** LIFF booking page, staff calendar, walk-in blocking, daily LINE schedule, no-show tracking, customer reminders.
6. **Audit log:** append-only record of the user's request, the model's proposal, the tap, the tool result and the merchant's action.
7. **Safety:** shop and web text treated as data; rate limits per user and per shop; shops can block a user.

### 8.3 Priority (MoSCoW)

| Priority | Feature |
|---|---|
| Must | Merchant setup, calendar, LIFF booking page, daily schedule |
| Must | Customer booking agent with the confirmation gate, state machine, idempotency |
| Must | Reminders to the customer (and to the shop) |
| Must | Reschedule and cancel; no-show record |
| Must | Audit log, PDPA notice and consent, deletion |
| Should | Memory of the usual shop, barber and service ("ร้านเดิม", "same shop") |
| Should | Drafted handoff plus the merchant invite link |
| Should | Google Calendar write |
| Should | Owner dashboard: bookings, no-shows, new customers this month |
| Could | Merchant AI that answers booking questions in the shop's own OA chat |
| Could | Group booking ("book for me and Nok") |
| Won't (MVP) | Deposits, calls, browser agents, clinics, restaurants |

**Drop order if late:** Google Calendar → group booking → merchant AI chat → dashboard beyond 3 numbers.

### 8.4 Core loop
1. A customer types in LINE: "ตัดผมเสาร์นี้บ่ายสอง ร้านเดิม" ("haircut this Saturday at 2pm, same shop").
2. The agent fills in the shop, service and time, checks availability, and holds the slot.
3. A Flex card appears; the customer taps Confirm.
4. The booking is confirmed. The shop sees it in its LINE schedule, and it is written to the customer's calendar.
5. Reminders go out the day before and 1 hour before. The customer arrives, the shop marks them done, and the agent learns "ร้านเดิม" ("same shop").
6. If the shop is not on the system: a drafted message, the user sends it, a follow-up reminder, and an invite for the shop to join.

---

## 9. Market and beachhead

| Item | Number | Tag |
|---|---|---|
| LINE users in Thailand | 56M | [S] |
| Barber shops | about 13.5k nationally. Bangkok counts disagree: 2,501 vs 489 depending on the directory | [S, weak, verify #11] |
| Beauty salons | about 31.3k (Bangkok about 8.3k) | [S, weak] |
| Private clinics (not a target) | 40,552 | [S, confirmed] |

Market size [I, market §6]:
- **Merchant SAM** (Bangkok barbers and salons): about ฿65M a year at ฿500 a month. At our lower ฿199-299 price it is **about ฿25-40M**.
- **Obtainable in 3 years:** about ฿1-3M a year.
- This is **not startup-scale on barbers alone**. The upside needs salons, other cities, a consumer tier, or other LINE markets.

Beachhead is barbers because [I, market §7; verify]:
- the booking is simple (one service, one barber) and repeats every 3-5 weeks;
- owners cannot answer chat while cutting;
- there is little LINE-native competition;
- LMWN/JERA is strong in salons and clinics, not barbers.

**Contrarian evidence:**
- Many barbers are walk-in only.
- Hungry Hub pivoted away from reservations because Thais walk in [S].
- There is no Thai data on whether people want AI to book for them. The "26%" figure is a global survey, not a Southeast Asian one [V, verify #16].

---

## 10. Competitors and our difference

| Player | What it does | Threat | Our difference |
|---|---|---|---|
| Abdul | Reminders in LINE; writes to Calendar | High (same channel; could add booking) | It has no shop network. We are the booking layer and could even partner with it |
| LINE / LY Corp | Japan: Agent i assistant; bought Toreta (Jan 2026); AI phone booking for restaurants (Oct 2026). Thailand: ActEngine AI and Seller Agent for merchants; an agent developer program | Highest | Barbers are too small a niche for LINE to target first; apply to its agent program [S, verify #7] |
| LINE MAN Wongnai + JERA Cloud | JERA does salon, clinic and spa booking at 1,700+ branches; Wongnai Reservations for restaurants | High in salons and clinics | Barbers first; a customer-side assistant [S, verify] |
| Fresha | Free scheduler; 20% fee on new clients; ChatGPT/Claude integration (Aug 2026) hands off to Fresha | Medium | LINE-native; no fee on regular customers; Thai-first [V/S] |
| Goragod, Chinda, Zaapi, KaoJai, SleekFlow | LINE chat and booking tools, ฿499-3,300 a month | Medium | Cheaper; built for barbers; customer-side agent |
| ChatGPT, Google AI Mode | Book through OpenTable, Resy, Booksy; Google's booking is not available in Thailand | Low for now | Thai barbers are not on their partners [V, verify #8-9] |

**Moat** [I]:
1. Opted-in shops, densely clustered.
2. Thai booking-phrase data and an evaluation set.
3. Habit inside LINE.

It is a weak moat early on. Speed and density matter more than features.

---

## 11. Business model

### 11.1 Price to test (a hypothesis)

| Tier | Price | Includes |
|---|---|---|
| Shop Free | ฿0 | Booking page, calendar, bookings from our assistant, up to 50 bookings a month |
| Shop Pro | ฿199-299 a month (test both) | Unlimited bookings, customer reminders, no-show tracking, multiple barbers, dashboard |
| New-customer fee (test later) | Flat ฿10-20 per *new* customer the assistant brings | Invoiced to the shop monthly. Never on regulars, never taken from customer payments |
| Customer | Free in the MVP | Later, possibly bundled with reminders (Abdul charges ฿83-125) |

Fresha charges 20% on new clients and nothing on regulars; Thai tools cost ฿499+ [V/S]. Our price sits below the Thai tools.

### 11.2 Unit cost [I, legal §4]
- **AI cost:** ฿0.1-0.2 per booking on Claude Haiku 5.5 ($0.10/$0.50 per million tokens [V]). Sonnet 5.5 is used only as a fallback (about ฿1-2).
- **LINE push messages:** ฿0.06-0.10 each plus VAT. About 5 pushes per booking, so about ฿0.5.
- **Variable cost per shop with 100 bookings a month:** about ฿60-70. At ฿249 the gross margin is about 70-75%.
- **Fixed cost:** about ฿4,000 a month now and about ฿9,000 from Feb 2027 (the reply plan).
- **Break-even:** about 50 paying shops at ฿249, excluding salaries.

### 11.3 Go-to-market (first 30 shops)
1. Walk the district; sign shops face to face with QR stickers for the mirror or counter.
2. Customers pull shops in: every drafted handoff invites the shop.
3. Campus launch: students as the first customers.
4. Apply to LINE Thailand's agent developer program.

---

## 12. Legal and compliance

| Area | Rule | Our design |
|---|---|---|
| Electronic Transactions Act s.15 | A message sent by a system the user set up to act automatically is the user's message [V, verify #13] | Bind only with an explicit tap; store it in the audit log |
| ETA s.17/1 | A user may withdraw an input error if no chance to correct it was given [V] | An Edit step on every confirmation card |
| PDPA | We are the controller for user data. Cross-border LLM transfers follow s.28-29 [S] | Privacy notice listing vendors and regions; zero-retention LLM routing; deletion; a data-processing clause in the merchant agreement |
| PDPA s.26 | Health data is sensitive | No clinics in the MVP |
| Computer Crime Act s.11 | Unwanted messages need an easy opt-out [S] | Shops opt in; every message has a stop option |
| Payment Systems Act | Holding or transferring money needs a licence [S] | Never touch money. Deposits come later, only to the shop's own PromptPay, with a lawyer's review |
| Draft AI Act (ETDA, July 2026) | Risk-based; transparency; reported strict liability; not enacted [S, verify #14] | Always disclose that a message came from an AI assistant; keep a human-confirm design |
| LINE terms and pricing | No bots on personal accounts; one OA per group; reply quota from Feb 2027 [V] | Merchant opt-in only; budget ฿5,000 a month |

Items for a lawyer before launch: terms of service and the merchant agreement, our liability for wrong bookings, the cross-border legal basis, and any deposit flow.

---

## 13. Technical approach

| Layer | Choice |
|---|---|
| Channel | LINE Messaging API webhook; LIFF (booking page, shop setup, confirmation); Rich Menu |
| App | Next.js (TypeScript) |
| Database | Postgres (Supabase) with Row Level Security per shop and per user |
| Jobs | Queue for reminders, hold expiry and retries |
| LLM | Claude Haiku 5.5 by default; Sonnet 5.5 for messy requests; strict tool schemas. Compare with DeepSeek, Gemini and Typhoon on the Thai test set |
| Calendar | Google Calendar API (events scope, opt-in) |
| Observability | Audit log table; error tracking; per-booking traces |

Core data model: shop, staff, service, opening hours, slot or hold, booking (with state and idempotency key), customer, customer preference ("usual shop"), message log, audit event, consent.

Agent safety rules, all enforced in code:
1. Tool calls may come only from the user's own turns.
2. No booking, cancellation or merchant message without a confirmed tap.
3. Never report success without a confirmed tool result.
4. Text from shops and the web is treated as data.
5. Rate limits; a cap on active bookings per user per time window.

Testing:
- The Thai phrase set: 200 real phrases, with slot accuracy measured.
- Scripted races for double booking.
- A prompt-injection test set.
- End-to-end LINE tests in a sandbox OA.
- A pilot with real shops.

---

## 14. Validation and timeline

### 14.1 Validation first (weeks 1-4, before production code)

| Test | Pass mark | Stop or pivot if |
|---|---|---|
| 1. Interview 15 barbers | ≥8 take bookings by LINE or phone and name chat load or no-shows as a top-3 pain | Most are walk-in only, so switch to salons or drop the idea |
| 2. Interview 20 customers (students and workers) | ≥10 book a barber or salon at least monthly and would let the assistant book after a confirmation tap | Fewer than 6 |
| 3. Fake-door LINE OA or landing page for "จองตัดผมใน LINE" ("book a haircut in LINE") | ≥100 sign-ups or ≥5% conversion from campus posts | <30 sign-ups |
| 4. Concierge test: the team books by hand for users for 2 weeks | ≥30 bookings requested; repeat requests | <10 requests |
| 5. Shop commitment | ≥5 shops agree to pilot; ≥2 say they would pay ≥฿199 | 0 willing to pay |
| 6. Tech spike | LIFF and webhook work; ≥85% slot accuracy on the first 100 phrases | <70%: rethink the model or prompting |

### 14.2 Build plan (7 months) [I]

| Month | Build | Exit test |
|---|---|---|
| 1 | Validation tests; data model; LINE and LIFF skeleton; collect Thai phrases | Pass marks in 14.1 |
| 2 | Merchant setup, calendar, booking page, daily schedule | A shop sets up in <10 minutes |
| 3 | Booking agent, confirmation gate, state machine | ≥90% slot accuracy; 0 bookings without a tap |
| 4 | Reminders, reschedule and cancel, drafted handoff, audit log | 0 double bookings in 500 races; injection tests pass |
| 5 | Pilot with 10 shops; PDPA notices, deletion | ≥50 real bookings |
| 6 | Grow to 20-30 shops; memory of the usual shop; dashboard | ≥150 bookings; repeat rate measured |
| 7 | Hardening, price test, final report and demo | ≥5 shops pay or sign a letter of intent at ≥฿199 |

---

## 15. KPIs

| Area | Target |
|---|---|
| Slot accuracy on Thai phrases | ≥90% |
| Bookings without a user tap | 0 |
| Booking completion (request to confirmed) | ≥80% for shops on the system |
| Shops active weekly after 4 weeks | ≥60% of onboarded shops |
| Customer repeat booking within 6 weeks | ≥40% |
| No-show change in pilot shops | Measured; target a 30% reduction |
| Paying shops by month 7 | ≥5 |
| LINE pushes per booking | ≤5 |

---

## 16. Gate check (the team's own rules)

| Gate | Result | Why |
|---|---|---|
| G1 No bypass | **PARTIAL** | A customer can still LINE their barber directly. Shop-side value (calendar, reminders) does not depend on that. Shops can leave for Fresha |
| G2 Clear payer and real saving | **UNPROVEN** | The shop is the payer, but chat load and no-shows are inferred, not measured |
| G3 Buildable, no dominant giant | **PASS with caution** | Buildable in 7 months. LINE, LMWN/JERA, Fresha and Abdul could each copy it |
| G4 Startup scale | **FAIL on barbers alone** | Barbers and salons are about ฿25-65M a year. Upside needs more verticals, more cities or other markets |
| G5 Evidence | **PARTIAL** | Desk research only; demand data is missing |

---

## 17. Budget (pilot phase) [I]

| Item | Per month |
|---|---|
| LINE OA (Pro, including VAT) | about ฿1,900 |
| Hosting (Railway or Supabase plus queue) | about ฿1,000-1,700 |
| LLM (pilot volume) | <฿300 |
| Domain, monitoring | about ฿500 |
| **Total** | **about ฿3,500-4,500**. From Feb 2027 add ฿5,000 if replies exceed 25,000 a month (unlikely in the pilot) |
| One-off | QR stickers and printing about ฿1,500; legal review ฿30-80k later (UNVERIFIED) |

---

## 18. Risks

| Risk | Likelihood / impact | Mitigation |
|---|---|---|
| Barbers are walk-in only and won't adopt | High / high | Test 1 first; pivot to salons; sell the walk-in queue and no-show tools |
| Customers don't want an AI to book | Medium / high | Tests 2-4; the confirmation tap keeps control with the user |
| LINE, LMWN or Abdul copies it | Medium-high / high | Density in one district; move fast; join LINE's program; possible partnership with Abdul |
| LINE changes pricing or policy | Medium / medium | Budget the ฿5,000 plan; keep pushes ≤5 per booking |
| Wrong booking or hallucination | Medium / medium | Confirmation gate with Edit; read back the weekday; audit log |
| Prompt injection or fake bookings | Low-medium / medium | Data-only handling; rate limits; shops can block users |
| Scope creep (calls, clinics, deposits, general assistant) | High / medium | Out-of-scope list; drop order |
| Two-sided cold start | High / high | The shop tool is useful on its own; customers pull shops in; one district |

---

## 19. Decisions made while auditing the research

1. **Payer:** the shop pays; customers are free. The legal file's ฿99 consumer subscription is not the main model.
2. **LINE reply charge:** the official LINE Thailand notice of 30 Oct 2026 replaces the secondary source.
3. **AI cost per booking:** ฿0.1-0.2 on Claude Haiku 5.5. The two files differed only in their token assumptions.
4. **Break-even:** the legal file's table (120-320 users) applied to a consumer model. Recomputed for shops: about 50 shops at ฿249.
5. **"26% want agentic AI":** relabelled as a global survey, not a Southeast Asian or Thai one [verify #16].
6. **Fresha's AI integration:** dated August 2026, not September [verify #1].
7. **Barber counts:** marked weak, since the directories disagree.
8. **First vertical:** barbers, not salons, because LMWN/JERA is in salons and clinics.
9. **ETA s.17/1:** added as a legal reason for the Edit step.
10. **Shop price:** cut to ฿199-299, below the ฿499 Thai tools, because Fresha is free.

---

## 20. Comparison with the tutoring idea (ClassLedger, `2026-11-04-project-proposal.md`)

| Criterion | Tutoring / class-school tool | AI booking assistant (barbers) | Edge |
|---|---|---|---|
| Who pays | School owner, ฿390-1,490 a month | Barber shop, ฿199-299 a month | Tutoring (higher price, one payer) |
| Sides to win | One (the school brings its own parents) | Two (shops and customers): cold start | **Tutoring** |
| Pain evidence | Desk evidence of an admin cost (฿14-25k salary), slip fraud, chasing unpaid fees | Inferred chat load and no-shows; no Thai demand data | Tutoring |
| Market size | About 1,000-1,500 active schools; G4 fails | About 13.5k barbers and 31k salons, but a lower price; G4 fails on barbers alone | Even (both small) |
| Competition | Tellfox, iDotutor, GMS, CodexLearn; no one does the full loop | LINE/LY, LMWN+JERA, Fresha, Abdul, Thai LINE tools | Tutoring (weaker rivals) |
| Platform risk | LINE costs; slip-check vendors | LINE costs and policy, and LINE as a competitor | Tutoring |
| Legal risk | Medium: minors' data, tax exports; never hold money | Low-medium: ETA attribution, PDPA; never hold money | Booking |
| AI depth for grading | Low (an AI report draft, optional) | **High** (Thai NLU, tool-calling agent, safety gate, evaluation set) | **Booking** |
| Demo appeal | Solid but "admin software" | Strong: "type in LINE and it books" | Booking |
| Build risk in 7 months | Medium (payments, ledger correctness) | Medium (agent reliability, two products) | Even |
| Team access to users | Unknown (school owners) | Easy: barbers near campus, students as customers | **Booking** |
| Startup upside | Small business; adjacent verticals later | Small at first; larger if it expands to a general "do it for me" assistant across verticals and LINE markets | Booking (more option value, more risk) |
| Gate summary | G1 PASS, G2 UNPROVEN, G3 PASS with caution, G4 FAIL, G5 PARTIAL | G1 PARTIAL, G2 UNPROVEN, G3 PASS with caution, G4 FAIL, G5 PARTIAL | Tutoring slightly |

**Reading:**
- **Tutoring is the safer business:** one-sided, a clear payer at a higher price, and weaker rivals.
- **The booking assistant is the stronger senior project:** real AI depth, easier user access, a better demo, and more upside. But its demand is unproven, it has a two-sided cold start, and LINE itself is a competitor.

**Recommendation:** don't decide on opinions. Run a **2-week test sprint on both ideas in parallel**:
- 15 barbers plus 20 customers;
- 10-15 school owners.

Pick the idea that passes its pass marks:
- If both pass, choose **booking when the advisor weighs AI depth**, and **tutoring when the goal is paying customers soonest**.
- If neither passes, revisit.

---

## 21. Questions for the team

1. Which matters more for this project: AI depth for grading, or paying customers?
2. Can each member reach 5 barbers near campus, and 5 school owners, this month?
3. Do we accept a two-sided product (shops and customers) and its cold start?
4. Should we approach Abdul as a partner (they remind, we book) rather than a rival?
5. Should we apply to LINE Thailand's agent developer program (Pioneer, LINE HACKS 2027)?
6. Is keeping deposits out of the MVP final?

---

## 22. Key references
- **Abdul:** https://abdul.chat/ ; https://abdul.chat/en/privacy-policy ; https://abdul.chat/en/terms-of-service ; https://page.line.me/319gqomd
- **LINE:**
  - Messaging API: https://developers.line.biz/en/docs/messaging-api/sending-messages ; group chats https://developers.line.biz/en/docs/messaging-api/group-chats/ ; pricing https://developers.line.biz/en/docs/messaging-api/pricing/
  - Thailand reply-message notice: https://www.lineforbusiness.com/th/news/20261030
- **Claude pricing:** https://platform.claude.com/docs/en/about-claude/pricing
- **Booking platforms:**
  - Fresha: https://fresha.com/pricing
  - Fresha AI integration: https://www.cxnetwork.com/cx-experience/articles/fresha-goes-conversational-with-native-claude-and-chatgpt-integration-for-beauty-bookings/amp
  - Google Actions Center: https://developers.google.com/actions-center
- **Law:**
  - ETA (MDES): https://www.mdes.go.th/law/detail/3616-ELECTRONIC-TRANSACTIONS-ACT--B-E--2544--2001-
  - Draft AI Act: https://www.tilleke.com/insights/thailand-releases-new-draft-artificial-intelligence-act
  - Cross-border transfers: https://www.tilleke.com/insights/thailands-regulations-for-cross-border-personal-data-transfer-come-into-force
- Full source lists are in each research file.
