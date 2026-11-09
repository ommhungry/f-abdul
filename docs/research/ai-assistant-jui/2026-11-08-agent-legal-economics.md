# Booking agent in LINE: Thai legal rules, risks, and unit economics

- Date: 2026-11-08 (access date for all sources)
- Question: can we legally and profitably build a LINE assistant that books appointments (barber, salon, clinic, restaurant) for the user, and later takes deposits?
- Baseline: Abdul only reminds; its stack and prices are in `2026-11-08-abdul-teardown.md`.
- Tags: [V] primary or official source; [S] secondary source; [I] our inference; UNVERIFIED where we are unsure.
- **We are not lawyers.** Items marked LAWYER need a Thai lawyer before launch.
- FX used: ฿33 per US$1 [I, UNVERIFIED; check the rate on the day you budget].

## 1. Summary

1. Booking with no money involved is legally light. The big duties are PDPA (consent, cross-border notice, health data) and clear attribution of who made the booking.
2. Clinics are the exception. A booking reason ("acne", "STD test") is health data and needs explicit consent. Start with barbers, salons and restaurants.
3. Never hold customer money. Deposits should go straight to the merchant's own PromptPay.
4. LINE is the main cost risk, not the LLM. From 1 Feb 2027 reply messages above 25,000 a month cost ฿5,000 a month.
5. With a cheap model (Claude Haiku 5.5 or DeepSeek Flash) the variable cost per booking is well under ฿1. Phone-call booking costs about ฿15-25 per booking and breaks the ฿83-125 price point.
6. Rough break-even: about 140-350 paying users at ฿99 a month, depending on how many free users we carry (excluding salaries).

## 2. Thai law and platform rules

### 2.1 Electronic Transactions Act B.E. 2544 (ETA)

| Point | Finding | Tag |
|---|---|---|
| Attribution | ETA s.15 follows the UNCITRAL Model Law: a data message is the originator's if sent by the originator, by a person authorised to act for them, or by an information system programmed by or for the originator to run automatically. So a booking our bot sends on the user's instruction is, in principle, the user's message. | [I] wording from memory, UNVERIFIED; official English text at the ETDA/MDES links below was a scanned PDF we could not read |
| Recipient reliance | The 2019 amendment (No. 3) replaced s.16(1). The merchant may rely on the message if it "appropriately ascertained" that it came from the originator. | [S] mdes.go.th ETA No. 3; search snippet |
| Automated contracts | The 2019 amendment added a definition of "automated message system" and new ss.13/1-13/2: a contract is not invalid only because no natural person was involved. | [S] Tilleke; MDES ETA No. 3 |
| Pending reform | ETDA held a hearing on a revised ETA (12 May to 15 June 2026) that reportedly covers automated systems and e-contracting. | [S] Tilleke |

What this means [I]:
- A booking is a contract-like data message. It binds the user only if the user really authorised it. So **we need proof of authorisation**: an explicit "Confirm" tap by the user, stored with a timestamp.
- If the bot books something the user did not ask for (hallucination, prompt injection), the user can argue it is not their message. Then the loss falls on us. LAWYER: confirm s.15/16 reading and our liability to the merchant.

### 2.2 PDPA B.E. 2562

| Topic | Rule | Our implication | Tag |
|---|---|---|---|
| Our role | We are the data controller for the user's chats, as Abdul is. | Privacy notice, lawful basis, deletion, records of processing. | [I], mirrors Abdul [V] teardown |
| Merchant data | Merchant name, phone and staff names are personal data if about individuals (sole-trader barbers). | Merchant opt-in agreement; a data processing clause. | [I] |
| Third parties | Users will mention friends ("book for me and Nok"). | Collect only what the booking needs; no profiles of third parties. | [I] |
| Cross-border (s.28-29) | PDPC notifications on cross-border transfer took effect 24 March 2024. Transfers need an adequate destination or safeguards (contract clauses, BCRs, or consent). | Sending chats to US LLM APIs is a transfer. Use vendor terms with zero retention; disclose countries, as Abdul does (Singapore, US). | [S] Tilleke on PDPC notifications |
| Records | A 2026 draft PDPC guideline would require records of all processors and cross-border transfers, with destination countries. | Keep a list of vendors and regions from day one. | [S] Tilleke |
| Health data (s.26) | Health data is sensitive. Collection needs explicit consent; exceptions are narrow. | A clinic booking with a symptom or treatment type is health data. Explicit, separate consent before we send it to an LLM or a merchant. | [S] pdpathailand.com s.26; Tilleke |
| Enforcement | The PDPC issued eight new administrative fines in August 2025. | Real risk, not theory. | [S] Tilleke |

LAWYER: whether a clinic booking with only "general check-up" is health data; whether consent is the right basis for cross-border LLM transfer.

### 2.3 AI disclosure, calls, recording, spam

| Topic | Finding | Tag |
|---|---|---|
| AI law | ETDA released a new draft AI Act on 2 July 2026. It is risk-based (like the EU AI Act), has extraterritorial reach and transparency rules for AI-generated content, and reportedly **strict joint liability** for damage. Not enacted as of our search. | [S] Tilleke |
| AI disclosure today | We found no enacted Thai rule that forces a bot to say it is a bot. | UNVERIFIED (absence of evidence) |
| Our policy | Always disclose "Booked by [product] for [user]" to the merchant. It is cheap, builds trust, and fits the draft AI Act. | [I] |
| Call recording | No single statute. Practice is close to one-party consent, but a voice recording is personal data under the PDPA and needs a lawful basis and notice. | [S] recordinglaw.com; mixed sources, UNVERIFIED |
| Telemarketing | Direct marketing to consumers falls under the Direct Sales and Direct Marketing Act B.E. 2545 (OCPB registration). A booking call is not marketing, so it is likely out of scope. | [S] Mondaq; [I] |
| Spam (Computer Crime Act s.11, as amended 2017) | Sending messages that annoy the recipient without an easy opt-out is an offence; fine up to ฿200,000. A ministerial notification sets opt-out rules. | [S] Tilleke PDF; fine level reported differently across sources |

Implication [I]: reminders the user asked for are fine. Marketing pushes, or messages to merchants who never opted in, are risky. Every merchant message needs a "stop" option.

### 2.4 Payment Systems Act B.E. 2560 (later phase: deposits)

- E-money, payment facilitation and e-money transfer services need a licence from the Minister of Finance on the Bank of Thailand's advice [S, KPMG; V, bot.or.th oversight page].
- "Fund transfer" means accepting money from a user and passing it to a receiver [S, KPMG]. Collecting a deposit and paying it to the merchant would look like this [I].
- Since 21 Feb 2026 PromptPay is a designated systemically important retail payment system [S].
- **Design rule [I]:** the user pays the merchant's own PromptPay QR directly. We only show the QR and record the slip. We never touch the money. If we later charge merchants a commission, invoice them separately. LAWYER before any deposit feature.

### 2.5 Clinics and consumer protection

- Clinic advertising needs prior approval from the Department of Health Service Support (Medical Facility Act B.E. 2541). "Advertising" includes any message that makes the public aware of a facility for its commercial benefit [S, Tilleke]. If we recommend or rank clinics, we may be advertising for them [I]. LAWYER.
- DHSS checked 4,521 posts in Oct 2025 to Mar 2026 and found 2,433 likely unlawful [S, HDmall, a commercial source].
- Consumer Protection Act: if we charge users, our claims ("guaranteed booking") must be true [I]. Say "request sent" until the merchant confirms.
- Recommendation [I]: in phase 1, book clinics only as "contact request" with no medical detail, or skip clinics.

### 2.6 LINE platform rules and prices (Thailand)

| Item | Detail | Tag |
|---|---|---|
| What counts | Push, multicast, broadcast and narrowcast count toward the quota. Reply messages do not. | [V] developers.line.biz pricing |
| Free plan | 300 messages a month | [V] lineforbusiness.com/th |
| Basic | ฿1,280 a month, 15,000 messages, extra ฿0.10 each, excl. 7% VAT | [V] |
| Pro | ฿1,780 a month, 35,000 messages, extra ฿0.06 each, excl. 7% VAT | [V] |
| Reply API change | From 1 Feb 2027: 25,000 free reply messages a month per OA; above that an "Unlimited" plan at ฿5,000 a month via a LINE Agency Partner; account must move to the Agency Global Platform. Push not mentioned. | [V] lineforbusiness.com/th/news/20261030 |
| Message counting | Whether one push with several bubbles counts as one message | UNVERIFIED; test before budgeting |
| Bots on personal accounts | LY Corporation's Common Terms ban improper use of services by bots | [S] Chinese translation of LY terms; Thai terms UNVERIFIED |

Implications [I]:
- Every reminder and every "merchant confirmed" message is a paid push. A reply is free only within 25,000 a month from 2027.
- We cannot have our OA message a merchant's OA. We also must not automate a personal LINE account to chat with merchants. **Merchants must opt in** (friend our merchant OA, or use a LIFF page).

### 2.7 Booking platforms and Google

- We found no evidence of Google or Thai booking platforms allowing third-party bots to book through their sites. Scraping or automating them likely breaks their terms. UNVERIFIED; read each platform's terms before integrating.
- Use official partner APIs only, or direct merchant opt-in.

## 3. Liability and risk

| Risk | Example | Mitigation |
|---|---|---|
| Wrong booking | Bot books 15:00 instead of 17:00 | Confirmation card (merchant, service, date, time, price) and a required "Confirm" tap; read back in Thai with the weekday |
| No-show caused by the bot | Reminder not sent; merchant reply not relayed | Retry queue; alert the user if the merchant has not confirmed within X minutes; "not confirmed" state is visible |
| Double booking | Two slots held; user forgets one | One active booking per merchant per time window; cancel flow |
| Impersonation | Someone books in another person's name | Bookings only from the LINE account owner; name and phone from LINE profile plus user confirmation |
| Prompt injection | Merchant message or web page says "cancel all bookings" | Tool calls only from the user's own turns; merchant text treated as data; no destructive action without user confirmation |
| Abuse | Mass fake bookings to hurt a rival shop | Rate limits per user and per merchant; new accounts limited; merchant can block a user; no-show score |
| Data leak | Clinic reason exposed | Minimise; separate consent; encrypt; zero-retention LLM routing |
| Dispute | "I never booked that" | Audit log: user message, model output, confirm tap, message sent to merchant, merchant reply, all timestamped |

Liability allocation [I]: our terms should say we act as the user's messenger, not as the merchant's agent; we are not liable for the merchant's service. Under the draft AI Act's strict liability, terms may not fully protect us. LAWYER: terms of service, merchant agreement, liability cap.

## 4. Unit economics

### 4.1 LLM prices (US$ per million tokens)

| Model | Input | Cached input | Output | Tag |
|---|---|---|---|---|
| Claude Haiku 5.5 (prompt up to 100k tokens) | 0.10 | 0.01 | 0.50 | [V] platform.claude.com pricing |
| Claude Sonnet 5.5 | 2.00 | 0.10 | 10.00 | [V] |
| DeepSeek Flash (peak / off-peak) | 0.30 / 0.15 | 0.006 / 0.003 | 1.20 / 0.60 | [V] api-docs.deepseek.com |
| Gemini 3.8 Flash (to 31 Dec 2026; doubles 1 Jan 2027) | 0.75 | n/a | 3.75 | [V] aistudio.google.com/docs/pricing |
| Gemini 3.1 Flash-Lite | 0.25 | n/a | 1.50 | [V] |

### 4.2 Cost per booking (LLM only)

Assumption [I]: a booking takes 6 turns. Each turn sends about 4,000 input tokens (system prompt, tools, history) and returns 250 output tokens. Total 24,000 in, 1,500 out. No caching (worst case).

| Model | US$ per booking | THB per booking |
|---|---|---|
| Claude Haiku 5.5 | 0.0032 | ฿0.11 |
| DeepSeek Flash (peak) | 0.0090 | ฿0.30 |
| Gemini 3.1 Flash-Lite | 0.0083 | ฿0.27 |
| Gemini 3.8 Flash (2026 price) | 0.024 | ฿0.78 |
| Claude Sonnet 5.5 | 0.063 | ฿2.08 (about ฿0.90 with 80% cache hits) |

Other chat per user per month (60 messages, 4,000 in / 300 out each) [I]: Haiku about ฿1.1; Sonnet about ฿22 uncached.

### 4.3 LINE cost per user per month

Assumption [I]: 4 bookings a month, each with 5 pushes (confirmation, merchant notice, merchant reply relayed, reminder the day before, reminder 1 hour before), plus 10 other reminders. Total about 30 pushes.

| Plan rate | Cost per user per month |
|---|---|
| Pro overage ฿0.06 + 7% VAT | about ฿1.9 |
| Basic overage ฿0.10 + 7% VAT | about ฿3.2 |

Fixed: Pro ฿1,905 a month incl. VAT covers 35,000 pushes (about 1,150 such users). From Feb 2027 add ฿5,000 a month once replies pass 25,000 (about 300 active users at 80 replies each) [I].

### 4.4 Phone-call booking

- Twilio outbound to Thai mobile or local: US$0.10 a minute (≈ ฿3.3); a Thai number US$22-25 a month [S, twilio.com/voice/pricing/th; page partly garbled].
- Speech-to-text, TTS and LLM add more per minute. UNVERIFIED; assume ฿3-5 a minute [I].
- A 2-3 minute call plus retries ≈ **฿15-25 per booking** [I]. At 4 bookings a month that is ฿60-100 per user, which is the whole ฿83-125 price. Voice also brings recording-consent and merchant-annoyance risk.
- Verdict [I]: no phone calls in phase 1. Use merchant opt-in via LINE. Maybe offer voice later as a paid add-on per call.

### 4.5 Hosting and fixed costs (per month) [I unless tagged]

| Item | Cost |
|---|---|
| LINE OA Pro (incl. VAT) | ฿1,905 [V price] |
| LINE Reply Unlimited (from Feb 2027, once over 25k replies) | ฿5,000, VAT unclear [V] |
| Hosting (Railway Pro US$20 plus usage, about US$50) | ฿1,650 [S, Railway price sites] |
| Domain, monitoring, error tracking, backups | ฿500 |
| Total before Feb 2027 / after | ≈ ฿4,000 / ≈ ฿9,000 |
| One-off lawyer review (ToS, privacy, merchant agreement) | ฿30,000-80,000 UNVERIFIED |

### 4.6 Gross margin per paying user

Assume price ฿99 a month (between Abdul's ฿83 and ฿125 tiers). Card or PromptPay fees about 3-4% (≈ ฿4) UNVERIFIED.

| Cost per paying user | Haiku / DeepSeek | Sonnet | Haiku + phone booking |
|---|---|---|---|
| LLM (4 bookings + chat) | ฿1.5 | ฿30 | ฿1.5 |
| LINE pushes | ฿2-3 | ฿2-3 | ฿2-3 |
| Voice | 0 | 0 | ฿80 |
| Payment fee | ฿4 | ฿4 | ฿4 |
| **Variable cost** | **≈ ฿9** | **≈ ฿37** | **≈ ฿89** |
| **Gross margin** | **≈ ฿90 (91%)** | **≈ ฿62 (63%)** | **≈ ฿10 (10%)** |

Free users cost money too. A free user costs about ฿1.5-3 a month (LLM plus pushes) [I]. If 3-5% of users convert, each paying user carries 20-30 free users.

### 4.7 Break-even (excluding salaries) [I]

| Scenario | Contribution per paying user | Fixed cost | Paying users needed |
|---|---|---|---|
| Haiku, light free tier (10 free per paid at ฿1.5) | ฿90 − ฿15 = ฿75 | ฿9,000 | **≈ 120** |
| Haiku, generous free tier (25 free per paid at ฿2.5) | ฿90 − ฿62 = ฿28 | ฿9,000 | **≈ 320** |
| Sonnet, light free tier | ฿62 − ฿15 = ฿47 | ฿9,000 | ≈ 190 |

Abdul comparison [I]: Abdul's ฿83 tier caps reminders at 100 a month. At ฿0.06-0.10 a push that is at most ฿6-10 of LINE cost, so its quotas match LINE pricing. Abdul has 41,573 friends [V, teardown]; at 3% paid that is ~1,250 paying users, far above these break-even points. Our cost structure is similar; the difference must come from the booking feature, not price.

Revenue ideas beyond subscriptions [I]: a small fee per confirmed booking paid by the merchant (it saves them a no-show or an empty slot). This needs the payment-law check in 2.4.

## 5. Recommendations [I]

1. Phase 1: salons, barbers, restaurants. Merchants opt in through our merchant LINE OA or a LIFF page. No clinics with medical detail, no money, no phone calls.
2. Required confirm tap before any booking leaves the system; full audit log.
3. Default model Claude Haiku 5.5 or DeepSeek Flash; route hard cases to Sonnet 5.5. Zero-retention settings; record regions for PDPA.
4. Disclose "sent by an AI assistant for [user]" in every merchant message; easy opt-out.
5. Budget for LINE's ฿5,000 reply plan from February 2027.
6. LAWYER before launch: ETA attribution and liability; PDPA privacy notice and cross-border basis; health-data consent; ToS and merchant agreement; any deposit flow under the Payment Systems Act; clinic "advertising" exposure.

## Sources

Official or primary [V]:
- LINE Messaging API pricing: https://developers.line.biz/en/docs/messaging-api/pricing/
- LINE for Business Thailand packages: https://www.lineforbusiness.com/th/service/line-oa-features/broadcast-message
- LINE Thailand Reply API notice, 30 Oct 2026: https://www.lineforbusiness.com/th/news/20261030
- Anthropic pricing: https://platform.claude.com/docs/en/about-claude/pricing
- DeepSeek pricing: https://api-docs.deepseek.com/quick_start/pricing/
- Gemini API pricing: https://aistudio.google.com/docs/pricing
- Bank of Thailand, Payment Systems Act oversight: https://www.bot.or.th/en/our-roles/payment-systems/payment-act-oversight.html
- ETA B.E. 2544 (MDES, scanned, not read): https://www.mdes.go.th/law/detail/3616-ELECTRONIC-TRANSACTIONS-ACT--B-E--2544--2001-
- ETA No. 3 B.E. 2562: https://www.mdes.go.th/law/detail/5099-ELECTRONIC-TRANSACTIONS-ACT--NO-3---B-E-2562-2019-
- Abdul teardown (our file): docs/research/2026-11-08-abdul-teardown.md

Secondary [S]:
- Tilleke, draft AI Act: https://www.tilleke.com/insights/thailand-releases-new-draft-artificial-intelligence-act
- Tilleke, revised ETA draft: https://www.tilleke.com/insights/thailand-set-to-overhaul-its-e-transactions-framework/11/
- Tilleke, cross-border transfer rules: https://www.tilleke.com/insights/thailands-regulations-for-cross-border-personal-data-transfer-come-into-force
- PDPA s.26 text (unofficial): https://pdpathailand.com/pdpa/content_eng/article26_eng.php
- Tilleke, spam and Computer Crimes Act 2017: https://www.tilleke.com/wp-content/uploads/2017/11/Spam-and-the-Computer-Crimes-Act-2017_0.pdf
- KPMG, Payment Systems Act: https://kpmg.com/th/en/home/insights/2023/03/legal-news-update-issue-23.html
- Tilleke, medical facility advertising: https://www.tilleke.com/insights/court-judgment-reaffirms-rules-on-advertising-of-medical-facilities-in-thailand
- HDmall, healthcare advertising rules: https://hdmall.co.th/work-with-us/en/resources/hospital-advertising-rules/
- Mondaq, Direct Sales and Direct Marketing Act: https://www.mondaq.com/advertising-marketing-branding/1372024/
- Recording law, Thailand: https://www.recordinglaw.com/world-laws/world-recording-laws/thailand-recording-laws/
- Relevant Audience, LINE Reply API change: https://www.relevantaudience.com/social-media-marketing/line-reply-api-25000-free-messages-agency-global-platform/
- Twilio Voice pricing Thailand: https://www.twilio.com/voice/pricing/th
- LY Corporation Common Terms: https://www.lycorp.co.jp/en/company/terms/
- Railway pricing summary: https://temps.sh/blog/railway-pricing-2026
