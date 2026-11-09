# Project Proposal (v2): an AI assistant in LINE that gets things done

- Working name: **TumHai (ทำให้, "I'll do it for you")**. This is a placeholder.
- Date: 2026-11-08. Version 2 replaces the barber-only v1 (`2026-11-08-agent-proposal.md`) after team feedback: the team wants a broad assistant, and businesses are reached by **requests**, not by writing into their systems.
- Status: **PROCEED WITH CHANGES**. Desk research only; no users or businesses have been interviewed yet.
- Sources (all in `docs/research/`):
  - `2026-11-08-abdul-teardown.md` (baseline)
  - `2026-11-08-agent-execution-tech.md`
  - `2026-11-08-agent-market-competitors.md`
  - `2026-11-08-agent-legal-economics.md`
  - `2026-11-08-verify-agent-proposal.md` (red team: 16 claims, 13 confirmed, 2 weaker, 1 wrong)
  - The comparison uses `2026-11-04-project-proposal.md` (tutoring, ClassLedger).
- Evidence tags: [V] primary or official source, [S] secondary source, [I] our inference, UNVERIFIED.

---

## 1. Summary

**Abdul** (abdul.chat) is a Thai AI assistant in LINE with 41,573 friends that only **reminds** [V, teardown]. TumHai is an assistant in LINE that **gets the task done**, in Thai, with the user confirming before anything leaves the system.

It works at three levels:

| Level | What happens | Examples |
|---|---|---|
| **1. Does it fully** (no business needs to join) | The assistant completes the task with the user's own accounts or inside LINE | Calendar events and invites; group polls and schedules; bill split with a PromptPay QR code to the user's own account; documents from photos or PDFs; search, compare and shortlist; reminders (the Abdul baseline) |
| **2. Sends a request** (any business) | The assistant builds a **request card** (service, 2-3 time options, name). It reaches the business, which taps Accept, Pick another time or Decline on a web page, with no sign-up. The answer returns to the user automatically. If no reply comes, the assistant nudges the user or suggests another business | Book a haircut, a table or a massage; ask a repair technician for a quote; ask a tutor or photographer if they're free |
| **3. Books directly** (businesses that opt in) | Businesses that get many requests are invited to a free LINE booking page. After that, requests become instant bookings | Grows from the request data; no type of business is chosen in advance |

**Focus:** a user group (students and staff at our university), not a type of shop. **A fixed list of 10 actions**, each with a pass mark.

**Payer:** the user, with a subscription to test at ฿99-149 a month (Abdul charges ฿83-125). Businesses get a paid tier later.

**Biggest risks:**
1. Users may not pay; worldwide only about 5% of ChatGPT users do [S].
2. Businesses may not answer request cards (unknown, so it is tested first).
3. Abdul, LINE, ChatGPT or Gemini may add "do it for me" in Thai.

**AI depth:** high. One agent framework, many tools, a confirmation gate enforced in code, and accuracy measured for each action on a Thai test set.

---

## 2. Background: Abdul as the baseline

Abdul today [V, teardown §2-4]:
- **Reminders:** natural-language Thai reminders (recurring, Buddhist holy days), with Done and snooze buttons and an escalation mode.
- **Group mode:** reminds tagged members in LINE groups; posts summaries.
- **Memory:** notes, files and links, with search.
- **Calendar:** reads and writes Google Calendar.
- **Extras:** image generation; a LIFF mini-app and a web workspace.
- **Pricing:** free, ฿83, ฿125 or ฿409 a month; PromptPay or Stripe.
- **Stack:** Next.js and Cloudflare; Railway (Singapore); OpenRouter with DeepSeek and Gemini; Voyage AI embeddings; PostHog.

What it cannot do: contact a business, split a bill, run a group poll that ends in a decision, or finish any task outside LINE and Calendar [I, teardown §6]. **This gap is our opening.** We assume Abdul can copy features. Our lead must come from execution quality, the reply network built from request cards, and moving fast.

---

## 3. Problem

**Users** (students and young workers) [I, to be confirmed by interviews]:
- Small tasks take many steps across apps: messaging a shop and waiting, chasing friends for money, getting a group to agree on a time, turning a photo of a document into something usable.
- Reminder apps note the task but leave all the work to the user.

**Businesses:**
- Requests arrive as loose chat and get lost.
- LINE Thailand says over 90% of new Official Account (OA) chats get no follow-up after the first reply [S, market §4].
- LINE OA auto-reply cannot see staff availability [S].
- A structured request card with Accept or Decline buttons is easier to handle than free text [I].

These are inferences. Section 14 tests them.

---

## 4. How the idea changed

| Earlier version | Problem found | Now |
|---|---|---|
| An AI books any shop | No open booking APIs in Thailand; a LINE bot cannot message another OA [V, verify #2, #4] | **Request cards:** the business answers by tapping, with no integration |
| v1: barbers only | The team wants broad scope; the request card works for any business | **Broad, with 10 actions;** the focus is a user group |
| v1: the shop pays | A broad assistant serves the user first | **The user pays;** businesses pay later |
| AI phone calls | ฿15-25 per call; scam-call fear [I/S] | Out of the MVP |
| SMS to the business | Thai rules: sender names must be registered and links in SMS must be registered, or they are blocked; scam-SMS fear [S, Bangkok Post; Twilio] | Later, once there is a registered company |

**How a request card reaches a business** (to be tested in the tech spike):
1. **Main route:** the user forwards the card with LINE's share picker. It sends to the user's friends and groups, which suits shops whose owners use a personal LINE account [S, LINE Developers; Classmethod]. Whether it can send to a business's OA chat is UNVERIFIED.
2. **Fallback:** the assistant opens the business's OA chat with the text and link ready to paste.
3. **Later:** registered SMS, or a direct booking page for businesses that opt in.

The business's answer always comes back through our web page, so the loop closes automatically.

---

## 5. Objectives

### 5.1 Project objectives (senior project, measurable)

| # | Objective | Target | When |
|---|---|---|---|
| O1 | Validate before building | Tests 1-5 in section 14, each recorded as pass or fail | Weeks 1-4 |
| O2 | Understand Thai requests | ≥90% intent and slot accuracy on 300 real Thai requests across the 10 actions | Month 3 |
| O3 | Act safely | 0 actions outside the system without a user tap; 0 actions caused by the prompt-injection test set; 0 duplicate actions in 500 retry tests | Month 4 |
| O4 | Get tasks done | Level 1 actions succeed ≥85% end to end. ≥50% of request cards are answered by the business within 3 hours | Month 5 |
| O5 | Real use | ≥200 pilot users; ≥30% active weekly at week 6; ≥3 completed tasks per active user per week | Month 6 |
| O6 | Willingness to pay | ≥10% of active pilot users (or ≥30 users) pay ฿99+ | Month 7 |
| O7 | Protect data | PDPA notice and consent; vendor and region list; deletion; audit log; 0 cross-user leaks in testing | Month 5 |

### 5.2 Business objectives (after the project)
- 1,000-3,000 paying users within 12 months of launch [I]. Abdul reached 41.5k friends in about 5 months, which suggests the channel can grow [V].
- At month 12, decide whether to add a business tier, expand to more user groups, or stay small.

---

## 6. Scope

### 6.1 The 10 MVP actions

| # | Action | Level | Notes |
|---|---|---|---|
| 1 | Reminders, personal and in groups | 1 | Abdul parity; needed for the follow-up on every other action |
| 2 | Calendar: create, move, invite | 1 | Google Calendar events scope. An app that is not yet verified by Google is limited to 100 test users, so verification is needed before launch [I, Google OAuth policy, UNVERIFIED detail] |
| 3 | Group poll and schedule ("when is everyone free?"), ending in a calendar event | 1 | Inside LINE groups |
| 4 | Bill split with a PromptPay QR code per person to the payer's own PromptPay; tracks who has paid | 1 | Money never passes through us [S, legal §2.4] |
| 5 | Document helper: photo or PDF to a summary, a filled template or a translation | 1 | Vision model |
| 6 | Search, compare, shortlist (places, prices, options) | 1 | Web search |
| 7 | **Request card to any business** (booking, quote, availability, price), with automatic reply handling | 2 | Core differentiator |
| 8 | Follow-up and fallback: nudge, "no reply, try these 2 others" | 2 | Uses action 6 |
| 9 | Direct booking into businesses that opt in (free booking page) | 3 | Built in month 5-6; first businesses come from the request data |
| 10 | Links for ride, delivery and maps, with details filled in | 2 | Where an app supports deep links; otherwise Google Maps [I] |

**Drop order if late:** 10 → 9 → 5 → 6. Actions 1, 2, 4 and 7 are the core.

### 6.2 Out of scope (MVP)

| Item | Reason |
|---|---|
| Holding or moving money; deposits | Payment Systems Act licence [S] |
| Sending email for the user | Google's verification of sensitive scopes adds review work; it may come later [I] |
| AI phone calls; SMS | Cost; SMS registration rules [S] |
| Browser agents, scraping, automating personal LINE accounts | Platform terms; unreliable [S] |
| Medical requests (clinics with symptoms or treatments) | Health data, PDPA s.26 [S] |
| Notes, file storage, image generation beyond what tasks need | Abdul already does these; not our edge |
| Native apps | LINE is the channel |

### 6.3 Assumptions and constraints
- 3 members, about 15-20 hours a week each, for about 7 months.
- The first 200 users come from our university (UNVERIFIED).
- **LINE costs:** replies are free up to 25,000 a month until 1 Feb 2027; above that, ฿5,000 a month [V, verify #6]. A chat assistant will pass that soon after launch, so budget for it.
- Every request card says it was sent by an AI assistant for a named user [I].

---

## 7. Users and stakeholders

| User | Role | Pays? | Need |
|---|---|---|---|
| End user (student or young worker) | Asks, confirms, receives results | Yes (subscription to test) | Tasks finished, not just noted |
| Friend group or family | Group polls, bill splits | No | Less chasing |
| Business (any) | Receives request cards; may opt in to direct booking | Later (paid business tier) | Clear requests; fewer lost chats |
| Project supervisor | Grades scope, AI depth, evaluation | - | Measurable technical contribution |
| LINE | Channel owner, pricing, policy | - | Platform risk; agent developer program (Pioneer, LINE HACKS 2027) [S, verify] |

---

## 8. Design

### 8.1 Agent design (all [I] unless tagged)
1. **Router:** a small model sorts each message into chat, one of the 10 actions, or unclear (ask the user).
2. **Planner and tools:** a tool-calling LLM with narrow typed tools, for example:
   - `create_event`, `create_poll`, `split_bill`, `make_promptpay_qr`;
   - `send_request_card`, `check_request_status`, `book_slot`;
   - `search_web`, `read_document`, `set_reminder`.
3. **Confirmation gate:** before any action that leaves the system (an invite, a request card, a booking, a group post), the user sees a Flex card with **Confirm** and **Edit**. The tap is checked in code. The Edit button follows ETA s.17/1: the user must be able to correct input errors [V, verify #13].
4. **State machines:** request goes requested → accepted, new time offered, declined, or expired → confirmed → reminded → done. Bill split goes open → part-paid → settled.
5. **Idempotency keys** on every action that leaves the system; holds and requests expire.
6. **Audit log** (append-only): the request, the plan, the tap, the tool result, the business's answer.
7. **Safety:** text from businesses and the web is treated as data, never as instructions. Rate limits per user and per business. Businesses can block a sender.

### 8.2 Stack

| Layer | Choice |
|---|---|
| Channel | LINE Messaging API, LIFF (confirmation, request page for businesses, settings), Rich Menu |
| App | Next.js (TypeScript) |
| Database | Postgres (Supabase) with Row Level Security; job queue for reminders, expiry and retries |
| LLM | Claude Haiku 5.5 by default ($0.10/$0.50 per million tokens [V]); Sonnet 5.5 for hard cases; compare with DeepSeek, Gemini and Typhoon on the Thai test set |
| Integrations | Google Calendar; PromptPay QR (EMVCo standard); web search API |
| Observability | Audit log, traces per task, success-rate dashboard per action |

### 8.3 Core loop example
1. A user types: "หาร้านตัดผมแถวมหาลัย เสาร์บ่าย" ("find a barber near the university, Saturday afternoon").
2. The assistant shortlists 3 shops (action 6) and the user picks one.
3. The assistant shows the request card; the user confirms and forwards it to the shop.
4. The shop taps "14:00 OK".
5. The user gets "Confirmed Saturday 14:00", plus a calendar event and a reminder.
6. If the shop gets more than 5 requests, the assistant invites it to direct booking (action 9).

---

## 9. Market

| Item | Number | Tag |
|---|---|---|
| LINE users in Thailand | 56M | [S] |
| Thais paying small monthly fees inside LINE | LINE Premium: about 3M subscribers at ฿169 | [S, confirmed in verify #10] |
| Abdul | 41.5k friends; ฿83-409 | [V] |
| ChatGPT in Thailand | Weekly users quadrupled in a year; Go about ฿259, Plus about ฿699 | [S] |
| Paying share (global proxy) | About 5% of ChatGPT weekly users pay | [S] |

Market size [I], with explicit assumptions:
- **TAM:** 1% of Thai LINE users pay ฿99 a month, which is about **฿665M a year**.
- **SAM:** urban Thais aged 18-40 who already use AI. Assume 5M people, 2% of whom pay: about **฿120M a year**.
- **SOM in 3 years:** 5,000-15,000 paying users, about **฿6-18M a year**.
- **Upside:** a business tier (direct booking, paid request inbox) and other LINE markets (Taiwan, Japan).

This is bigger than the tutoring market. But **it depends on a consumer conversion rate that has not been proven.**

---

## 10. Competitors

| Player | Does it execute? | Threat | Our difference |
|---|---|---|---|
| Abdul | Reminds; writes to Calendar | High | Request cards to businesses, bill split, group decisions |
| ChatGPT (Thai works) | Books restaurants through OpenTable, Resy, Yelp; slow browser agent [V, verify #9] | High as a general assistant | Lives in LINE; reaches Thai local businesses; group features |
| Google Gemini / AI Mode | Booking in the US plus 9 markets; **not Thailand**; "Ask for me" calling is US-only [V, verify #8] | Medium now, high later | Thai-first, inside LINE |
| LINE / LY Corp | Japan: Agent i assistant; Toreta booking; AI phone booking (Oct 2026). Thailand: merchant agents (ActEngine AI, Seller Agent); agent developer program [S, verify #7] | Highest long term | Move first; apply to its program; could become a partner |
| LINE MAN Wongnai + JERA Cloud | Booking for its own businesses (restaurants, salons, clinics) | Medium | We reach any business, not only theirs |
| Thai LINE chatbot tools (Botnoi, Chinda, Zaapi, Goragod) | Business side only | Low | We serve the user |

**Moat** [I]:
1. Request-card network: businesses that learn to answer our cards, and the reply-rate data.
2. A Thai task dataset and evaluation set.
3. Group habit: once a friend group uses polls and bill splits, it is sticky.

The moat is weak early on, so speed matters.

---

## 11. Business model

### 11.1 Price to test (a hypothesis)

| Tier | Price | Includes |
|---|---|---|
| Free | ฿0 | 15 actions a month, reminders, 1 group |
| Plus | ฿99 a month (test against ฿149) | Unlimited personal actions, request cards, bill split, 3 groups |
| Group / Team | ฿249-399 a month | More groups; shared tasks for clubs and small teams |
| Business (later) | ฿199-299 a month | Direct booking page, request inbox, reminders to its customers |

Payment by PromptPay to our company account. This is our own revenue, not money held for others.

### 11.2 Unit cost [I, legal §4]
**Per paying user per month:**
- AI: about ฿3-10 on Haiku 5.5 (more actions and vision than booking alone).
- LINE push messages: about ฿2-6.
- Payment fee: about ฿4.
- **Total variable cost about ฿10-20.** At ฿99, the gross margin is about 80-90%.

**Free users** cost about ฿2-4 each a month. With 20 free users per paying user, that is about ฿40-80 per paying user, so **the free tier must stay small.**

**Fixed costs:**
- About ฿4,000 a month now.
- About ฿9,000 a month once the LINE reply plan applies.

**Break-even:** about 120-320 paying users, excluding salaries.

---

## 12. Legal and compliance

| Area | Rule | Design |
|---|---|---|
| Electronic Transactions Act s.15 | A message sent by a system set up to run for the user counts as the user's message [V, verify #13] | Every action leaving the system needs a confirmed tap, stored in the audit log |
| ETA s.17/1 | Input errors can be withdrawn if the user had no chance to correct them [V] | Edit button on every confirmation |
| PDPA (controller; cross-border transfers s.28-29) | [S] | Privacy notice listing vendors and regions; zero-retention LLM routing; deletion; minimal data about third parties named in requests |
| PDPA s.26 health data | [S] | No medical requests |
| Computer Crime Act s.11 (unwanted messages) | [S] | The user sends cards to businesses they chose; each card has a "stop requests" button for the business |
| SMS rules (registered sender; registered links) | [S] | No SMS in the MVP |
| Payment Systems Act | [S] | PromptPay QR codes always point to the payer's or merchant's own account; we never hold money |
| Draft AI Act (ETDA, July 2026, not enacted) | [S, verify #14] | Disclose AI on every card; human-confirm design |
| LINE terms and pricing | [V] | No automation of personal accounts; budget the ฿5,000 reply plan |
| Google OAuth | [I] | App verification for the Calendar scope before going past 100 users |

A lawyer should review the terms of service, our liability for actions, the cross-border legal basis and any later payment features.

---

## 13. Testing approach
- **Thai task set:** 300 real requests (30 per action). Measure intent accuracy, slot accuracy and end-to-end success.
- **Safety:** prompt-injection test set; 500 retry and duplicate tests; a "no tap, no action" test for every tool.
- **Request-card field test:** 50 real cards to real businesses (sent by users, with consent). Measure the reply rate and the time to reply.
- **Pilot:** 200 users for 6 weeks. Track weekly actives, tasks per user, and success rate per action.

---

## 14. Validation and timeline

### 14.1 Validation first (weeks 1-4)

| Test | Pass mark | Stop or pivot if |
|---|---|---|
| 1. Interview 20 users | ≥12 name 3+ of the 10 actions as something they would delegate weekly | <6 |
| 2. Concierge pilot: a LINE OA where the team does tasks by hand for 30 users, for 2 weeks | ≥2 tasks per user per week; ≥50% still use it in week 2 | <1 task per user per week |
| 3. Request-card reply test: 50 real cards | ≥50% answered within 3 hours | <25%: drop Level 2 and focus on groups and personal tasks |
| 4. Price test: ask concierge users to pre-pay ฿99 | ≥10% pre-pay | <3% |
| 5. Tech spike | LIFF and webhook work; check whether the share picker can send to an OA; ≥85% accuracy on the first 100 requests | <70% |

### 14.2 Build plan (7 months) [I]

| Month | Build | Exit test |
|---|---|---|
| 1 | Validation; data model; LINE and LIFF skeleton; collect Thai requests | Section 14.1 pass marks |
| 2 | Router, confirmation gate, audit log; actions 1-2 | 0 actions without a tap |
| 3 | Actions 3-4 (group poll, bill split) | ≥90% accuracy on the test set |
| 4 | Action 7 (request cards and replies), action 8 | ≥50% reply rate in the field |
| 5 | Actions 5-6; PDPA; start the pilot | 100 users |
| 6 | Action 9 (direct booking for businesses that opt in); action 10 | 200 users; ≥30% weekly active |
| 7 | Hardening, price test, final report and demo | ≥10% pay |

---

## 15. KPIs

| Area | Target |
|---|---|
| Intent and slot accuracy (Thai set) | ≥90% |
| Actions without a tap | 0 |
| Level 1 end-to-end success | ≥85% |
| Request cards answered within 3 hours | ≥50% |
| Weekly active users at week 6 | ≥30% |
| Tasks per active user per week | ≥3 |
| Paying users at month 7 | ≥10% of actives or ≥30 |
| LINE pushes per active user per month | ≤40 |

---

## 16. Gate check (the team's own rules)

| Gate | Result | Why |
|---|---|---|
| G1 No bypass | **PASS** | We don't sit between a payment and the business. The value is the assistant itself. A business answering outside our card only loses the automation |
| G2 Clear payer and real saving | **UNPROVEN, weaker than tutoring** | A consumer payer with a time saving, not a money saving |
| G3 Buildable, no dominant giant | **PARTIAL** | Buildable. General giants exist (ChatGPT, Gemini, LINE), but none does things in Thai inside LINE today |
| G4 Startup scale | **PARTIAL** | The consumer TAM is large (about ฿665M) but depends on conversion. Upside from a business tier and other markets |
| G5 Evidence | **PARTIAL** | Desk research only |

---

## 17. Budget (pilot) [I]
- LINE OA Pro: about ฿1,900 a month.
- Hosting: about ฿1,000-1,700 a month.
- LLM: about ฿500 a month (200 pilot users).
- Search API and monitoring: about ฿500 a month.
- **Total about ฿4,000-4,600 a month.** Add ฿5,000 a month once replies pass 25,000 a month (after 1 Feb 2027).

---

## 18. Risks

| Risk | Likelihood / impact | Mitigation |
|---|---|---|
| Users won't pay | High / high | Price test in month 1; small free tier; group and team tier |
| Businesses ignore request cards | Medium-high / high | Reply test first; the card shows the user's name and an AI disclosure; fall back to Level 1 focus |
| Abdul, ChatGPT or LINE copies it | High / high | Speed; request-card network; group habit; consider partnering (Abdul) or LINE's program |
| Scope too broad for 3 people | High / medium | Fixed 10 actions; drop order; shared tool framework |
| Wrong or unwanted action | Medium / medium | Confirmation gate with Edit; audit log; idempotency |
| Prompt injection from business replies or web pages | Medium / medium | Treat them as data; tools run only from user turns |
| LINE price or policy change | Medium / medium | Budget the reply plan; cap pushes |

---

## 19. Changes from v1 (barbers) and audit decisions
1. **Scope:** barbers only becomes a broad assistant with 10 actions. Focus moves from a shop type to a user group.
2. **Reaching businesses:** "write into their system" becomes **request cards** that any business answers with a tap. Direct booking is only for businesses that opt in, chosen from request data.
3. **Payer:** the shop becomes the user; the business tier comes later.
4. **SMS** is added to out of scope (Thai sender and link registration rules) [S].
5. **Reply plan:** the ฿5,000 LINE plan is budgeted as near certain for a chat assistant.
6. Carried over from v1:
   - official LINE reply-plan notice;
   - "26%" relabelled as a global survey;
   - Fresha's AI integration dated August 2026;
   - barber counts marked weak;
   - ETA s.17/1 behind the Edit step.

---

## 20. Comparison: TumHai (assistant) vs ClassLedger (tutoring)

### 20.1 Side by side

| Criterion | ClassLedger (tutoring and class schools) | TumHai (AI assistant in LINE) |
|---|---|---|
| What it is | Invoice, verified slip, class credits, attendance, teacher pay statement, owner dashboard | An assistant that does tasks: calendar, groups, bill split, documents, request cards to any business |
| Customer | School owners (B2B) | Everyday users (B2C); businesses later |
| Who pays and how much | Owner, ฿390-1,490 a month | User, ฿99-149 a month (to test) |
| Pain type | Money and staff time: unpaid fees, fake slips, admin salary ฿14-25k [V/S] | Personal time and convenience [I] |
| Evidence of pain | Desk evidence, fairly strong | Weak; no Thai data on delegating tasks |
| Sides to win | One | One at first (users); businesses only need to answer |
| Market size | About 1,000-1,500 active schools; G4 FAIL | Consumer TAM about ฿665M; G4 PARTIAL |
| Competition | Tellfox, iDotutor, GMS, CodexLearn (small; none do the full loop) | Abdul, ChatGPT, Gemini, LINE (big, but none acts in Thai inside LINE) |
| Platform risk | LINE costs; slip-verification vendor | LINE costs and policy; LINE could compete |
| Legal load | Medium: minors' data, slips, teacher tax exports | Medium-low: ETA attribution, PDPA; no money handling |
| AI depth (grading) | Low: optional AI report draft | **High:** agent, tools, safety gate, Thai evaluation |
| Demo appeal | Solid admin software | Strong: "type in LINE, it gets done" |
| Access to users | Unknown: must reach school owners | **Easy:** university students and staff |
| Build risk in 7 months | Medium: money ledger must be exact | Medium-high: 10 actions, agent reliability |
| Time to first revenue | Possibly fast once one school says yes | Slower: needs many users at a low price |
| Startup ceiling | Small business; adjacent verticals | Larger, if users convert |
| Gates | G1 PASS · G2 UNPROVEN · G3 PASS with caution · G4 FAIL · G5 PARTIAL | G1 PASS · G2 UNPROVEN (weaker) · G3 PARTIAL · G4 PARTIAL · G5 PARTIAL |

### 20.2 Weighted score [I]

Weights reflect the team's two stated goals: a strong senior project and an idea that could become a real startup. Scores are 1-5. The team should change the weights if its priorities differ.

| Criterion | Weight | Tutoring | Assistant |
|---|---|---|---|
| Clear payer and pain evidence | 20% | 4 | 2 |
| Market ceiling (startup potential) | 15% | 2 | 4 |
| Competition and defensibility | 10% | 3 | 2 |
| AI and technical depth (grading) | 15% | 2 | 5 |
| Ease of reaching users for validation | 10% | 2 | 5 |
| Build risk in 7 months (higher = safer) | 10% | 3 | 3 |
| Legal and platform risk (higher = safer) | 10% | 3 | 3 |
| Team motivation (team chose the new idea) | 10% | 3 | 4 |
| **Weighted total** | 100% | **2.80** | **3.45** |

How sensitive this is:
- If the weights become clear payer 35%, AI depth 5%, ease of reaching users 5% and the rest unchanged, tutoring wins: 3.10 vs 3.00.
- The result turns on one question: **does the team value a proven payer more than AI depth and upside?**

### 20.3 How to decide today
- **Choose TumHai (assistant)** if the team's priorities are AI depth for grading, easy access to test users, a bigger ceiling, and motivation. Accept that payment is unproven, and commit to the kill switch below.
- **Choose ClassLedger (tutoring)** if the priority is a clear paying customer and lower market risk, and the team can reach 10-15 school owners this month.

**Recommendation:** the scores lean toward **TumHai**, as long as the team accepts a **kill switch at week 4**. If tests 2-4 fail (fewer than 1 task per user per week, a reply rate under 25%, or fewer than 3% pre-paying), switch to ClassLedger, whose proposal is ready. Only about 4 weeks would be lost, and the LINE, LIFF and audit-log work carries over, since both products use the same stack.

---

## 21. Questions for the team (to settle today)
1. Which idea? If TumHai, do we accept the week-4 kill switch?
2. Do we agree the 10 actions, or swap some?
3. Can each member bring 10 users for the concierge pilot, and send about 15 real request cards?
4. Should we approach Abdul as a possible partner, and apply to LINE Thailand's agent program?
5. Is the final name to be decided later? TumHai is a placeholder.

---

## 22. Key references
- **Abdul:** https://abdul.chat/ ; https://abdul.chat/en/privacy-policy ; https://abdul.chat/en/terms-of-service ; https://page.line.me/319gqomd
- **LINE:**
  - Messaging API: https://developers.line.biz/en/docs/messaging-api/sending-messages ; group chats https://developers.line.biz/en/docs/messaging-api/group-chats/ ; pricing https://developers.line.biz/en/docs/messaging-api/pricing/
  - Share target picker: https://developers.line.biz/en/docs/line-mini-app/develop/share-messages
  - Thailand reply-message notice: https://www.lineforbusiness.com/th/news/20261030
- **Thai SMS rules:**
  - https://www.bangkokpost.com/thailand/general/2889473/all-senders-of-sms-with-links-must-register
  - https://support.twilio.com/hc/en-us/articles/5142014273307-Documents-Required-and-Instructions-to-Register-Your-Alphanumeric-Sender-ID-in-Thailand
- **Claude pricing:** https://platform.claude.com/docs/en/about-claude/pricing
- **Law:**
  - ETA (MDES): https://www.mdes.go.th/law/detail/3616-ELECTRONIC-TRANSACTIONS-ACT--B-E--2544--2001-
  - Draft AI Act: https://www.tilleke.com/insights/thailand-releases-new-draft-artificial-intelligence-act
  - Cross-border transfers: https://www.tilleke.com/insights/thailands-regulations-for-cross-border-personal-data-transfer-come-into-force
- **Tutoring proposal:** `docs/research/2026-11-04-project-proposal.md`
- Full source lists are in each research file.
