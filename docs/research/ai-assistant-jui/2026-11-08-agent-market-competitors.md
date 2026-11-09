# Booking agent in LINE: competitors, demand, supply, business model, market size

- Date: 2026-11-08. All sources accessed 2026-11-08.
- Question: Is there room for a Thai assistant in LINE that books appointments for the user (barber, salon, clinic, restaurant), not just reminds?
- Method: desk research with web search only. No sign-ups, no contact with anyone, no probing of Abdul. Builds on `2026-11-08-abdul-teardown.md`.
- Tags: [V] primary or official source; [S] secondary source; [I] our inference; UNVERIFIED where we are unsure. Many numbers come from vendor marketing, and we flag those.

## 1. Bottom line

1. Nobody in Thailand offers a consumer assistant that books local services for you today. The big agents that book (ChatGPT, Google AI Mode) work mainly through US and European booking partners and do not cover Thai barbers or salons [S][I].
2. The biggest threat is LINE itself. In Japan, LY Corp has an AI agent (Agent i) and a phone agent that takes restaurant bookings. In Thailand, LINE sells AI agents to merchants [V][S].
3. The merchant is the clearest payer. Booking-software benchmarks in Thailand are ฿499 to ฿1,875 a month [S]. A consumer subscription for booking alone looks weak, because people book a barber about once a month [I].
4. Recommended beachhead: barbers and hair salons in Bangkok. Clinics come second, because health data and an existing clinic-software player raise the bar. Restaurants come last, because Thai diners walk in and global players already cover them [I].
5. The key technical fact: a LINE bot cannot message another shop's LINE OA [I, from how the LINE Messaging API works]. So "the agent books for you" in practice needs the shop on our system, or a phone call, or a human. Plan the supply side first.

## 2. Competitors and substitutes

| Player | What it does today | Books or only reminds? | Price | In Thailand / Thai? |
|---|---|---|---|---|
| Abdul (abdul.chat) | Reminders, notes, group summaries, Google Calendar write | Reminds only | ฿0 / 83 / 125 / 409 a month [V, teardown] | Yes, Thai, in LINE, 41.5k friends [V] |
| LINE Thailand (consumer) | No consumer AI assistant found. LINE Premium sells sticker and backup perks | Neither | LINE Premium ฿169 a month, 3M+ subscribers [S, relevantaudience; S, websearch on LINE Premium] | Yes |
| LINE Thailand (merchant) | ActEngine AI (support and sales agents, used by LINE MAN Wongnai); Seller Agent for OA and MyShop | Merchant answers chats; booking not stated | Not public | Yes [V, linecorp.com 2026-03-12; S, sanook.com] |
| LY Corp Japan: Agent i | Combined "LINE AI" and Yahoo AI Assistant in April 2026; "Agent i in chat" planned in 2026 | Assistant; booking unclear | Free and paid tiers earlier [S, KED Global] | Japan only [V, lycorp.co.jp 020398, 020594] |
| LY Corp Japan: LINE AI Reservation | AI phone agent that takes restaurant bookings, linked to Toreta | Books (merchant side) | Not found | Japan only [S, travelvoice.jp] |
| ChatGPT | Restaurant reservation search in chat: OpenTable globally, Resy US, Yelp US/Canada. Agent mode can click through websites, slowly | Partly books; hands off for card steps | Plus ฿699, Go ฿259 a month [S, glbgpt.com; S, iapp.co.th] | Works in Thai. Few Thai merchants on OpenTable [I] |
| Google AI Mode | Agentic restaurant booking with OpenTable, Resy, Tock, Booksy and others; "Ask for me" phones businesses | Finds slots, then hands off; calls in the US | Labs / AI Ultra at first | US, then UK and a few others. No Thai evidence [S, androidcentral, ppc.land, engadget] |
| Apple Intelligence / Siri | No Thai in the supported language list | No | Free with device | No Thai [V, apple.com newsroom 2025-11] |
| Alexa+ | 12 countries listed, plus Japan early access | Some bookings in US | Prime | Not Thailand [V, aboutamazon.com] |
| Botnoi, Chinda, Crescendo Lab, KaoJai, Zaapi, SleekFlow | Chatbots for businesses on LINE OA | Merchant side; some take bookings | Chinda from ฿499; KaoJai ฿590; Zaapi ฿1,875; SleekFlow ~฿3,300 a month [S, iapp.co.th; S, hdmall.co.th] | Yes |
| LINE MAN Wongnai | Food delivery, reviews, POS. "Wongnai Reservations" button in Google Search and Maps | Books restaurants (partly) | Not found | Yes; about 700k merchant partners [S, salestechstar via ActEngine news] |
| Hungry Hub | Prepaid dining offers; commission from diners | Books restaurants | Commission rate not public | Yes, ~2,200 restaurants [S, thestorythailand] |
| QueQ | Remote queue for restaurants, hospitals, banks | Queues, not appointments | Free to users | Yes; 16M users claimed [S, CB Insights, UNVERIFIED] |
| GoWabi | Beauty and wellness marketplace | Books | Commission; affiliate 2-20% [V, gowabi.com FAQ] | Yes; 500k MAU, 5,000 merchants claimed [S, e27] |
| Fresha | Free scheduler; marketplace fee 20% (min US$6) on new clients only | Books | Free core [V, fresha.com/pricing] | Yes, Bangkok listings in baht [V, fresha.com] |
| HDmall Jib OS | Clinic chat inbox and booking across LINE, Facebook, web | Books (clinic side) | Not found | Yes, claims 2,000+ clinics [S, hdmall.co.th, vendor] |
| Booksy | Barber booking; Google AI Mode partner | Books | Not checked | No Thai evidence found [I] |

### Who could copy this fastest

| Rank | Player | Why |
|---|---|---|
| 1 | LINE / LY Corp | Owns the channel, the 56M users and 6M+ OAs. Already runs merchant AI agents in Thailand and a booking phone agent in Japan [V][S]. Can change API pricing (Reply API charges from Feb 2027) [S, relevantaudience]. |
| 2 | LINE MAN Wongnai | 700k merchants, reviews, POS, and ActEngine AI already deployed [S]. |
| 3 | Abdul | Same channel, same users, already writes to Google Calendar [V, teardown]. Adding "book via shop's page" is a short step [I]. |
| 4 | Google | Booking partners already include Booksy (barbers). Rolling out country by country [S]. |
| 5 | Fresha, GoWabi | Already hold Thai beauty supply; could add a LINE front end [I]. |

Our defensible edge, if any, is supply: shops onboarded to a booking page that our LINE agent can write to [I]. A pure chatbot layer is easy to copy.

## 3. Demand side (consumers)

**LINE reach.** LINE says it has 56 million users in Thailand, about 78% of the population, and 12 billion OA conversations in 2025 [S, relevantaudience citing Marketing Oops]. There were about 800 million new OA friend adds in the first half of 2026 [S, same]. Health and beauty brands have opened 380,000 OAs [V, lineforbusiness.com beauty-clinic]. Thais are used to talking to businesses inside LINE.

**How Thais book today.**
- Barbers and salons: walk-in, phone, LINE chat with the shop, Facebook Messenger, and some apps (Fresha, GoWabi) [I, plus listings on fresha.com]. We found no Thai survey that splits these channels. UNVERIFIED.
- Salons in the US: 36% book online and 27% by phone [S, Zenoti 2024, US data]. 69% say they skipped a booking because it was hard to reach the shop [S, Zenoti 2025, US data]. Use only as a direction, not a Thai number.
- Clinics: LINE OA plus admins replying by hand; one hospital lets patients book "without the need for a phone call" [S, lineforbusiness.com].
- Restaurants: mostly walk-in. Hungry Hub started as a reservation tool and struggled "because walk-in dining was easy in Thailand", so it pivoted to prepaid deals [S, e27].

**Evidence that Thais want an agent that books.** It is thin.
- Thai ChatGPT weekly users quadrupled in a year, led by ages 18-24 [S, khaosodenglish].
- 17% of Thai shoppers use a brand's own AI assistant, above the SEA average of 10% [S, Krungsri research].
- Against it: only 26% of APAC consumers want agentic AI for transactions, and 24% refuse to delegate purchases [S, fintechnews.sg; S, marketing-interactive].
- We found no Thai survey on "let AI book my appointment". This is the biggest gap. We must test it with interviews and a fake-door page [I].

**Willingness to pay for AI.**
- Abdul charges ฿83-409 a month [V]. Its paid user count is unknown.
- ChatGPT Go about ฿259 and Plus about ฿699 a month in Thailand [S].
- Only about 5% of ChatGPT's weekly users pay, worldwide [S, the-decoder].
- LINE Premium has 3M+ Thai subscribers at ฿169 a month [S]. This shows that Thais pay small monthly fees inside LINE.

## 4. Supply side (merchants)

| Vertical | Count | Source quality |
|---|---|---|
| Barber shops | 13,483 (Bangkok 2,501) | [S, poidata.io, June 2026, scraped directory] |
| Beauty salons | 31,264 (Bangkok 8,322); 78% list a phone number | [S, poidata.io, Aug 2025] |
| Registered beauty companies (DBD) | 6,621 | [S, nationthailand; companies only, most shops are not companies] |
| Private clinics (all types) | 40,552 (Bangkok 8,273), 2024 | [S, nationthailand citing Dept. of Health Service Support] |
| Restaurants and food outlets | ~680,000 (incl. street food); 50-100k close each year | [S, thestar, thethaiger citing Wongnai] |

Directory counts likely miss small shops with no Google listing [I]. No official count of barbers and salons was found. UNVERIFIED.

**How merchants take bookings.** Mostly LINE OA chat answered by the owner or an admin, plus phone and walk-ins [I]. LINE OA's free auto-reply is limited, and bot schedules often drift from the real schedule [S, hdmall.co.th]. Over 90% of new OA chats were not followed up after the first reply, according to LINE Thailand [S, mgronline].

**Pain.**
- No-shows: clinic rates of 15-30% are commonly cited [S, vendor blogs, not Thai]. A Thai salon-software vendor claims reminders cut no-shows from 25% to 15% [S, vendor, UNVERIFIED]. Reminders cut no-shows by about 41% in a systematic review [S, cited by hdmall].
- GoWabi had frequent no-shows and added prepayment to fix them [S, omise.co case study].
- Chat interruptions: a barber cutting hair cannot answer LINE. This is our inference; we need interviews to confirm it [I].

**What they pay.**
- LINE OA: free tier, Basic ฿1,280, Pro ฿1,780 a month before VAT [S, website.z.com, lineforbusiness]. From Feb 2027, 25,000 free Reply API messages a month, then paid [S].
- Booking and chat tools: ฿499 to ฿3,300 a month (see table in section 2) [S].
- Fresha core scheduler: free; it earns from payments and new-client fees [V].

## 5. Business model options

| Model | Benchmark | Pros | Cons |
|---|---|---|---|
| Consumer subscription | Abdul ฿83-125; ChatGPT Go ~฿259; LINE Premium ฿169 | Matches Abdul; simple | People book services rarely; 5% global pay rate for ChatGPT [S]; booking alone is a weak reason to pay [I] |
| Merchant SaaS (free booking page, paid extras) | Fresha free core; Thai tools ฿499-590 entry | Merchant has clear pain (no-shows, chat load); repeat revenue | Thai SMEs price sensitive; many shops are tiny [I] |
| Per-booking fee | Fresha 20% on new clients (min US$6) [V] | Pay only for results | Shops hate fees on regulars; only fair on new clients [I] |
| Affiliate commission | GoWabi affiliate 2-20% [V]; Hungry Hub diner commission [S] | Fast to start using existing supply | Small payouts; depends on partners' terms; affiliate links are not "execution" [I] |
| Deposits and payments (later) | GoWabi prepay to stop no-shows [S] | Solves no-shows; payment margin | Out of scope at first (no deposit) |

**Clearest payer: the merchant** [I]. The merchant has a measurable loss (no-shows, missed chats). It already pays ฿500-1,800 a month for LINE OA and chat tools. Suggested model: free booking page and LINE booking for shops; paid tier around ฿299-499 a month for reminders, no-show tracking and an AI admin that answers LINE chats; a fee only on new customers that our consumer assistant brings. The consumer side stays free, so it can grow, and later a paid tier can be bundled with Abdul-style reminders [I].

## 6. Market size (Thailand)

All figures are [I], built from the counts above. ฿ figures are yearly.

**Assumptions**
- A1. Bookable merchants: barbers 13.5k + salons 31.3k + clinics 40.6k + restaurants that take reservations, taken as 10% of 680k = 68k. Total ≈ 153k.
- A2. Merchant price ฿500 a month = ฿6,000 a year (benchmark ฿499-590).
- A3. Consumer side: 56M LINE users; 1% pay ฿99 a month.

| Level | Definition | Calculation | Size |
|---|---|---|---|
| TAM (merchant) | All bookable merchants in Thailand | 153k × ฿6,000 | ≈ ฿920M (~US$26M) |
| TAM (consumer) | 1% of LINE users pay ฿99 a month | 560k × ฿1,188 | ≈ ฿665M |
| SAM | Bangkok barbers and salons | (2,501 + 8,322) × ฿6,000 | ≈ ฿65M |
| SOM (3 years) | 3-5% of SAM | 300-500 shops × ฿6,000 | ≈ ฿1.8M-3M |

Reading: the merchant SAM is small. ฿65M a year does not make a venture-scale company on its own. The startup case needs (a) clinics and salons nationwide, (b) take-rate on payments or new-customer fees, or (c) expansion to other LINE markets (Taiwan, Japan) [I]. As a senior project and a first pilot, the size is fine.

## 7. Beachhead vertical

| Criterion | Barbers | Hair / beauty salons | Clinics | Restaurants |
|---|---|---|---|---|
| Visit frequency | High (3-5 weeks) | Medium | Low-medium | High, but mostly walk-in |
| Booking simple? | Yes: one service, one barber | Medium: many services and durations | Hard: doctor, treatment, consult | Easy, but low need |
| No-show pain | Medium (low ticket) | High (long, costly slots) | High | Low-medium |
| Competition | Low in Thai LINE | GoWabi, Fresha | HDmall Jib OS, chat tools | Wongnai, Hungry Hub, Google, ChatGPT |
| Regulation | Low | Low | High: health data under PDPA, medical ad rules | Low |
| Bangkok density | 2,501 | 8,322 | 8,273 | very high |

**Recommendation: start with barbers in Bangkok, then hair salons** [I]. Barbers have simple bookings, frequent repeat visits, owners who use LINE but cannot answer while cutting, and almost no Thai LINE-native competitor. A team of three can onboard 20-30 shops near their university by hand. Salons are the next step, with higher tickets and stronger no-show pain. Clinics pay more but bring PDPA health-data duties and an incumbent. Restaurants have the most competition and the weakest booking habit.

## 8. Why this might fail (contrarian evidence)

1. **Walk-in culture.** Hungry Hub found reservations weak in Thailand and pivoted [S]. Many barbers take walk-ins only [I].
2. **No API to the shop.** A LINE bot cannot write to another OA's chat [I]. Without the shop onboarded, "booking" means a phone call or a human, which does not scale.
3. **Two-sided cold start.** Consumers want many shops; shops want many consumers. The agent is useless with 20 shops [I].
4. **Platform risk.** LINE controls pricing and policy. The Reply API becomes paid above 25,000 messages from Feb 2027 [S]. LINE already runs agents and a booking phone agent in Japan [S].
5. **Weak consumer pull.** Only 26% of APAC consumers want agentic transactions [S]. No Thai data supports "book for me" demand.
6. **Small merchants pay little.** 50-100k restaurants close each year [S]. Barbers may refuse even ฿299 a month [I].
7. **Fast followers.** Abdul, LINE MAN Wongnai and Fresha could each add a LINE booking flow quickly [I].

## 9. Next steps (validation, not code)

1. Interview 15 Bangkok barbers and salons: how they book, no-show rate, chat load, what they pay now.
2. Interview 20 students: how they book now, and would they let an assistant book?
3. Run a fake-door LINE OA or landing page for "book my haircut in LINE". Measure sign-ups.
4. Check LINE's policy on bots acting for users, and the Feb 2027 messaging cost.

## Sources

- Abdul teardown (internal): 2026-11-08-abdul-teardown.md (same folder)
- LINE Thailand 56M users, 12B OA conversations, LINE Premium: https://www.relevantaudience.com/digital-marketing-en/line-thailand-business-solutions-2026/
- LINE Reply API charges from Feb 2027: https://www.relevantaudience.com/social-media-marketing/line-reply-api-25000-free-messages-agency-global-platform/
- LINE OA pricing: https://website.z.com/th/en/lineoa-article-package/ ; https://lineforbusiness.com/th-en/helpcenter/line-oa ; https://developers.line.biz/en/docs/messaging-api/pricing/
- LINE for Business, beauty clinic insights (380k OAs): https://lineforbusiness.com/th-en/trends-and-insights/beauty-clinic
- ActEngine AI: https://www.linecorp.com/pr/news/global/20260312/ ; https://salestechstar.com/b2b-technology/line-plus-unveils-b2b-platform-actengine-ai-to-drive-enterprise-revenue-growth-with-action-oriented-ai/
- LINE 2026 Thai features (Seller Agent): https://www.sanook.com/hitech/1627218/ ; https://mgronline.com/business/detail/9690000076839
- LY Corp Agent i: https://www.lycorp.co.jp/en/news/release/020398/ ; https://www.lycorp.co.jp/ja/news/release/020594/
- LINE AI Reservation (Japan): https://www.travelvoice.jp/english/ly-corporation-launches-the-ai-powered-telephone-answer-service-for-restaurants-covering-from-reservation-to-alternative-ideas
- Original LINE AI assistant (KED Global): https://amp.kedglobal.com/tech,-media-telecom/newsView/ked202402230019
- ChatGPT reservations: https://ppc.land/chatgpt-gains-yelp-table-booking-as-65-want-action-in-ai-local-search/ ; https://magica.com/news/chatgpt-restaurant-reservations-opentable-resy-yelp ; https://www.pcworld.com/article/3123989/i-asked-ai-to-book-dinner-it-made-me-want-to-use-the-app-instead.html
- ChatGPT prices in Thailand: https://iapp.co.th/blog/ai-price-war-thailand-opportunity ; https://www.glbgpt.com/hub/chatgpt-plus-cost-in-thailand/
- ChatGPT paying share: https://the-decoder.com/only-5-percent-of-chatgpts-900-million-weekly-users-pay-and-reportedly-most-arent-worth-much-to-advertisers/
- Thai ChatGPT growth: https://www.khaosodenglish.com/net/2025/09/13/thailand-emerges-as-top-openai-growth-market-in-southeast-asia/
- Google AI Mode booking and calling: https://www.androidcentral.com/apps-software/ai/ai-mode-hunts-down-dinner-reservations-search-labs-experiment ; https://www.engadget.com/ai/google-ai-mode-is-expanding-to-180-countries-and-adding-an-agentic-restaurant-finder-154833417.html ; https://ppc.land/googles-ai-mode-now-books-uk-restaurants-meet-the-8-partner-platforms/
- Apple Intelligence languages: https://www.apple.com/hk/en/newsroom/2025/11/apple-intelligence-expands-to-new-languages-including-traditional-chinese/
- Alexa+ countries: https://aboutamazon.com/news/devices/alexa-plus-international-launch
- Thai chatbot prices: https://iapp.co.th/products/chinda ; https://hdmall.co.th/jib-os/blog/7-line-oa-chat-systems-for-clinics/ ; https://www.chat.goragod.com
- HDmall on LINE bots and no-shows: https://hdmall.co.th/jib-os/resources/139-line-bot-booking-for-clinics/ ; https://hdmall.co.th/jib-os/resources/26-clinic-appointment-system-reduce-no-shows/
- Hungry Hub: https://e27.co/how-hungry-hub-survived-the-pandemic-to-become-a-leading-player-in-special-occasion-dining-in-thailand-20230220/ ; https://www.thestorythailand.com/en/?p=120223
- QueQ: https://www.cbinsights.com/company/queq
- GoWabi: https://e27.co/how-this-startup-makes-health-and-beauty-service-booking-thrives-in-thailand-20181217/ ; https://www.gowabi.com/customer_faq/gowabi-affiliates-faq-en/ ; https://www.omise.co/en/voice-of-customers/featured-customer-gowabi
- Fresha: https://fresha.com/pricing ; https://www.fresha.com/lp/en/bt/barbershops/in/th-krung-thep-maha-nakhon
- Wongnai reservations: https://brandinside.asia/?p=254973 ; https://www.nationthailand.com/business/30377413
- Barber and salon counts: https://poidata.io/report/barber-shop/thailand ; https://poidata.io/report/beauty-salon/thailand
- DBD beauty businesses: https://www.nationthailand.com/business/economy/40046901
- Private clinics: https://www.nationthailand.com/health-wellness/40056407
- Restaurants: https://www.thestar.com.my/aseanplus/aseanplus-news/2024/09/21/with-over-600000-closures-thai-restaurant-industry-in-the-midst-of-a-crisis ; https://thethaiger.com/news/business/food-delivery-market-grows-as-economy-threatens-restaurants
- Salon booking surveys (US): https://www.zenoti.com/thecheckin/salon-spa-booking-communication-trends
- Agentic commerce trust: https://www.krungsri.com/en/research/research-intelligence/agentic-commerce-2026 ; https://fintechnews.sg/137054/ai/trust-and-control-emerge-as-key-barriers-to-ai-enabled-commerce/ ; https://www.marketing-interactive.com/ai-is-changing-how-sea-shops-but-trust-still-wins-the-checkout
