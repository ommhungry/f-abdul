# Validation report: omm's business and architecture brief

- Target: `docs/research/business-and-architecture-brief.md` (version 0.1, research date 6 Nov 2026, 743 lines)
- Validator run date: 9 Nov 2026. All web sources below were accessed on 9 Nov 2026 unless another date is given.
- Revised copy: `docs/review/revised/omm/business-and-architecture-brief.md`
- Post-judge corrections (9 Nov 2026): after the judge's verdict (`docs/review/judge/judge-omm-brief.md`), this report was corrected on claims #6, #7, #11, #31, and #33, finding L1, architecture items A1, A4, and A9, and the S06 citation note. The verdict counts are unchanged.
- Tags: `[V]` verified on a primary or official page that the validator opened; `[S]` secondary source (law firm, press, reseller, or the jui red-team file `2026-11-08-verify-agent-proposal.md`, which itself opened primary pages on 8 Nov 2026); `[I]` validator inference; `UNVERIFIED` could not be checked.
- Method: desk research only. Every cited URL in section 13 was opened. No sign-ups, no chats with any bot, and no probing of Abdul (see the Abdul research boundary in `TEAM-BRIEF.md`).

## 1. Summary verdict

**Sound research, wrong size.** The brief is careful and honest. Most facts hold up. Of the 30 factual claims checked, 22 are CONFIRMED, 5 are WEAKER, 3 are UNVERIFIED, and none is factually WRONG. Three important facts are missing. One planning assumption is wrong for this project: the paid pilot (see L1). It is very good at separating advertised features from proven ones, and "sent" from "confirmed". Its security thinking is the strongest of the three member contributions.

The main problems are about fit and freshness, not truth:

1. **The scope fits a funded startup, not this project.** The brief plans a paid pilot, a Temporal workflow cluster, independent security review, merchant contracts, payments, voice calls, Gmail ingestion, employer sales, and a break-even at about 4,800 paying users. The team has about 225–300 person-hours in total until 30 April 2027 (3 people × 3–4 h/week × about 25 weeks) `[I]`. That is enough for one well-tested core flow, not this plan.
2. **LINE message economics are out of date.** The brief says reply messages do not count. That is still true today `[V]`. But LINE Thailand announced on 30 Oct 2026 that from 1 Feb 2027 only 25,000 Reply API messages a month are free, and above that an "Unlimited" plan costs ฿5,000 a month `[V]`. The brief also ignores the Free plan's hard cap of 300 push messages a month, which is the plan a student prototype would start on `[V]`.
3. **The competitor picture is incomplete.** The brief lists Abdul, Lamai, and Cole correctly. It misses the platform threat that jui's research found: LY Corp's Agent i, LINE Thailand's ActEngine AI, and LINE's planned agent APIs and Pioneer Program `[S]`.
4. **Two citations point at the wrong kind of page.** The MEA link is a staging host (`awspoc-www`, menu items marked `_test`). The ETDA 1212 page shows the questions but no answers when it loads. A third, Bangkok Spin, renders no readable content, so its claim cannot be checked.
5. **One affiliate fact undermines a revenue idea.** GoWabi's affiliate terms say "Private messaging or hidden link promotion is not allowed" `[V]`. A LINE assistant that sends referral links in a private 1:1 chat may break that rule. The brief presents GoWabi as a possible referral route without this caveat.

**Recommendation for the combined proposal:** keep omm's product principles, evidence states, and security architecture. Cut the build to a single-region, single-service prototype: capture a bill or appointment → confirm the fields → schedule reminders → track completion with evidence levels. Booking becomes a "drafted handoff" (the LINE `oaMessage` URL opens a shop's chat with the message pre-filled; the user taps Send) `[V]`, not merchant integrations.

## 2. Claim table

Location = section of the original brief. Verdicts: CONFIRMED, WEAKER (partly true, or the source is weaker than stated), WRONG, UNVERIFIED.

### 2.1 Competitors

| # | Claim | Location | Verdict | Evidence | Fix |
|---|---|---|---|---|---|
| 1 | Abdul advertises personal and group reminders, recurring reminders, stored information, Google Calendar, and family or team use | §1, §4 | CONFIRMED | The abdul-ai.com landing page lists repeating reminders, group tagging, Google Calendar (paid plans), permanent memory, and family or shop groups `[V, abdul-ai.com]`. It matches jui's teardown `[S, 2026-11-08-abdul-teardown.md]`. | Also cite `abdul.chat`. The landing page links to `app.abdul.chat`, and the team brief uses that domain. |
| 2 | Abdul is free, with annual prices of ฿990, ฿1,490, and ฿4,900 | §4 | CONFIRMED | Free; Teen ฿83/month or ฿990/year; Pro ฿125/month or ฿1,490/year; Business ฿409/month or ฿4,900/year; 14-day Pro trial with no card `[V, abdul-ai.com]` | Add the monthly prices. Without them, readers compare Abdul's annual prices with omm's monthly ฿79/฿99 tests. |
| 3 | Earlier search results showed ฿99 and an early-bird ฿169 plan | §4 | UNVERIFIED | The live page does not show these prices. No archived snapshot was checked. | Mark UNVERIFIED, or delete. |
| 4 | Lamai offers tasks, groups, Google and Microsoft calendars, files, email help, voice input, and phone reminders | §4 | CONFIRMED | All listed on lamai.me `[V]`. The "phone reminder" is Lamai calling the **user** at the reminder time, not calling a business. | Say "calls the user as a reminder", so readers do not mistake it for calling a business. |
| 5 | Lamai is free, with paid tiers of ฿169, ฿349, and ฿890 a month | §4 | CONFIRMED | Origin ฿0; Pro ฿169; Collab ฿349; Team ฿890 (5 seats, +฿89 per extra seat); "prices include VAT" `[V, lamai.me]` | Add "VAT included". That matters when comparing with LINE's prices, which exclude VAT. |
| 6 | Cole (Pocodot) offers LINE help, reminders, email and calendar actions, and Thai-language phone calls | §1, §4 | CONFIRMED | The Thai page says Cole makes Thai phone calls for the user, for example booking restaurants or contacting clinics. Calls need a paid plan, cost 40 credits a minute, and Cole confirms with the user before calling. It also offers reminders, email/calendar with confirmation before sending, and help finding information ("ช่วยหาข้อมูล") `[V, pocodot.ai/th/line-ai-assistant]`. | Add the credit cost. Starter's 2,000 credits buy about 50 call-minutes a month `[I]`. |
| 7 | Cole offers research and browser tasks | §4 | Research CONFIRMED; browser tasks UNVERIFIED | The Thai page says Cole helps find information ("ช่วยหาข้อมูล") `[V]`. It does not mention browser tasks `[V, absence]`. | Keep "research"; mark browser tasks UNVERIFIED. |
| 8 | Cole has 150 introductory credits and paid plans from US$19/workspace/month | §4 | CONFIRMED | 150 one-time credits on the Free plan; Starter $19, Pro $49, Max $199 a month; 20% off yearly `[V, pocodot.ai/pricing]` | None. |
| 9 | (Missing) LINE itself is building agents | §4 | Gap | LY Corp's Agent i launched 20 Apr 2026 `[S, via jui verify #7, which cites lycorp.co.jp]`. LINE Thailand's ActEngine AI launched 12 Mar 2026 `[S, via jui verify #7]`. At LINE DEV CONF 2026, LINE announced agent APIs and MCP for third-party agents, plus a Pioneer Program `[S, via jui verify "missed" #3, thansettakij.com]`. | Add a "platform risk" row. This is the biggest strategic risk for any LINE assistant. |

### 2.2 Context, substitutes, and market numbers

| # | Claim | Location | Verdict | Evidence | Fix |
|---|---|---|---|---|---|
| 10 | The Bank of Thailand calls household debt a persistent vulnerability | §3 | CONFIRMED | The page says household debt left unsolved could impede growth and threaten financial stability, and that the debt-to-GDP ratio "is likely to exceed the watchful level of 80%" `[V, bot.or.th household-debt page]` | None. The brief correctly says this does not prove demand. |
| 11 | MEA offers bill checking, payment, and notifications | §4 | WEAKER | The content matches (bill check, history, online payment, auto-debit; notifications in the MEA Smart Life app) `[V]`. The page also lists **MEA Connect**, MEA's own LINE official account, as a bill-check channel `[V]`. But the cited host is `awspoc-www.mea.or.th`, and 12 menu items in the sustainability section are marked `_test`. It **appears to be** a proof-of-concept or staging host, although its content looks official and is dated 17 Sep 2569. | Replace with the official MEA site (production URL UNVERIFIED). Add MEA Connect as a LINE-based substitute. |
| 12 | D-MyTax is a government tax-service portal | §4 | WEAKER | The link opens a bare "E-FILING" login screen `[V]`. It is a Revenue Department host, but the page itself shows nothing that supports the claim. | Link the Revenue Department's public e-filing landing page instead. |
| 13 | Market numbers | whole brief | n/a | The brief makes almost no market-size claims. Its only numbers are illustrative economics (§9). That is a strength. | None. |

### 2.3 Booking integration matrix (§6)

| # | Claim | Location | Verdict | Evidence | Fix |
|---|---|---|---|---|---|
| 14 | Hungry Hub's terms describe real-time booking APIs for directory and portal partners, with branding and access conditions | §6 | CONFIRMED | The FAQ says Hungry Hub "is happy to provide its booking API to directories and portals … to enable real time bookings". It requires "Powered by Hungry Hub" branding and says access "may be withdrawn at Hungry Hub's absolute discretion" `[V, blog.hungryhub.com]` | None. Hungry Hub moved away from plain reservations to dining packages in 2016–2017 `[S, via jui verify #12]`, so inventory is package deals. |
| 15 | TableCheck states that fully off-site booking is unsupported and directs customers to hosted booking pages | §6 | WEAKER | The page does not use those words. It answers "Why can't I book online reservations through the API?" by saying the booking form is "too complex to represent all mandatory options via API", and tells developers to "link to our booking page" `[V, tablecheck.atlassian.net]` | Use TableCheck's own wording. The conclusion (hand off to the hosted page) stands. |
| 16 | GoWabi documents affiliate referrals | §6, §9 | CONFIRMED, with an important caveat | 5% base commission from 1 Aug 2025; monthly payout with a ฿500 minimum; 3% withholding tax. **"Private messaging or hidden link promotion is not allowed."** `[V, gowabi.com affiliate FAQ]` | Add the private-messaging rule. A 1:1 LINE chat is private messaging, so this referral route may not be allowed at all. Get written confirmation from GoWabi first. |
| 17 | Fresha Data Connector is one-way export for reporting | §6 | CONFIRMED | "a one-way sync, which means data only flows from Fresha to your selected platform" `[V, fresha.com help]`. Also, Fresha launched ChatGPT and Claude integrations (an MCP server, that is, Model Context Protocol) in August 2026. The user is sent to Fresha to confirm and pay `[S, via jui verify #1]`. | Add the MCP note. It is discovery only, not booking-write. |
| 18 | Playtomic's documented bookings endpoint only retrieves reservations | §6 | CONFIRMED | Only `GET /api/v1/bookings` ("Retrieves a list of bookings") is documented `[V, third-party.playtomic.io]` | None. |
| 19 | Bangkok Spin advertises LINE login with its own booking and payment | §6 | UNVERIFIED | The page renders only a title ("Bangkok Spin — จองคอร์ตเทนนิส"); the content loads by script. A web search found nothing more. | Mark UNVERIFIED, or drop the example. |
| 20 | No Drama Padel sends reservations to Playtomic | §6 | CONFIRMED | "Reserve one of our covered Bangkok padel courts through Playtomic" `[V, nodramapadel.com/book]` | None. |
| 21 | A LINE bot receives events only for interactions with its own official account | §6 | CONFIRMED | Events come from 1:1 chats with friends, groups, and multi-person chats the bot is in `[V, LINE receiving-messages]`. Only one official account fits in a group `[S, via jui verify #4]`. | Add the one-OA-per-group limit. |
| 22 | Twilio restricts Thai verified caller IDs for outbound calls into Thailand | §6 | CONFIRMED | "Thailand verified caller-IDs are not supported for outbound calls to Thailand." It says to use a Twilio number for domestic calls `[V, Twilio TH voice guidelines]` | None. |
| 23 | (Missing) A drafted handoff is a fully allowed booking route | §6 | Gap | LINE's URL scheme `https://line.me/R/oaMessage/{LINE ID}/?{text}` opens an official account's chat with the text pre-filled but not sent `[V, LINE URL scheme docs]` | Add it as the default booking method for the prototype (jui also recommends it). |

### 2.4 Law, privacy, AI providers, and payments (§7)

| # | Claim | Location | Verdict | Evidence | Fix |
|---|---|---|---|---|---|
| 24 | PDPA: report breaches without delay and, where feasible, within 72 hours, unless unlikely to cause risk; notify affected people of high-risk breaches; DPO criteria are statutory | §7 | CONFIRMED | Section 37(4) and section 41 match the wording `[V, PDPA unofficial translation PDF]`. The PDF is labelled "(Unofficial Translation)". | None. |
| 25 | Cross-border processing needs processor contracts and transfer safeguards; Thai hosting alone does not fix foreign model processing | §7 | CONFIRMED | DLA Piper's guide (last modified 14 Feb 2026) covers section 28–29 transfers, the 2023 notification, binding corporate rules (BCRs), appropriate safeguards, and controller–processor agreements `[S, DLA Piper]`. Cross-border rules came into force 24 Mar 2024 `[S, via jui legal file, Tilleke]`. | None. |
| 26 | The PDPC's October 2026 AI guidance covers prompts, logs, embeddings, and outputs, and is advisory | §7 | CONFIRMED | Tilleke's article is dated 14 Oct 2026. It says the guidance is "advisory rather than legally binding" and lists prompts, logs, "embeddings and vector data", and identifying outputs `[S, tilleke.com]` | Say "reported 14 Oct 2026". The exact issue date is UNVERIFIED. |
| 27 | Thailand has a 2026 draft AI Act, not an enacted law | §7 | CONFIRMED | ETDA released the draft on 2 Jul 2026 for a roughly 30-day hearing. It is risk-based, extraterritorial, and "imposes joint liability for damages regardless of willful act or negligence" `[S, tilleke.com]`. No sign of enactment by 8 Nov 2026 `[S, via jui verify #14]`. | Add the date and the strict-liability point. |
| 28 | ETDA says qualifying small platforms may still have simplified notification duties | §7 | WEAKER | The cited ETDA 1212 page shows the questions (for example, "must small businesses register?") but every answer field is empty `[V]`. Thai press reports that small or low-impact platforms must file a brief notification (แจ้งข้อมูลโดยสังเขป) under the 2022 Digital Platform Services (DPS) Royal Decree, and that ETDA is revising the decree `[S, dailynews.co.th; bangkokbiznews.com, via search]`. | Downgrade to `[S]`. Add that a non-commercial university prototype is unlikely to be a "platform business" `[I]` (not legal advice). |
| 29 | E-money, transfers, and accepting electronic payments for others may need a licence or registration | §7 | CONFIRMED | The Bank of Thailand lists e-money, "accepting electronic payment for and on behalf of others", e-money transfer, and card services as designated services needing a licence or registration `[V, bot.or.th]` | None. |
| 30 | (Missing) Electronic Transactions Act (ETA) rules on who "sent" an automated message, and on correcting input errors | §7 | Gap | Section 15 treats a message sent by a system the originator programmed as the originator's message. The 2019 amendment added section 17/1: a person who makes an input error with an automated system that gives no chance to correct it may withdraw that part `[S, via jui verify #13, which quotes the ETDA official English translation]`. | Add both. Section 17/1 is a legal reason for omm's "confirm before acting" design. |

### 2.5 Payments and LINE economics (§9)

| # | Claim | Location | Verdict | Evidence | Fix |
|---|---|---|---|---|---|
| 31 | Stripe supports customer-initiated PromptPay and subscriptions only through `send_invoice`, not automatic QR charging | §9 | CONFIRMED | "Payment confirmation: Customer-initiated"; "Invoices and Subscriptions only support the send_invoice collection method"; only Thai Stripe accounts can accept it `[V, docs.stripe.com/payments/promptpay]` | Add that a Thai Stripe account is required `[V]`. Stripe Thailand accepts individual and sole-proprietor accounts as well as companies and partnerships `[V, support.stripe.com, opened 9 Nov 2026]`, so no company is needed. But the member who opens the account is personally liable, and VAT, consumer terms, and a PDPA controller are still needed `[I]`. Payment stays out of prototype scope. *(Corrected after judge review, 9 Nov 2026: an earlier version of the revised brief wrongly said a business entity was required.)* |
| 32 | LINE Thailand's Pro plan is ฿1,780 a month for 35,000 messages, with extra messages at ฿0.06, excluding VAT | §9 | CONFIRMED | Free 300 messages; Basic ฿1,280 / 15,000 / ฿0.10; Pro ฿1,780 / 35,000 / ฿0.06. "Prices in the table do not include 7% VAT" `[V, lineforbusiness.com/th/service/line-oa-features]` | Add the Free and Basic plans. The prototype will run on one of them. |
| 33 | Push messages count toward the quota; reply messages do not | §9 | WEAKER (true today, out of date from Feb 2027) | True today `[V, LINE pricing doc]`. LINE counts messages "by the number of people you send a message to"; several message objects in one request still count once per person `[V]`. **From 1 Feb 2027:** 25,000 free Reply API messages a month, then a ฿5,000/month Unlimited plan bought through a LINE Agency Partner on the Agency Global Platform. The notice does not state VAT `[V, lineforbusiness.com/th/news/20261030]`. | Add the 2027 change, per-recipient counting, and the 300-message cap on the Free plan. The Thai page shows "-" for the Free plan's extra-message price, and the pricing doc says messages above the limit return an error and are not sent `[V]`. |
| 34 | 60 extra notifications cost about ฿3.60 before VAT; ฿99 including 7% VAT is about ฿92.52 net; contribution of ฿62.52, ฿32.52, and ฿12.52 (67.6%, 35.1%, 13.5%); about 4,800 users at ฿300,000 fixed cost; two 10-minute tasks at ฿150/h = ฿50 | §9 | CONFIRMED (arithmetic) | 60 × 0.06 = 3.60; 99 / 1.07 = 92.52; 300,000 / 62.52 = 4,798. All figures recomputed `[I]`. | Label all of these as illustrative business estimates. They are not inputs to the senior-project prototype. |

**Totals:** 22 CONFIRMED (#16 has an important caveat), 5 WEAKER (#11, #12, #15, #28, #33), 3 UNVERIFIED (#3, #7, #19), 0 factually WRONG, and 1 not applicable (#13). There are also 3 coverage gaps (#9, #23, #30). The one WRONG item is a planning assumption, not a fact: the paid pilot (§3.1, L1).

### 2.6 Dead or mismatched citations

| Source ID | Problem |
|---|---|
| S06 MEA | Appears to be a staging or proof-of-concept host (`awspoc-www`, `_test` menu items), although its content looks official. Replace it. |
| S07 D-MyTax | A login screen with no content. It does not support the claim. |
| S09 TableCheck | Live, but the brief paraphrases it more strongly than it reads. |
| S13 Bangkok Spin | Content renders by script only. The claim cannot be checked. |
| S17 PDPA PDF | Live. It is a hospital-hosted copy of the unofficial translation. That is fine, but prefer a PDPC or ETDA host if one exists (UNVERIFIED). |
| S18 DLA Piper | Live (a PDF). It is a law-firm guide `[S]`, not regulator text. |
| S21 ETDA 1212 | Questions show, but answers are empty. The cited claim is not visible. |
| S01 Abdul | Live, but it is the marketing domain. The product domain is `abdul.chat`. |

All other cited URLs (S02–S05, S08, S10–S12, S14–S16, S19, S20, S22–S30) opened and supported the claims they were cited for.

## 3. Internal logic and scope

### 3.1 Findings

| ID | Finding | Severity |
|---|---|---|
| L1 | **The paid pilot is wrong for this project.** §10 Phase B and the "Charge during pilot" mitigation assume charging real users. A student team would need a Thai Stripe account (an individual account is possible, but a member becomes personally liable), VAT handling, consumer terms, and a named PDPA controller. None of these are in scope for a senior project. Replace with a free evaluation that has a willingness-to-pay survey, or a "fake door" pricing test that collects no money. | High |
| L2 | **Effort budget.** 3 members × 3–4 h/week × about 25 weeks (9 Nov 2026 to 30 Apr 2027) ≈ 225–300 person-hours `[I]`. Exam weeks and holidays reduce this further (UNVERIFIED). The §8 architecture (Temporal, isolated scanning workers, per-provider connector workers, a human exception queue, encrypted backups with tested restore, an independent security review) would use the whole budget on infrastructure alone. | High |
| L3 | **Booking is "recommended" but has no access path.** §6 shows no integration that a student team can use for writing bookings (Hungry Hub is partner-gated, TableCheck has no API booking, GoWabi is referral-only, Fresha and Playtomic are read-only). §1 still promises "booking added where dependable merchant access exists", and §10 plans a 10–20 merchant pilot. The evidence supports a drafted handoff only. | Medium |
| L4 | **The differentiation claim needs a test.** §11 rates present differentiation 4/10. The advantage ("capture to completion with evidence levels") is plausible, but nothing in the plan measures it against Abdul. Add a comparative evaluation: the same set of Thai bills and messages, measuring field-extraction accuracy and time to a tracked obligation. | Medium |
| L5 | **Price anchors are mixed.** §9 tests ฿79/฿99 a month against Abdul's annual prices (§4). Abdul's monthly entry tier is ฿83 and Lamai's is ฿169 including VAT, so ฿79/฿99 sits at or below the market. That is consistent, but the brief should say so. | Low |
| L6 | **Founder language.** The brief says "the founder" in several places. The project now has three student members and an approver rule in `CLAUDE.md`. Use "the team". | Low |
| L7 | **Decision gates are good but sized for a startup.** The "50% week-4 retention" and "60% contribution margin" gates need hundreds of users. For about 10–20 testers, use task success rate, extraction accuracy, and System Usability Scale (SUS) scores instead. | Medium |
| L8 | **Numbers are internally consistent.** All §9 arithmetic was checked and holds (claim #34). | — |

### 3.2 Realistic prototype scope (validator proposal)

**Must have (core):**
1. Text or photo of a bill or appointment → extraction (an LLM with a JSON schema) → confirmation card (LINE Flex message or LIFF form) → stored obligation.
2. Reminder scheduling in `Asia/Bangkok`, with Buddhist-era date handling, snooze, done, and quiet hours.
3. Completion states (user-reported versus evidence attached), with a slip photo as evidence.
4. An "upcoming before payday" summary.

**Should have:**
5. Share one obligation with one invited person (item-level grant).
6. Drafted LINE handoff to a shop (`oaMessage` URL), with a follow-up reminder.

**Won't have (documented as future work):** paid plans, Temporal, Gmail, voice calls, merchant APIs, payments, employer channel.

## 4. Architecture findings (§8)

| ID | Area | Finding | Severity | Evidence |
|---|---|---|---|---|
| A1 | Webhook ingress | Correct to verify signatures over the raw body. Add the specifics: HMAC-SHA256 with the channel secret over the exact received body string, base64-encoded, compared with the `x-line-signature` header in constant time; reject on mismatch. | Low | `[V, LINE verify-webhook-signature]` |
| A2 | Webhook processing | Missing: return 2xx quickly and process asynchronously (LINE recommends this). Redelivery is **off by default** and must be turned on in the console. Even when on, it is not guaranteed and its retry count and interval are not disclosed. Redelivered events keep the same `webhookEventId` and set `deliveryContext.isRedelivery`. Events can arrive out of order, so check `timestamp`. | High | `[V, LINE receiving-messages]` |
| A3 | Idempotency key | "Deduplicate events" (§8.1) should name the key: a unique constraint on `webhookEventId` in an inbox table, written in the same transaction as the work it creates (an "inbox" pattern, the mirror of the outbox). | Medium | `[V]` + `[I]` |
| A4 | Outbound idempotency | Missing: LINE's `X-Line-Retry-Key` header on push, multicast, narrowcast, and broadcast. It makes a retried send run once, returns `409` on a duplicate, and is valid for 24 hours. Use one key per (reminder occurrence, recipient). Retry only on `500` or timeout. Treat `409` as "already accepted" and mark the send done, or crash recovery will loop. A `200` does not mean delivery (the user may have blocked the account). | High | `[V, LINE retrying-api-request]` |
| A5 | Reply versus push | Missing: LLM latency means the reply may be sent late. Reply tokens are single-use and short-lived (exact limit UNVERIFIED). A fallback to push spends quota. Use the "loading animation" feature for 1:1 chats while the model works. | Medium | `[V, LINE sending-messages]` for the loading animation; reply-token limit UNVERIFIED |
| A6 | Quota | §8.8 says "monitor LINE quotas". For the prototype this is concrete: the Free plan has 300 pushes a month and **no overage**, so pushes beyond the cap are not delivered. For example, 15 testers × 1.5 reminders a day × 30 days ≈ 675 pushes `[I]`. Budget for Basic (฿1,280 + VAT) during the evaluation month, or batch reminders into a daily digest. | High | `[V]` for plans |
| A7 | Unfollow and block | Missing: handle `unfollow` events by pausing schedules. Pushes to users who blocked the account are not counted but are not delivered either. | Medium | `[V, LINE pricing; receiving-messages]` |
| A8 | LIFF identity | Correct principle. Add specifics: send the ID token (from `liff.getIDToken()`) to the server and verify it with `POST /oauth2/v2.1/verify`, or verify the access token's channel ID and expiry. Never send profile fields from the client. Access tokens are revoked when the LIFF app closes. | Low | `[V, LIFF using-user-profile]` |
| A9 | Workflow engine | Temporal is technically sound (durable, "fully recoverable after a failure") `[V, docs.temporal.io]`, but too heavy to run for 250 person-hours. Use a Postgres-backed job table with a `SELECT … FOR UPDATE SKIP LOCKED` worker and an idempotent handler. Inserting the job row in the same transaction as the record that creates it already is the outbox; a separate outbox relay is needed only for an external queue. Keep Temporal as a documented upgrade path. | High (scope) | `[I]` |
| A10 | Scheduler correctness | Add: store reminders as UTC instants plus the original local rule (RRULE, an iCalendar recurrence rule), with `Asia/Bangkok` as the zone. Thailand has no daylight saving time (UNVERIFIED here, but widely documented), which lowers risk. A worker that crashes after sending but before marking "sent" causes a duplicate. A4's retry key closes this within 24 hours. | Medium | `[I]` |
| A11 | Group context | Add: only one official account can be in a group `[S, via jui verify #4]`. omm's choice to avoid group-chat ingestion at first is right. | Low | |
| A12 | AI provider processing | §7 and §8.5 are sound. Add a concrete provider table (region, data retention, training opt-out) to fill in during design. Abdul's public policy shows the standard to beat: zero-retention, no-training routing, and stated regions (Singapore and US) `[S, via jui teardown]`. | Medium | |
| A13 | Prompt injection | Strong already (§8.4–8.5: "Text inside a bill is data"). Keep it. Add an evaluation set of injected bills. | — | |
| A14 | Secrets | Add: keep the channel secret and channel access token in a secret store, and plan to rotate access tokens. | Low | `[I]` |
| A15 | Gmail scope | Confirmed: restricted scopes need a third-party security assessment "at least every 12 months" `[V, Google restricted-scope verification]`. The `calendar.app.created` scope exists ("Make secondary Google calendars, and see, create, change, and delete events on them") `[V, Calendar auth]`. The brief's advice holds. | — | |

## 5. Strengths worth keeping in the combined proposal

1. **Evidence states for completion** (captured → confirmed → user-reported → evidence attached → externally verified → uncertain). It is a clear, testable differentiator against "reminder-only" products, and it fits an evaluation chapter well.
2. **The architectural principle:** "AI interprets requests; independently enforced systems control access, authorize actions, and determine whether they succeeded." Use it as the combined proposal's top design rule.
3. **Approval binding:** the approved offer binds merchant, time, price ceiling, and data disclosure, and material changes need renewed approval. Together with ETA section 17/1 (an editable confirmation step), this gives a legal and technical basis for the confirm step.
4. **Thai-specific requirements** (Buddhist-era dates, `Asia/Bangkok`, "ศุกร์หน้า" ambiguity, shift workers, quiet hours, no shame-based debt reminders). They can go directly into the requirements specification.
5. **Item-level family sharing** with separate read, edit, complete, and transact permissions, and revocation that affects pending actions. A small version is buildable.
6. **Honest research limits (§2)** and "advertised, not proven" language about competitors.
7. **The booking lifecycle and the rule that a sent message is not confirmation.** These match jui's "request sent until the shop confirms" rule.
8. **The test list in §8.10** (forged webhooks, replay, prompt injection, ambiguous Thai dates, model outage with reminders already scheduled). It is a ready-made test plan.
9. **The data model (§8.2)** separates the extracted claim (with confidence and source location) from the obligation. That makes extraction accuracy measurable.

## 6. What could not be verified

- The earlier Abdul prices (฿99 and early-bird ฿169).
- Cole "browser tasks".
- Bangkok Spin's LINE login and booking claim (the page renders by script only).
- The official URL for MEA bill services, and the exact ETDA answer text on small-platform notification.
- The exact issue date of the PDPC AI guidelines (only the 14 Oct 2026 article date).
- The exact reply-token validity window.
- Whether ฿5,000 for LINE Reply Unlimited includes VAT (the notice does not say).
- LY Corp, LINE ActEngine, and LINE agent-API facts were taken from jui's red-team file (accessed 8 Nov 2026) and not reopened in this run.

## 7. Sources opened in this validation (accessed 9 Nov 2026)

- Abdul: https://abdul-ai.com/ `[V]`
- Lamai: https://lamai.me/ `[V]`
- Cole (Thai page): https://pocodot.ai/th/line-ai-assistant `[V]`; pricing: https://pocodot.ai/pricing `[V]`
- LINE OA Thai plans: https://lineforbusiness.com/th/service/line-oa-features `[V]`
- LINE Reply API notice (30 Oct 2026): https://www.lineforbusiness.com/th/news/20261030 `[V]`
- LINE pricing: https://developers.line.biz/en/docs/messaging-api/pricing/ `[V]`
- LINE receiving messages: https://developers.line.biz/en/docs/messaging-api/receiving-messages/ `[V]`
- LINE webhook signature: https://developers.line.biz/en/docs/messaging-api/verify-webhook-signature/ `[V]`
- LINE retry key: https://developers.line.biz/en/docs/messaging-api/retrying-api-request/ `[V]`
- LINE sending messages: https://developers.line.biz/en/docs/messaging-api/sending-messages/ `[V]`
- LINE URL scheme: https://developers.line.biz/en/docs/line-login/using-line-url-scheme/ `[V]`
- LIFF user profile: https://developers.line.biz/en/docs/liff/using-user-profile/ `[V]`
- Hungry Hub FAQ: https://blog.hungryhub.com/en/hungry-hub-faqs-general-qa/ `[V]`
- TableCheck: https://tablecheck.atlassian.net/wiki/spaces/API/pages/44630975/Getting%2BStarted `[V]`
- GoWabi affiliate FAQ: https://www.gowabi.com/customer_faq/gowabi-affiliates-faq-en/ `[V]`
- Fresha data connector: https://www.fresha.com/help-center/knowledge-base/reports/432-data-connector-overview `[V]`
- Playtomic: https://third-party.playtomic.io/endpoints/bookings/ `[V]`
- No Drama Padel: https://nodramapadel.com/book/ `[V]`; Bangkok Spin: https://bkkspin.com/ (no readable content)
- Twilio Thailand voice: https://static1.twilio.com/en-us/guidelines/th/voice `[V]`
- Stripe PromptPay: https://docs.stripe.com/payments/promptpay `[V]`
- PDPA unofficial translation: https://www.bphosp.or.th/pdpabgh/doc/thailand-personal-data-protection-act-2019-en.pdf `[V, unofficial]`
- DLA Piper Thailand guide: https://www.dlapiperdataprotection.com/guide.pdf?c=TH `[S]`
- Tilleke, PDPC AI guidelines: https://www.tilleke.com/insights/thailand-issues-personal-data-protection-guidelines-for-ai-systems/ `[S]`
- Tilleke, draft AI Act: https://www.tilleke.com/insights/thailand-releases-new-draft-artificial-intelligence-act/ `[S]`
- ETDA 1212: https://1212.etda.or.th/Information/Question (answers empty)
- DPS small-platform notification (Thai press, via search): https://www.dailynews.co.th/news/3734657/ ; https://www.bangkokbiznews.com/tech/1234972 `[S]`
- Bank of Thailand payment oversight: https://www.bot.or.th/en/our-roles/payment-systems/payment-act-oversight.html `[V]`
- Bank of Thailand household debt: https://www.bot.or.th/en/financial-innovation/sustainable-finance/household-and-sme/household-debt.html `[V]`
- MEA (staging host): https://awspoc-www.mea.or.th/our-services/mea-service/check-pay
- Revenue Department e-filing: https://efiling.rd.go.th/rd-efiling-web/authen/ONE
- Google Calendar scopes: https://developers.google.com/workspace/calendar/api/auth `[V]`
- Google restricted-scope verification: https://developers.google.com/identity/protocols/oauth2/production-readiness/restricted-scope-verification `[V]`
- Temporal: https://docs.temporal.io/workflow-execution `[V]`
- Reused, not reopened: `docs/research/ai-assistant-jui/2026-11-08-verify-agent-proposal.md`, `2026-11-08-agent-legal-economics.md`, `2026-11-08-agent-execution-tech.md`, `2026-11-08-abdul-teardown.md` (all 8 Nov 2026)
