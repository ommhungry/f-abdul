# How an AI assistant in Thailand can actually complete bookings

- Date: 2026-11-08. All web sources accessed 2026-11-08.
- Question: Abdul only reminds (see `2026-11-08-abdul-teardown.md`). Our idea is an assistant in LINE that **executes**, e.g. "book me a haircut Saturday afternoon at my usual barber". How can it really book, and what can 3 students build in about 7 months?
- Method: desk research only (web search and page reads). No sign-ups, no calls, no bookings, no contact with businesses, no probing of Abdul.
- Tags: [V] primary or official source; [S] secondary source; [I] our inference; UNVERIFIED where unsure.

## 1. Short answer

1. **No open "book anything in Thailand" API exists.** The big booking platforms either have no public API, gate it behind partner deals, or let an AI search but not book [V/S, section 2a].
2. **We cannot message a barber's LINE OA from our bot.** The LINE Messaging API only talks to users who added our OA [I, from V docs].
3. **AI phone calls in Thai are technically possible but risky** for a student MVP: cost about 15-25 THB a call, Thai voice quality is unproven, and the law on AI calls is unsettled [S/I].
4. **Browser agents are weakest exactly at form-filling** and break on CAPTCHAs, logins and site changes [S].
5. **The realistic path is to own the merchant side**: a free LINE booking page for small barbers and salons, so our agent books into a calendar we control. Then add a "human-in-the-loop" fallback (we draft the LINE message, the user taps send) for shops not on our system [I].

## 2. Execution channels

### 2a. Booking platforms with APIs

| Platform | Used in Thailand? | Can a third party book through it? | Notes |
|---|---|---|---|
| Fresha | Yes, small Bangkok footprint (e.g. "34 venues" on its Bangkok salon page) [V, fresha.com/lp/en/th-bangkok] | **No booking API.** Public MCP server for ChatGPT and Claude (Sept 2026) gives real-time discovery, but the user is sent to Fresha checkout to confirm [S, cxnetwork.com]. No public merchant API, only a read-only BI connector [S, supergood.ai] | Best "AI-aware" platform, but only discovery |
| Booksy | Little Thai presence [I] | Partner API exists; docs return 401 to the public; access needs a deal with Booksy [S, supergood.ai; apis.io] | Not practical for us |
| SimplyBook.me | Has a Thai/Asia presence and a LINE bot add-on [S, simplybook.net] | **Yes, technically.** JSON-RPC API with a `book` method, but needs each merchant's API key [V, simplybook.me/api/doc]. Docs are patchy [S, tech-support.simplybook.me] | Works only for merchants who already use it and give us a key |
| Square Appointments | Square does not operate in Thailand [I, UNVERIFIED] | API can create bookings, but only for sellers on paid Appointments plans [V, developer.squareup.com] | Irrelevant for Thailand |
| Setmore | Unknown Thai use | Not researched in depth. UNVERIFIED | Low priority |
| Zenwel (Olsera) | Thai salon and spa tool, free tier [S, olsera.com/en/zenwel] | No public API found. UNVERIFIED | Possible partner, not an API |
| GoWabi | Thai beauty and spa marketplace; 1,000-10,000 merchants depending on source and year [S, techsauce.co; omise.co] | No public developer API [S, apis.io] | Partnership only |
| QueQ | Thai queue app for restaurants, hospitals, banks; reported 16M users [S, dealroom/cbinsights] | No public API found [S] | Partnership only |
| Hungry Hub | Thai restaurant packages, 2,000+ partners [S, apievangelist] | No public API [S]. A possible OpenRice webhook link is unconfirmed. UNVERIFIED | Partnership only |
| Eatigo | Thai restaurant deals; Wongnai tie-up dated 2019 [S, nationthailand.com] | No public API found. UNVERIFIED | Possibly stale |
| LINE MAN Wongnai | 55,000 restaurants on Wongnai/FoodStory POS (2023) [S, lmwn.com] | "Wongnai Reservations" (Feb 2026) puts a Reserve button in Google Search and Maps for Wongnai POS restaurants; 500-2,390 THB a month [S, brandinside.asia]. No third-party API mentioned | Restaurants only; Google is the front door |
| Reserve with Google / Actions Center | Thailand listed in older partner notices [S, tablecheck.com]; current status unconfirmed. UNVERIFIED | **Inbound only.** Scheduling providers integrate so users book on Google Search and Maps. A 12-16 week integration [V, developers.google.com/actions-center]. It does not let another assistant book | Useful later: our merchant tool could become a Google booking partner |

Coverage takeaway [I]: most Thai barbers, small salons and clinics book by LINE chat, phone or a paper book, not through any SaaS. Thai clinic-software vendors say LINE OA auto-reply "doesn't know which staff is free", so real booking still waits for an admin [S, hdmall.co.th]. So even a perfect API set covers only a small share of the "usual barber" case.

Legal/ToS: scraping or reverse-engineering consumer apps (e.g. paid "unofficial API" layers for Booksy) likely breaks their terms [S, supergood.ai]. Avoid.

### 2b. AI voice calls to the business in Thai

| Item | Finding |
|---|---|
| Google "Ask for me" | Calls businesses to ask price and availability; US only; says it is an AI; businesses can opt out. It asks, it does not book [S, techcrunch.com 2025-01-30] |
| Platforms | Retell, Vapi, Bland. Retell lists $0.055/min infra + LLM ($0.003-0.16/min) + TTS ($0.015-0.10/min) + telephony $0.015/min; Thailand appears as a Twilio country [V, retellai.com/pricing]. Retell has a Thai language page [V, retellai.com/da/languages-ai/thai-ai]. Vapi headline $0.05/min, all-in $0.10-0.30/min [S, 11x.ai; ainora.lt]. Bland $0.11-0.14/min [S, layer3labs.io] |
| Thai vendors | Botnoi Voicebot (inbound and outbound, SIP, enterprise pricing on request) [V, botnoigroup.com]; True AI VoiceBot [V, business.true.th]. Both sell to companies running their own call centres, not to apps calling small shops [I] |
| Thai speech quality | Thai speech-to-text is now decent: Typhoon ASR reports 6.8-10% character error rate; Gemini 3 Pro about 11-12.5% [S, arxiv.org/abs/2601.13044; emergentmind]. Errors on names, times and Thai numbers matter for bookings [I]. Thai TTS quality is vendor-claimed only (ElevenLabs) [S] |
| Cost per booking call | A 3-minute call at $0.15-0.25/min is about $0.45-0.75, roughly 15-25 THB, plus retries [I] |
| Law | Voice recordings are personal data under the PDPA; tele-sales advice is to disclose before recording [S, tilleke.com]. NBTC AI guidelines for telecom (2 July 2026) are non-binding and stress transparency [S, hlc.com; tilleke.com]. We found no Thai rule that explicitly requires an AI to say it is an AI. UNVERIFIED. Reports of a Thai AI law in force from 1 March 2026 are UNVERIFIED |
| Practical risks [I] | Thai shops are on high alert for scam calls; an unknown number with a synthetic voice may be hung up on. Calls to busy barbers mid-cut fail. Each failure costs money and user trust |

Verdict [I]: good for a later "concierge" feature or a demo, not the MVP core. If used, always disclose "I am an AI assistant calling for Khun X", record only with notice, and keep a human fallback.

### 2c. Messaging the business's own LINE OA

- A bot can send messages only to users, groups or rooms tied to its own channel; push and reply both target IDs that come from users who interacted with our OA [V, developers.line.biz/en/docs/messaging-api/sending-messages].
- Only **one** LINE Official Account can be in a group chat at a time [V, developers.line.biz/en/docs/messaging-api/group-chats]. So we cannot put our bot and the barber's OA in one group.
- There is no documented bot-to-OA path, and user IDs are scoped to a provider, so we cannot address a barber's OA [I, UNVERIFIED as an explicit rule].
- Automating a user's personal LINE account to chat with the shop would breach LINE's terms and risk account bans [I, UNVERIFIED wording].
- **What works:** a "drafted handoff". The agent writes the Thai message ("สวัสดีครับ ขอจองตัดผมวันเสาร์ บ่าย 2 โมง ชื่อ...") and gives a button that opens the shop's OA chat via its LINE URL with the text ready to send. The user taps Send; the agent then sets a reminder to check the reply [I]. This is not full execution, but it removes most of the effort and is fully allowed.
- Cost note: push messages count against LINE plan quotas; replies are free [V, developers.line.biz/en/docs/messaging-api/pricing]. A secondary report says LINE Thailand will charge Reply API messages above 25,000 a month from 1 Feb 2027 [S, relevantaudience.com]. This affects our unit economics at scale.

### 2d. Browser or computer-use agents

- Web Bench (5,750 tasks, 452 sites) found all agents did "surprisingly poorly" on write tasks such as logging in and filling forms [S, skyvern.com].
- OSWorld scores now reach 70-80%+ on desktop tasks, but these are self-reported and not booking-specific [S, codesota.com].
- A 2026 comparison suggests agents using structured site tools (e.g. WebMCP) are cheaper and as reliable as screenshot agents [S, windtunnel results, UNVERIFIED methodology].
- Barriers [I]: CAPTCHAs (by design they block bots; solving them breaks most site terms), OTP and login walls, Thai-only date pickers, slow pages, and frequent layout changes. Each run costs more and takes 1-3 minutes.
- Verdict [I]: use only as a fallback for a few known, simple booking pages, with the user approving the final step. Not a core channel.

### 2e. Building our own merchant side

- What it is: a free LINE booking tool for barbers, salons and small clinics. The shop gets a booking page (LIFF), a staff calendar, auto-confirm, and reminders to customers to cut no-shows. Our consumer agent books straight into it.
- Why it fits Thailand: shops already live in LINE; LINE OA auto-reply cannot see staff availability [S, hdmall.co.th]; competing tools exist but are paid (e.g. Dolyu US$15/month) or generic (SimplyBook LINE bot) [S, launch.unwire.hk; simplybook.net]. Wongnai shows the model works for restaurants and charges 500-2,390 THB a month [S, brandinside.asia].
- The hard part is the two-sided cold start. Ways to soften it [I]:
  1. **Single-player value first.** The shop gets a free booking page and no-show reminders even if no customer uses our assistant.
  2. **Customer-pulled onboarding.** When a user asks to book an unlisted barber, we send the drafted LINE message plus "Your shop can take bookings like this for free: [link]". Each request recruits a merchant.
  3. **Dense pilot.** Sign 20-40 barbers and salons around one campus or district, not all of Bangkok.
  4. Later, become a Google Actions Center scheduling provider so our merchants also get "Book" on Google Maps [V, developers.google.com/actions-center]. That is a strong selling point for shops.
- Cost: our own hosting only; no per-booking fee to anyone.

### Channel comparison

| Channel | Feasible in 7 months | Coverage of Thai small shops | Cost per booking | Reliability | Legal/ToS risk |
|---|---|---|---|---|---|
| Third-party booking APIs | Low (partner deals) | Very low | ~0 | High where it exists | Low with a deal; high if scraped |
| AI voice call | Medium (demo) | High in theory | 15-25 THB+ | Low-medium | Medium (PDPA, unclear AI-call rules) |
| Message shop's LINE OA via bot | Not possible | n/a | n/a | n/a | Blocked by design |
| Drafted LINE handoff (user taps send) | High | High | ~0 | High (human sends) | Low |
| Browser agent | Medium | Low | ~3-10 THB [I] | Low | Medium-high (CAPTCHA, ToS) |
| Own merchant tool | High | Grows with sign-ups | ~0 | High | Low |

## 3. Recommended architecture for the "execute" agent

All [I] unless tagged.

1. **Channel layer.** LINE Messaging API webhook, LIFF mini-app for settings, booking page and confirmations (same base as Abdul [V, teardown]).
2. **Intent and slot filling.** An LLM with tool calling turns Thai text into a structured request: service, shop, staff, date window, name, phone. "My usual barber" resolves from memory of past bookings.
3. **Tools** (narrow and typed): `search_availability`, `hold_slot`, `confirm_booking`, `cancel_booking`, `draft_line_message`, `create_calendar_event`, `set_reminder`. Use strict tool schemas so arguments always validate [V, Claude API docs via skill].
4. **Confirmation gate.** No external action without a LINE Flex card ("Sat 15 Nov, 14:00, Somchai Barber, haircut 200 THB. Confirm?") and an explicit tap. The tap is checked in code, not by the LLM.
5. **Idempotency.** Each booking intent gets a unique key; `confirm_booking` with the same key twice returns the first result. Holds expire after a few minutes.
6. **State machine.** draft → held → confirmed → reminded → done / cancelled / no-show. The LLM proposes moves; code enforces allowed transitions.
7. **Audit log.** Append-only record of who asked, what the agent proposed, what the user approved, and what each tool returned. Needed for disputes and PDPA access requests.
8. **Failure handling.** Slot taken → offer the next 3 slots. Tool error → retry with backoff, then tell the user plainly. Unknown shop → drafted LINE handoff plus merchant invite. Never claim success without a confirmed tool result.
9. **Calendar sync and reminders.** Write the confirmed booking to Google Calendar and set a reminder, matching Abdul's baseline (natural-language Thai times, snooze, group reminders) [V, teardown].
10. **Safety.** Treat merchant replies and web content as data, not instructions (prompt-injection defence). Store minimal personal data; PDPA notice at onboarding.

### Models and cost per booking

| Model | Price per 1M tokens (input / output) | Use |
|---|---|---|
| Claude Haiku 5.5 | $0.10 / $0.50 (prompts up to 100K) | Default: intent, slot filling, tool calls, Thai replies |
| Claude Sonnet 5.5 | $2 / $10 | Escalation for messy or multi-step requests |

Prices [V, Anthropic model table cached 2026-10-06, platform.claude.com/docs/en/about-claude/pricing].

Rough cost per booking [I]: about 6 model calls, 30K input tokens (mostly cached system prompt and tools) and 3K output tokens.
- Haiku 5.5: about $0.005, roughly 0.2 THB.
- Sonnet 5.5: about $0.09 before caching, roughly 3 THB.
- Add LINE push messages for reminders (quota-based). An AI voice call would add 15-25 THB, which is 50-100 times the LLM cost.
- Thai quality must be tested on a set of 100-200 real Thai booking requests before choosing Haiku as default. DeepSeek and Gemini (Abdul's choices) or Typhoon (Thai open model) are alternatives to compare [I].

## 4. Realistic MVP slice (about 7 months)

**Recommendation:** start with **our own merchant booking tool + the consumer agent in LINE**, with the **drafted LINE handoff** as the fallback for shops not on the system. Skip voice calls and browser agents in the MVP.

Scope:
1. Consumer side: Thai natural-language booking for one vertical (barbers and hair salons), confirmation card, calendar write, reminders, reschedule and cancel.
2. Merchant side: free LINE/LIFF booking page, staff and service setup in under 10 minutes, daily schedule pushed to the owner's LINE, no-show reminders to customers.
3. Fallback: drafted LINE message for any other shop, plus merchant invite link.
4. No deposits or payments in version 1 (as the team planned).

Reasoning [I]:
- It is the only channel where we control **both** the data and the action, so bookings are reliable and testable.
- It needs no partner deal, no paid phone minutes, and carries little legal risk.
- It gives merchants value on day one, which solves part of the cold start.
- It differentiates from Abdul: Abdul could add calendar booking, but it has no merchant network. A network of shops is a moat; reminders are not [V/I, teardown section 6].
- It is demo-able for the senior project: one district, 20-40 shops, real bookings, measurable success rate.

Suggested timeline [I]:

| Month | Milestone |
|---|---|
| 1 | Interview 15 barbers and salons; collect 200 Thai booking phrases; finish this tech spike |
| 2-3 | Merchant booking page and calendar; consumer agent with confirmation gate |
| 4 | Pilot with 10 shops near campus; drafted-handoff fallback |
| 5 | Reschedule and cancel; no-show reminders; audit log; PDPA notices |
| 6 | Grow to 30-40 shops; measure booking completion rate and repeat use |
| 7 | Optional stretch: voice-call prototype for availability questions only, with clear AI disclosure; final report |

Key open questions to verify next:
- Is Reserve with Google / Actions Center appointments live in Thailand today?
- Exact LINE OA Thai pricing and the reported 2027 Reply API change.
- Will barbers accept a new tool? (field interviews)
- Thai law on AI-disclosed outbound calls (ask a Thai lawyer or check PDPC and NBTC texts).

## Sources

All accessed 2026-11-08.

- Abdul teardown (internal): docs/research/2026-11-08-abdul-teardown.md
- Fresha MCP and Claude/ChatGPT launch: https://www.cxnetwork.com/cx-experience/articles/fresha-goes-conversational-with-native-claude-and-chatgpt-integration-for-beauty-bookings/amp
- Fresha API report card: https://supergood.ai/api-report-card/fresha
- Fresha Bangkok listings: https://www.fresha.com/lp/en/th-bangkok
- Fresha partner terms: https://terms.fresha.com/partner-terms
- Booksy API notes: https://supergood.ai/api-report-card/booksy ; https://apis.io/apis/booksy/booksy-public-api/
- SimplyBook.me API: https://simplybook.me/api/doc ; https://help.simplybook.me/index.php/Company_administration_service_methods ; https://tech-support.simplybook.me/t/302-insufficient-documentation-for-admin-api-book/306
- SimplyBook LINE bot: https://simplybook.net
- Square Bookings API: https://developer.squareup.com/reference/square/bookings-api/create-booking
- Zenwel: https://www.olsera.com/en/zenwel
- GoWabi: https://www.omise.co/en/voice-of-customers/featured-customer-gowabi ; https://techsauce.co/en/pr-news/gowabi-raises-another-round-to-expand-its-spa-and-beauty-booking-marketplace-in-sea ; https://apis.io/providers/gowabi/
- QueQ: https://app.dealroom.co/companies/queq ; https://www.nationthailand.com/tech/30357391
- Hungry Hub: https://providers.apievangelist.com/providers/hungryhub/
- Wongnai and Eatigo (2019): https://www.nationthailand.com/business/30377413
- Wongnai Reservations (Feb 2026): https://brandinside.asia/?p=254973
- LINE MAN Wongnai restaurants: https://lmwn.com/line-man-wongnai-restaurants-2023-en/
- Google Actions Center appointments: https://developers.google.com/actions-center/verticals/appointments/e2e/policies/integration-policies ; https://developers.google.com/actions-center/verticals/local-services/e2e/integration-steps/overview
- Reserve with Google in Thailand (older): https://tablecheck.com/en/company/press/reserve-with-google-press-release ; https://support.google.com/reserve/answer/9172607
- Google "Ask for me": https://techcrunch.com/2025/01/30/googles-ask-for-me-feature-calls-businesses-on-your-behalf-to-inquire-about-services-pricing/
- Retell pricing and Thai page: https://www.retellai.com/pricing ; https://www.retellai.com/da/languages-ai/thai-ai
- Vapi and Bland pricing: https://www.11x.ai/guides/vapi-pricing ; https://ainora.lt/blog/ai-voice-agent-cost-per-minute-2026 ; https://www.layer3labs.io/guides/bland-ai-pricing
- Botnoi Voicebot: https://botnoigroup.com/ai/voicebot
- True AI VoiceBot: https://business.true.th/en/solutions/true-ai-chatbot-and-voicebot
- Typhoon ASR paper: https://arxiv.org/abs/2601.13044 ; https://www.emergentmind.com/topics/typhoon-asr-benchmark
- ElevenLabs Thai TTS: https://elevenlabs.io/text-to-speech/thai
- PDPA and AI guidance: https://www.tilleke.com/insights/thailand-seeks-feedback-on-draft-data-protection-guidelines-for-ai ; https://www.tilleke.com/insights/thailand-insurance-industry-ai-and-privacy-regulatory-updates/65/
- NBTC AI guidelines for telecom: https://hlc.com/en/publications/thailand-issues-ai-guidelines-for-telecommunications-services ; https://www.tilleke.com/insights/thailand-issues-ai-guidelines-for-telecom-services/3/
- LINE Messaging API: https://developers.line.biz/en/docs/messaging-api/sending-messages ; https://developers.line.biz/en/docs/messaging-api/group-chats/ ; https://developers.line.biz/en/docs/messaging-api/pricing/
- LINE Thailand Reply API change (2027): https://www.relevantaudience.com/social-media-marketing/line-reply-api-25000-free-messages-agency-global-platform/
- Thai clinic LINE booking limits: https://hdmall.co.th/jib-os/resources/78-line-oa-auto-reply-vs-clinic-system/
- Dolyu: https://launch.unwire.hk/en/dolyu
- Web Bench: https://www.skyvern.com/blog/web-bench-a-new-way-to-compare-ai-browser-agents
- OSWorld leaderboard: https://www.codesota.com/benchmark/osworld
- WindTunnel results: https://nekuda-windtunnel-results-explorer.static.hf.space/index.html
- Claude pricing: https://platform.claude.com/docs/en/about-claude/pricing
