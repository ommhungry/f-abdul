# Fact check: LINE booking-agent research (red team)

- Date: 2026-11-08 (all sources accessed 2026-11-08)
- Checks: `2026-11-08-agent-execution-tech.md`, `2026-11-08-agent-market-competitors.md`, `2026-11-08-agent-legal-economics.md` (context: `2026-11-08-abdul-teardown.md`)
- Method: fresh web searches and page reads. No sign-ups, no contact with anyone, no probing of Abdul. The three source files were not edited.
- Tags: [V] read on the primary or official page; [S] secondary source (press, reseller, vendor); [I] our inference; UNVERIFIED = no source found.

## Scorecard

| # | Claim | Verdict |
|---|---|---|
| 1 | Fresha MCP / AI-assistant integration, ~Sept 2026, hands off to Fresha | WEAKER (launched August, not September) |
| 2 | Booksy API partner-only; SimplyBook.me API can create bookings | CONFIRMED |
| 3 | Reserve with Google / Actions Center in Thailand | CONFIRMED (Thailand is listed; joining is gated) |
| 4 | LINE bot cannot message other OAs or non-friends; one bot per group | CONFIRMED (with one exception) |
| 5 | LINE OA Thailand plan prices and quotas | CONFIRMED |
| 6 | Reply API above 25,000/month paid (฿5,000) from 1 Feb 2027 | CONFIRMED |
| 7 | LY Corp Agent i, AI phone booking in Japan; LINE TH ActEngine / Seller Agent | CONFIRMED (and the threat is bigger than written) |
| 8 | Google AI Mode booking / "Ask for me": not Thailand | CONFIRMED |
| 9 | ChatGPT books restaurants via OpenTable / Resy / Yelp | CONFIRMED |
| 10 | LINE Premium Thailand ~3M subscribers at ฿169/month | CONFIRMED |
| 11 | Merchant counts; LINE MAN Wongnai ~700k merchants | WEAKER |
| 12 | Hungry Hub moved away from reservations because Thais walk in | CONFIRMED (but the story is from 2014-2017) |
| 13 | ETA B.E. 2544 attribution of automated messages | CONFIRMED: section 15, paragraph two (exact wording below) |
| 14 | ETDA draft AI law status 2026 | CONFIRMED (draft only; one word needs fixing) |
| 15 | Claude Haiku 5.5 / Sonnet 5.5 prices | CONFIRMED |
| 16 | "26% of SEA consumers want AI to complete transactions" | WRONG (the survey is global, not SEA or APAC) |

**Counts: 13 confirmed, 2 weaker, 1 wrong, 0 unverifiable.**

---

## Detail per claim

### 1. Fresha MCP server: WEAKER
- Evidence: Fresha announced native ChatGPT and Claude integrations on its public MCP server. The Business Wire release is dated 4 Aug 2026, and a newsletter says bookings went live in August [S, businesswire.com/news/home/20260804452505 (403 on fetch, date taken from the URL); S, buttondown.com/bizconvos]. CX Network's article is dated 9 Sept 2026. It says users "are directed to Fresha to confirm the booking and securely complete payment" [S, cxnetwork.com]. Sign-in is reportedly required before tools are listed at mcp.fresha.com [S, xpay.sh]. The open-source "mcp-fresha" package is a third-party Snowflake data connector, not Fresha's server [S, mcpservers.org].
- Corrected wording: "In August 2026 Fresha launched native ChatGPT and Claude integrations on a public MCP server. They cover discovery and live availability; the user confirms and pays on Fresha. No public booking-write API for third parties was found."

### 2. Booksy and SimplyBook.me APIs: CONFIRMED
- Booksy: there is a partner-facing REST API. Anonymous calls return 401. Credentials come from partner onboarding (an RS256 JWT is exchanged for a bearer token). The API is described as alpha and gated behind partner outreach [S, apis.io/apis/booksy/booksy-public-api; S, supergood.ai/api-report-card/booksy (vendor)].
- SimplyBook.me: a JSON-RPC 2.0 API with a `book` method that "validates and registers a new booking". It needs a token from the merchant's API key. Several developer threads report "Method not found" until a plugin or feature is enabled, and some report generic errors [S, help.simplybook.me User_API_guide; S, tech-support.simplybook.me threads 1479, 1123].
- Corrected wording: "SimplyBook.me can create bookings through its API, but only with each merchant's API key and the right plugin enabled. Expect integration friction."

### 3. Reserve with Google / Actions Center in Thailand: CONFIRMED, with caveats
- Evidence: the Actions Center Reservations (end-to-end) platform-policy page lists "Thailand | TH" in its supported-countries table [V, developers.google.com/actions-center/verticals/reservations/e2e/policies/platform-policies]. Search snippets of the Appointments Business Link table show "Thailand | TH | Supported | Supported" [S, developers.google.cn/actions-center/verticals/appointments/bl/policies/platform-policies]. Appointments now run as "Redirect": Google sends the user to the provider's booking page, and partners apply through an interest form or a Google business-development contact [V, developers.google.com/actions-center/verticals/appointments/redirect/overview]. Hungry Hub reports a Reserve with Google partnership in Thailand [S, search snippet].
- Corrected wording: "Thailand is listed for Actions Center reservations and appointments (Business Link / Redirect). Becoming a scheduling provider requires Google's approval and is not self-serve. Appointment bookings finish on the provider's page." The execution-tech file's "UNVERIFIED" can become [S]/[V]. Plan step 4 (become a provider later) is possible but not guaranteed.

### 4. LINE Messaging API limits: CONFIRMED
- One bot per group: "At any time, only one LINE Official Account can be in a group chat or multi-person chat" [V, developers.line.biz/en/docs/messaging-api/group-chats/].
- Non-friends: the API reference lists "the user hasn't added the target LINE Official Account as a friend" as a cause of send errors [V, developers.line.biz/en/reference/messaging-api]. Exception: LINE Notification Messages (LON) can reach a user by phone number without a friend add. It is a corporate partner option (FWD Thailand uses it), not open to a student bot [V, developers.line.biz partner-docs/line-notification-messages; S, thailand4.com FWD].
- OA to OA: no official statement was found. An OA is not a LINE user, so it cannot become another OA's friend [I]. Treat as [I].
- Corrected wording: "Our OA can push only to users who added it. It cannot write into a shop's OA chat. Only one OA fits in a group. LON reaches non-friends but is enterprise-only."

### 5. LINE OA Thailand pricing: CONFIRMED
- Evidence: LINE's own notice (19 June 2024) cut the Free plan quota from 500 to 300 messages. It set Pro at ฿1,780 (down from ฿1,869) with 35,000 messages, extra messages ฿0.06, effective 1 Aug 2024 [V, lineforbusiness.com/th/news/20240619_1]. The notice does not cover Basic. A 2026 reseller page lists Free 300; Basic ฿1,280 / 15,000 / ฿0.10 extra; Pro ฿1,780 / 35,000 / ฿0.06 extra [S, business.true.th]. LINE's OA feature page says its prices exclude 7% VAT (that text is next to Premium ID; the plan table is an image) [S/V, lineforbusiness.com/th-en/service/line-oa-features]. Push, multicast, broadcast and narrowcast count toward the quota; reply messages are "not counted" [V, developers.line.biz/en/docs/messaging-api/pricing].
- Corrected wording: keep the figures. Tag Basic as [S] (reseller), not [V]. Say "reply not counted toward quota (until the Feb 2027 Reply API change)" instead of "reply free".

### 6. Reply API charge from 1 Feb 2027: CONFIRMED [V]
- Evidence: LINE's Thai notice says the first 25,000 Reply API messages a month are free, starting 1 Feb 2570 (2027). Unused quota does not carry over. The Unlimited plan is ฿5,000 a month and is bought through a LINE-designated Agency Partner on the Agency Global Platform (AGP), with no migration fee [V, lineforbusiness.com/th/news/20261030]. The notice is dated 30 Oct 2026 [S, relevantaudience.com]. VAT, tiers between 25k and unlimited, and how multi-message replies are counted are all unstated.
- Corrected wording: no change. Upgrade the tag from [S] to [V].

### 7. LY Corp Agent i, LINE AI booking, LINE Thailand agents: CONFIRMED, and stronger than written
- Agent i launched 20 Apr 2026. It merged LINE AI and Yahoo! JAPAN AI Assistant. By 28 Aug 2026 it had 27 domain agents and about 12M DAU. A standalone app and task features were planned for Oct 2026 and autumn. "Agent i in chat" is due in 2026 [V, lycorp.co.jp/en/news/release/020398; V, lycorp.co.jp/ja/news/release/020594; S, search summary of the 28 Aug briefing].
- "LINE AI予約" (AI phone booking for restaurants) became generally available on 22 Oct 2026. It handles booking, change and cancel 24/7 on the Toreta ledger, from ¥10,000 a month [V, lycorp.co.jp/ja/news/release/020924; S, watch.impress.co.jp]. LY Corp agreed to acquire Toreta on 30 Jan 2026 to make LINE OA plus Toreta a native in-LINE reservation flow [S, travelvoice.jp/20260130-159175].
- Thailand: ActEngine AI (LINE Plus, 12 Mar 2026). Its sales agent went live in LINE MAN Wongnai on 26 Feb 2026 [V, linecorp.com/pr/news/global/20260312]. "Seller Agent" for OA and MyShop [S, sanook.com/hitech/1627218].
- Corrected wording: "LY Corp runs Agent i and, since Oct 2026, a commercial AI phone-booking service in Japan built on Toreta, which it bought in Jan 2026. This is a working template LINE could bring to Thailand."

### 8. Google AI Mode agentic booking / "Ask for me": CONFIRMED (Thailand not included)
- Evidence: agentic restaurant booking launched in the US in Aug 2025 for AI Ultra users [S, engadget, techcrunch]. On 10 Apr 2026 it expanded to Australia, Canada, Hong Kong, India, New Zealand, Singapore, South Africa and the UK [S, 9to5google.com/2026/04/10/google-ai-mode-redesign; S, winbuzzer]. Japan followed on 6 Nov 2026 with Gurunavi, Ikyu and ebica [S, jetstream.blog]. Beauty and wellness appointments (Booksy) started with Pro and Ultra subscribers [S, yourstory]. "Ask for me" and agentic calling are US only. Google's help page was updated 24 Jul 2026 [S, seroundtable; S, localfalcon.com]. No source mentions Thailand.
- Corrected wording: "AI Mode books restaurants in the US plus 9 markets (8 in Apr 2026, Japan in Nov 2026). Thailand is not listed. Singapore and Hong Kong are nearby, so Thailand is plausible within 12 months [I]."

### 9. ChatGPT restaurant booking: CONFIRMED
- Evidence: launched about 10 Aug 2026. OpenTable globally, Resy in the US, Yelp in the US and Canada. Bookings complete in chat when no credit card is needed. Changes and cancellations go through the provider [S, androidauthority.com; S, ppc.land; S, releases.sh].
- Corrected wording: no change. Add "completes in chat when no card is required."

### 10. LINE Premium Thailand: CONFIRMED [S]
- Evidence: launched 9 Jul 2025 at ฿169 a month. Over 3M paying users about a year after launch, per LINE as reported by the press [S, pattayamail.com; S, posttoday.com; S, brandinside.asia/?p=280306]. LINE's own release was not found.

### 11. Merchant counts: WEAKER
- Barbers: 13,483 nationally and 2,501 in Bangkok [S, poidata.io/report/barber-shop/thailand, June 2026, scraped]. Another vendor counts only 489 barbers in Bangkok [S, rentechdigital.com]. These are scraped directory counts that conflict, not official statistics.
- Salons 31,264: poidata only [S]. Same weakness.
- Private clinics 40,552 (2024), Bangkok 8,273: [S, nationthailand.com/health-wellness/40056407, citing the Department of Health Service Support]. This one holds.
- LINE MAN Wongnai "700k+ merchant partners" appears in the ActEngine release [V, linepluscorp.com 20260312]. A June 2026 LY Corp story says "500k+" [S, lycorp.co.jp/en/story/20260602/lmwn.html].
- Corrected wording: "Barbers about 2.5k-13.5k and salons about 31k (scraped directories, low confidence). Private clinics 40.6k (government, via press). LMWN 500k-700k+ merchants, depending on the source." Show the SAM as a range, not ฿65M.

### 12. Hungry Hub and walk-ins: CONFIRMED (dated)
- Evidence: Hungry Hub launched as a reservation tool in 2014. It "failed as there was no real need ... one could walk into a restaurant and get a table easily". It pivoted in 2016-2017 to fixed-price dining deals [S, e27.co 2023 article; S, cambodiainvestmentreview.com 2024].
- Corrected wording: add "(2014-2017, restaurants only; not evidence about barbers)".

### 13. ETA B.E. 2544 attribution: CONFIRMED, exact section found
- Section 15, ETDA official English translation [V, etda.or.th ENG-Version PDF; the Thai text is authoritative]:
  > "Section 15. Whoever sends a data message by whatever means, it shall be deemed that the data message belongs to such person. As between the originator and the addressee, a data message is deemed to be that of the originator if it is sent by: (1) a person who is authorized to act on behalf of the originator in respect of that data message; or (2) an information system programmed to operate automatically in advance, by the originator or a person authorized to act on behalf of the originator."
- The 2019 amendment (ETA No. 3) [V, etda.or.th ETA-3-ACT-ENG PDF] made three changes that matter here:
  - It replaced s.16 para 1(1): the recipient must have "appropriately ascertained whether the data message is from the sender in accordance with the procedures agreed to".
  - It added s.13/2: a contract is not invalid only because it was made by automated message systems.
  - It added **s.17/1**: a person who makes an **input error** with another party's automated system **that gives no chance to correct it** may withdraw that part of the message. This is a legal reason for our confirm-and-edit screen. The legal file missed it.
- Corrected wording: replace "ETA s.15 ... wording from memory, UNVERIFIED" with the quote above, tagged [V]. Add s.17/1. It still needs a LAWYER on whether our bot acts for the user (s.15(2)) or is a third party.

### 14. ETDA draft AI law: CONFIRMED (draft only)
- Evidence: ETDA released a new draft AI Act on 2 Jul 2026 for about a 30-day public hearing. It is risk-based, has extraterritorial reach, adds transparency rules for AI content, brings **strict liability** for AI damage, and may require local data processing for "national importance" AI [S, tilleke.com/insights/thailand-releases-new-draft-artificial-intelligence-act; S, mondaq/legal500]. A Thai blog reports three tiers (prohibited, high-risk, notify/licence), logs kept at least 6 months, and fines of ฿1-5M [S, grandlinux.com, low quality]. No evidence of cabinet approval or enactment by 8 Nov 2026 [S, search of Thai news]. The ETA revision is a separate bill. It was debated around 28 Oct 2026 [S, thairath.co.th/news/politic/2968998].
- Corrected wording: "Draft only (2 Jul 2026 hearing). Not enacted. Tilleke says 'strict liability'; 'joint' liability is not confirmed."

### 15. Claude API prices: CONFIRMED [V]
- Evidence (platform.claude.com/docs/en/about-claude/pricing), per million tokens:
  - Haiku 5.5, prompts up to 100k tokens: $0.10 input, $0.125 cache write (5 min), $0.01 cache hit, $0.50 output.
  - Haiku 5.5, prompts over 100k tokens: $0.50 input, $0.05 cache hit, $2.50 output.
  - Sonnet 5.5: $2 input, $2.50 cache write (5 min), $0.10 cache hit, $10 output.
  - Batch is half price. US-only inference costs 1.1x.
- Corrected wording: no change.

### 16. "26% of SEA consumers want AI to complete transactions": WRONG
- Evidence: the 26% comes from an **Alipay+ / S&P Global survey of 6,000 consumers in nine markets across Asia, Europe and the US**: "only 26% of consumers shared interest in using agentic AI for transactions". The **24% "firm refusal to delegate purchases"** comes from a different **global** survey, Checkout.com's Agentic Commerce 2026 (12,000+ consumers) [S, fintechnews.sg/137054, 11 Sept 2026]. Neither is SEA or APAC, and Thailand is not confirmed in either. A separate Visa/YouGov APAC survey (14 markets, 14,764 people) found 32% reluctant to share data with AI. It also found 42% in India and Vietnam willing to let AI complete transactions on its own [S, techwireasia.com 2026/02; S, fintechnews.sg/126764].
- Corrected wording: "Globally, only 26% of consumers say they are interested in agentic AI for transactions (Alipay+/S&P Global, 9 markets), and 24% refuse to delegate purchases (Checkout.com, global). No Thai figure exists." The market file must stop calling these "APAC".

---

## What all three files missed

1. **LINE MAN Wongnai bought JERA Cloud (Aug 2025)** [S, lmwn.com/jera-cloud-acquisition-en; brandinside.asia]. JERA Cloud is a beauty and wellness POS with appointment scheduling, membership and CRM, used by 1,700+ branches of clinics, dental clinics, salons and spas. With Wongnai Reservations on Google and the ActEngine sales agent, **LMWN already has a salon and clinic booking ledger in Thailand**. This is a direct merchant-side competitor in salons, the files' "next step" vertical. Barbers remain less covered [I].
2. **LINE's own booking stack in Japan.** LINE OA has a native "LINEで予約" (reserve in LINE) feature [S, lycbiz.com/jp/service/line-official-account/reservation]. The Toreta deal makes in-chat reservation a core LINE product. A Thai rollout would hit a LINE-only booking layer directly [I].
3. **LINE Thailand agent-developer program.** At LINE DEV CONF 2026 (article dated 19 Oct 2026), LINE's CTO announced Agent APIs, MCP, Skills and agent-to-agent protocols for third-party agents. A Pioneer Program and a closed beta through **LINE HACKS 2027** are planned [S, thansettakij.com/technology/ai/671747]. This is a **risk** (LINE builds the assistant layer) and an **opportunity** (apply as a student team, and build on official agent APIs instead of fighting the one-bot-per-group and OA-to-OA limits).
4. **Thai LINE booking tools priced like ours.** Goragod chat offers LINE queue booking from ฿499 a month [S, chat.goragod.com]. Jib OS (HDmall) starts at about ฿6,000 a month for clinics [S, hdmall.co.th]. "Free booking page" is not a unique wedge.
5. **ETA s.17/1 input-error withdrawal** (see #13). This is a legal reason, not just UX, to always show an editable confirmation.
6. **Google is moving through Asia** (Singapore and Hong Kong in Apr 2026, Japan in Nov 2026). Wongnai is already a Reserve-with-Google source. A Thai AI Mode launch would cover restaurants first [I].
7. **LON (LINE Notification Messages)** lets big companies message non-friends by phone number. Incumbents can reach users we cannot [V/S].

No Thai consumer-side "assistant that books for you" was found. That part of the gap claim still holds [S, searches; UNVERIFIED as a negative].

---

## Overall verdict: PROCEED WITH CHANGES

The idea is a LINE assistant that books barbers and salons, built on our own merchant booking tool.

Why not STOP:
- The technical facts behind the plan hold (#2-#6, #13, #15). There is no open Thai booking API, LINE cannot write into a shop's chat, and model costs are tiny. Owning the merchant calendar is the only reliable way to "actually book."
- No Thai consumer booking agent exists yet. Barbers look under-served by LMWN/JERA, GoWabi and Fresha [I].
- As a senior project it is buildable and measurable: 20-40 shops and a booking completion rate.

Why not plain PROCEED:
- **Demand evidence is weaker than written.** The 26% stat is global and was misattributed (#16). Merchant counts are low-confidence (#11). The walk-in evidence is old and about restaurants (#12). There are still no Thai barber interviews.
- **Competition on the merchant side is stronger than written.** LMWN owns JERA Cloud in beauty and clinics, LINE runs reservation and AI booking in Japan, and Thai LINE booking tools exist from ฿499 a month.
- **Platform risk is concrete.** The Reply API is charged from Feb 2027, LINE has its own agent program for 2027, and there is one OA per group.

Required changes:
1. **Barbers first; stay away from salons and clinics while LMWN/JERA is strong there.** In weeks 1-2, interview 15+ barbers. Kill or pivot if most are walk-in only or refuse a booking page.
2. **Position the product as a merchant tool first** (booking page, no-show reminders, daily schedule to the owner's LINE). The consumer agent is the second layer. Do not depend on consumer subscriptions.
3. **Apply to LINE's Pioneer Program / LINE HACKS 2027 agent track.** Design so we could plug into LINE's agent APIs later rather than compete with them.
4. **Fix the documents:** the Fresha date, the 26% source, merchant-count ranges, the ETA s.15 quote plus s.17/1, and the Basic price tag as [S].
5. **Budget for the ฿5,000 a month Reply API plan** once there are about 300 active users. Prefer LIFF pages over chat turns for flows that would otherwise use many replies.
6. **Make the confirmation screen editable** (ETA s.17/1). Use "request sent" until the shop confirms.

---

## Sources (accessed 2026-11-08)
- Fresha: https://www.cxnetwork.com/cx-experience/articles/fresha-goes-conversational-with-native-claude-and-chatgpt-integration-for-beauty-bookings/amp ; https://www.businesswire.com/news/home/20260804452505/en/ ; https://buttondown.com/bizconvos/archive/quiet-day-for-launches-but-fresha-now-books/ ; https://www.xpay.sh/agent-ready-index/fresha/ ; https://mcpservers.org/de/servers/199-biotechnologies/mcp-fresha
- Booksy: https://apis.io/apis/booksy/booksy-public-api/ ; https://supergood.ai/api-report-card/booksy
- SimplyBook.me: https://help.simplybook.me/index.php/User_API_guide ; https://tech-support.simplybook.me/t/1479-client-api-booking-methods-return-32601-method-not-found-while-auth-and-reads-work/1480 ; https://tech-support.simplybook.me/t/1123-attempt-to-create-a-booking-through-grpc-book-method-throwing-unexpected-error/1123
- Actions Center: https://developers.google.com/actions-center/verticals/reservations/e2e/policies/platform-policies ; https://developers.google.cn/actions-center/verticals/appointments/bl/policies/platform-policies?hl=en ; https://developers.google.com/actions-center/verticals/appointments/redirect/overview
- LINE API: https://developers.line.biz/en/docs/messaging-api/group-chats/ ; https://developers.line.biz/en/reference/messaging-api/nojs/ ; https://developers.line.biz/en/docs/messaging-api/pricing/ ; https://developers.line.biz/en/docs/partner-docs/line-notification-messages/overview/ ; https://thailand4.com/en/1622633
- LINE pricing: https://lineforbusiness.com/th/news/20240619_1 ; https://business.true.th/en/solutions/line-business-solution ; https://lineforbusiness.com/th-en/service/line-oa-features ; https://www.lineforbusiness.com/th/news/20261030 ; https://www.relevantaudience.com/social-media-marketing/line-reply-api-25000-free-messages-agency-global-platform/
- LY Corp / LINE: https://www.lycorp.co.jp/en/news/release/020398/ ; https://www.lycorp.co.jp/ja/news/release/020594/ ; https://www.lycorp.co.jp/ja/news/release/020924/ ; https://www.watch.impress.co.jp/docs/news/2150496.html ; https://www.travelvoice.jp/20260130-159175 ; https://www.lycbiz.com/jp/service/line-official-account/reservation ; https://www.linecorp.com/pr/news/global/20260312/ ; https://www.sanook.com/hitech/1627218/ ; https://www.thansettakij.com/technology/ai/671747 ; https://www.lycorp.co.jp/en/story/20260602/lmwn.html
- Google: https://9to5google.com/2026/04/10/google-ai-mode-redesign/ ; https://winbuzzer.com/2026/04/13/google-ai-mode-rolls-out-restaurant-booking-to-8-countries-xcxwbn/ ; https://jetstream.blog/en/google-search-ai-mode-restaurant-booking-japan-rollout/ ; https://yourstory.com/ai-story/googles-ai-mode-adds-agentic-booking-for-tickets ; https://www.seroundtable.com/google-ask-for-me-answers-39748.html ; https://www.localfalcon.com/blog/googles-gemini-is-starting-to-call-local-businesses-for-customers
- ChatGPT: https://androidauthority.com/chatgpt-restaurant-reservations-and-waitlists-3696712 ; https://ppc.land/chatgpt-gains-yelp-table-booking-as-65-want-action-in-ai-local-search/ ; https://releases.sh/release/rel_KXqfnr8PTtwmVrlJiDWYG-restaurant-reservations-findable-in-chat-with-opentable-resy-yelp
- LINE Premium: https://www.pattayamail.com/news/3-million-thais-pay-for-line-premium-to-undo-costly-message-mistakes-560142 ; https://www.posttoday.com/business/746498 ; https://yespress.io/line-company-thailand
- Merchant counts: https://poidata.io/report/barber-shop/thailand ; https://rentechdigital.com/smartscraper/business-report-details/thailand/list-of-barber-shops-in-bangkok ; https://www.nationthailand.com/health-wellness/40056407 ; https://www.linepluscorp.com/pr/news/global/20260312/ ; https://cfotech.asia/story/line-plus-unveils-actengine-ai-for-merchant-support
- Hungry Hub: https://e27.co/how-hungry-hub-survived-the-pandemic-to-become-a-leading-player-in-special-occasion-dining-in-thailand-20230220/ ; https://cambodiainvestmentreview.com/2024/04/10/e119-surasit-sachdev-on-spearheading-innovation-in-thailands-restaurant-reservation-sector-regional-expansion-plan-by-2025/
- ETA: https://etda.or.th/getattachment/8faa736b-3235-49c8-8b01-d37ff53a9a45/ENG-Version.aspx ; https://etda.or.th/getattachment/ef10d3c2-1d31-43c9-96cd-4a58b4fcb10d/ETA-3-ACT-ENG-Version.aspx
- AI law: https://www.tilleke.com/insights/thailand-releases-new-draft-artificial-intelligence-act/ ; https://www.legal500.com/intelligence/thailand/media-telecoms-it-entertainment/thailand-releases-new-draft-artificial-intelligence-act ; https://www.grandlinux.com/blogs/thailand-draft-ai-act-2026-business-impact.html ; https://www.thairath.co.th/news/politic/2968998
- Claude pricing: https://platform.claude.com/docs/en/about-claude/pricing
- Consumer surveys: https://fintechnews.sg/137054/ai/trust-and-control-emerge-as-key-barriers-to-ai-enabled-commerce/ ; https://fintechnews.sg/126764/ai/apac-consumers-embrace-ai-though-security-concerns-slow-agentic-payment-adoption/ ; https://techwireasia.com/2026/02/ai-commerce-trust-gap-asia-pacific/
- Missed competitors: https://lmwn.com/jera-cloud-acquisition-en/ ; https://brandinside.asia/line-man-wongnai-acquire-jera-cloud/ ; https://kr-asia.com/thailands-line-man-pushes-beyond-food-delivery-eyes-2027-ipo ; https://www.chat.goragod.com ; https://hdmall.co.th/jib-os/blog/7-line-oa-chat-systems-for-clinics/
