# PO review 01: Thai competitor check (Thai-language search + app-store check)

Date of check: 2026-11-21 (all "accessed" dates below are this date unless stated). Author: market researcher, read-only on the repo. No repo file was modified. No real personal data was used.

## 0. Method and tag rules

Tags: [V] I saw it on the primary page or the store's own data; [S] secondary source (search-engine summary, press, listicle); [I] my inference; UNVERIFIED. Tags are never upgraded.

How the primary pages were read, so the reader can judge [V]:
- Apple App Store, Thailand storefront: Apple's own lookup/search endpoint (`itunes.apple.com/lookup?country=th&id=...`) and the `apps.apple.com/th` listing HTML. Gives rating, rating count, version, last-update date, declared languages, description text and in-app-purchase prices in THB. Rating counts are for the TH storefront.
- Google Play TH: `play.google.com/store/apps/details?id=...&hl=th&gl=TH` fetched as HTML. Rating value/count come from the page's own structured data, installs are Play's bucket ("500K+"), update dates are shown in Buddhist years (2569 = 2026). Play rating counts may be global, not TH-only [I]. Descriptions are the developer's text.
- Some official web pages (meowjot.com, set.or.th, bot.or.th, scb.co.th, walletstory.app, moneypod.app, LINE pages) were read with a fetch tool that returns a model-written summary of the page. I tag those [V] but flag "fetch summary" where a number matters, because the summary step is a possible error source.
- Web-search answers were used only to find leads and are tagged [S].
- I did not install or test any app. Everything below is what the apps advertise. "Not advertised" is never proof the feature is missing (same omission rule as `03-market-business.md` 3.2).

Thai-language and English queries run (search log, for CLAUDE.md compliance): แอปบันทึกรายรับรายจ่าย แอปจัดการหนี้ สินทรัพย์สุทธิ; แอป คำนวณ ผ่อนหนี้ ตารางผ่อนชำระ โปะเงินต้น; LINE OA บัญชีรายรับรายจ่าย / บอท LINE จดรายรับรายจ่าย; แอป จัดการหนี้ ติดตามหนี้ บัตรเครดิต สินเชื่อ ตารางผ่อน แจ้งเตือนวันครบกำหนด; เครื่องมือ คำนวณภาระหนี้ ธปท. สคบ. ก.ล.ต.; Notion/Excel template บัญชีรายรับรายจ่าย หนี้สิน สินทรัพย์; ธปท. Open Data / Your Data; K PLUS My Spending; SCB Just4U; Krungthai NEXT / ttb touch / GSB MyMo; Piggipo, Finnomena, Dime!, SET Happy Money, Lumpsum, MeeTang; net worth tracker Thailand; debt payoff app Thailand; plus direct store searches on App Store TH and Google Play TH for: เหมียวจด, Piggipo, Finnomena, Dime, Jitta, Lumpsum, Kept, Streaming, จัดการหนี้, ผ่อนชำระ ตารางผ่อน, สินทรัพย์ หนี้สิน, net worth, รายรับรายจ่าย, คำนวณสินเชื่อ.

## 1. Headline findings (read this first)

1. The "no competitor" claim cannot be called proven, and in its broad form it is false. At least three live apps advertise most of the balance-sheet idea, and one government-linked free app advertises "assets + debts + net worth + financial health" and has been on the store since 2019.
2. Closest rivals found (all [V], details in section 3):
   - SET Happy Money (Stock Exchange of Thailand): free, Thai, income/expense plus assets and liabilities, net worth comparison over time, bill reminders, a "financial health" rating. No loan schedule or payoff simulator advertised.
   - Wallet Story (Wallet Story Co., Ltd., Thai storefront, updated 2026-11-16, v18.5.0): Thai UI, slip reading, debts with installment plans, credit-card statement/due-date reminders, land/house/car in net worth, stocks/funds, cash-flow forecast, financial health score, AI coach. No amortization split or payoff simulator advertised.
   - AssetFlows (individual developer, first release 2026-04-13): loan and mortgage accounts with amortization schedules and a payoff simulator, Thai tax-fund limits, stocks/funds/bonds, reminders, "Financial Health". English only on the listing, no ratings yet, Pro THB 99/month.
   - MeeTang - มีตังค์ (individual developer, first released 2026-10-06, 100+ installs on Play): Thai-only, Thai AI assistant that answers from the user's own data with a "not financial advice" line, credit-card due dates, hire-purchase debt schedule ("ตารางผ่อน"), gold and Thai mutual funds with SEC NAV, bank-notification capture.
3. Free tools already cover the payoff arithmetic: Bank of Thailand "โปรแกรมแก้หนี้ DIY", and free Thai calculators on Google Play that show an amortization table and "pay extra, save X interest and Y months".
4. The gap that survives is narrow and about the bundle, not any single piece: one Thai-UI tracker that puts multi-type assets, a loan amortization engine (principal/interest per instalment), due-date view, debt-health ratios and a what-if payoff simulator in one balance sheet. I found no single app advertising all of that with a Thai UI as of 2026-11-21, but three come within one or two features and the field is moving monthly.

## 2. Competitor profiles

Legend for loans column: A = amortization/instalment schedule advertised, P = payoff/extra-payment what-if advertised, D = debts tracked without a schedule, - = not advertised on the pages I read.

| App (owner) | What it does | Loans / amortization / payoff | Assets across types | Due-date view | LINE channel | Thai quality | Price (THB, TH store) | Users / rating | Last update | URL |
|---|---|---|---|---|---|---|---|---|---|---|
| MeowJot เหมียวจด (KASIKORN X / KX Ventures) | Auto-logs expenses from transfer slips of many bank apps; credit-card statement upload (paid); categories, budgets | - (home page and store text mention no assets or loans) | - | Recurring entries only (paid); no due-date reminders seen | None seen; web home page does not mention LINE | Site in Thai/English; iOS listing declares English only; Play text is Thai | Free tier; Silver 79/mo, Gold 149/mo, annual 499/799 on App Store IAP [V]; website shows 42 and 67 with struck-through 79 and 149 (see 5.1) | iOS 4.87 from 11,378 ratings; Play 4.83 from 7,784, 500K+ installs | iOS 2026-10-19; Play 2026-10-02 | https://www.meowjot.com/ ; https://www.meowjot.com/pricing ; https://apps.apple.com/th/app/meowjot-auto-track-expenses/id6457365306 ; https://play.google.com/store/apps/details?id=tech.kbtg.meowjot |
| SET Happy Money (Stock Exchange of Thailand) | Income/expense by category, daily budgets, assets and liabilities lists, net worth over time, financial health "ฟิต/ฟุบ", savings goals, bill reminders | D (liabilities listed and categorised; no schedule or payoff stated) | Yes, categorised assets (types not enumerated in the listing) | Bill reminders | None seen | Thai, written by SET | Free | iOS 4.83 from 20,192; Play 4.84 from 9,294, 100K+ | iOS 2025-12-31 (v1.6.2); Play 2025-12-18 | https://www.set.or.th/happymoney/app.html ; https://apps.apple.com/th/app/set-happy-money/id1483898402 ; https://play.google.com/store/apps/details?id=th.or.set.happymoney |
| Wallet Story (Wallet Story Co., Ltd.) | Slip/receipt reading, voice entry, budgets, accounts, cards, debts, forecast, shared books, business books, AI coach | D + installment plans; amortization/payoff not advertised | Bank, cash, e-wallet, cards, land/house/car in net worth, stocks and funds | Card statement and due-date reminders; recurring reminders; "to pay" list widget | None seen (reads bank notifications/SMS) | Thai UI among 25 languages; slip reading of Thai transfers | Free; Monthly sub 49, Yearly sub 499, PRO 499 (one-time, per listing wording), icon/book add-ons 249 | iOS 4.83 from 28,490; Play 4.72 from 18,298 | iOS 2026-11-16 (v18.5.0); Play 2026-11-16 | https://walletstory.app ; https://apps.apple.com/th/app/wallet-story-expense-manager/id1482496559 ; https://play.google.com/store/apps/details?id=com.walletstory.app |
| MeeTang มีตังค์ (individual dev) | Thai expense tracker with slip OCR, bank-notification capture, budgets, credit cards, instalment debts, gold and Thai funds, Thai AI assistant | A (hire-purchase schedule, per release note); principal/interest split and payoff what-if not stated | Wallets, multi-currency, gold bars (96.5%), Thai mutual funds | Credit-card cycle and due date, monthly bills, instalments | None seen (reads bank messages that arrive in LINE) | Thai-only listing | Free; price points seen on listing: 69, 79, 590, 690, 1,490 (labels not captured) | iOS 5.0 from 5 ratings; Play 100+ installs, no rating shown | iOS and Play 2026-11-18; first release 2026-10-06 | https://apps.apple.com/th/app/meetang-%E0%B8%A1-%E0%B8%95-%E0%B8%87%E0%B8%84/id6818940531 ; https://play.google.com/store/apps/details?id=com.mamat1123.meetang |
| AssetFlows (individual dev) | Portfolio tracker plus cash/expenses, tax-saving fund limits (SSF/RMF/ThaiESG), DCA, dividends, bonds, insurance | A and P (v1.4.3, listed "10 Sept", year 2026 by my inference from the 2026-04-13 first release: "loan and mortgage accounts with amortization, a payoff simulator, and Financial Health integration") | Stocks, funds, bonds, "Other" (crypto, gold, commodities), insurance; bank balances | Transaction reminders (v1.3.3) | None seen | Listing language: English only | Free; Pro 99/month, 499/6 months, 899/year | No rating yet; fewer than needed to display | v1.4.7 on 2026-11-20; first release 2026-04-13 | https://apps.apple.com/th/app/assetflows/id6760455098 ; https://portfolio.assetflows.cloud/ (page text only returned the title) |
| Moneypod (Wanmai Apps / M. Tathuwan) | Cute Thai-popular expense tracker, AI receipt scan, budgets | Listing words: "Installment payment tracking", "Debt Estimate"; no schedule detail | "Net worth tracking" (types not described) | Daily reminders | None seen | Thai on Play; iOS declares English | Free; Premium monthly 49, yearly 249, lifetime 799 (listing items, other add-ons 29-429) | iOS 4.89 from 10,804; Play 4.91 from 11,794, 100K+ | iOS 2026-11-20; Play 2026-11-06 | https://moneypod.app/ ; https://apps.apple.com/th/app/expense-tracker-cute-moneypod/id6456409633 |
| Piggipo GO (Neversitup) | Credit-card tracker plus income/expense | Card interest and minimum-payment calculation, instalment ("ผ่อนชำระ") calculation; no general loans | - | Payment-due and statement-day notifications | Support contact only (LINE @Piggipo); not a feature | Thai/English | Free; Sub 59/mo, 599/yr | iOS 3.87 from 254; Play 3.48 from 234, 10K+ | iOS 2026-08-31; Play 2026-08-11 | https://apps.apple.com/th/app/piggipo-go-credit-card-tracker/id1480821477 ; https://play.google.com/store/apps/details?id=com.neversitup.piggipogo |
| Money Lover (Finsify) | Global tracker, budgets, bills | "Debt management", bill reminders in listing; no schedule detail | Not itemised | Bill reminders | None | Thai among 29 languages | Free; Premium lifetime 699, upgrade 399, linked-wallet (bank sync incl. "Banks in Thailand") 79-89/mo | iOS 4.75 from 15,825; Play 4.77 from 208,324, 5M+ | iOS 2026-11-17 | https://apps.apple.com/th/app/money-lover-money-manager/id486312413 ; https://play.google.com/store/apps/details?id=com.bookmark.money |
| Wallet by BudgetBakers | Global tracker with bank sync, debt, bills, stocks/ETFs, net worth | "manage debt" in listing; no schedule detail | Accounts plus investments combined for net worth | Bill tracking | None | Thai among ~47 languages | Free; Premium 199 / 499 / 999 / lifetime 999, 3-year 1,990 | iOS 4.66 from 823; Play 4.78 from 389,338, 10M+ | iOS 2026-11-18 | https://apps.apple.com/th/app/wallet-budget-money-manager/id1032467659 ; https://play.google.com/store/apps/details?id=com.droid4you.application.wallet |
| Spendee (Cleevio) | Budget tracker; bank, PayPal and crypto-wallet sync | - | Bank, e-wallet, crypto-wallet sync | Alerts on budget limit | None | Thai among 27 languages | Free; Premium monthly 149, other 100-3,990 | iOS 4.79 from 13,799; Play 4.53 from 64,296, 1M+ | iOS 2026-11-14 | https://apps.apple.com/th/app/money-tracker-spendee/id635861140 |
| Monefy | Fast expense entry | - (Play text: debt keyword only in sidebar/related text, not confirmed) | - | - | None | iOS listing has no Thai; Play page is Thai-localised | Free; Premium 1,320-1,990 | iOS 4.71 from 390; Play 4.39 from 194,829, 10M+ | iOS 2026-11-12 | https://apps.apple.com/th/app/monefy-money-tracker/id1212024409 |
| Money Manager (Realbyte) | Double-entry style tracker | Mentions loans via recurring transfers | Asset accounts | Repeat entries | None | Thai among 19 languages | Free (IAP not scraped) | iOS 4.88 from 8,595; Play 4.85 from 469,906 | iOS 2026-11-20 | https://apps.apple.com/th/app/money-manager-expense-budget/id560481810 |
| Lumpsum (EFIN Wealthfit / Online Asset) | Thai planning app: planning, tax, funds, insurance, loan-product referral | Debt-solving "advice" claimed; no schedule | Not itemised in listing | - | None | Thai text; iOS declares English | Free | iOS 4.18 from 159; Play 4.24 from 210, 10K+ | iOS 2025-08-06; Play 2025-09-12 | https://apps.apple.com/th/app/lumpsum/id1458064062 |
| MAKE by KBank / K PLUS (KASIKORNBANK) | Bank app with expense summary, custom categories, goal pockets, fixed-cost bill pockets | Own bank loans visible/payable in K PLUS; no cross-lender payoff tool seen | Own-bank accounts only | MAKE: scheduled bill pockets; K PLUS: transfers schedule | K PLUS has LINE-related services (not verified) | Thai | Free | MAKE iOS 4.90 from 309,735; K PLUS iOS 3.70 from 34,860 | MAKE iOS 2026-11-12; K PLUS 2026-11-11 | https://apps.apple.com/th/app/id361170631 ; https://apps.apple.com/th/app/make-by-kbank/id1496532821 ; https://support.linebk.com/en/personal/Digital-banking/kplus/help/Pages/expense-summary.html |
| SCB EASY, Krungthai NEXT, ttb touch, krungsri (KMA), GSB MyMo, Bualuang | Banking apps | Own-bank loans only; no budgeting/expense summary found in their store text | Own accounts | Krungthai NEXT: scheduled transactions up to 24 months and payment-reminder banner | SCB Just4U (via SCB Connect on LINE) ends 2026-01-10 | Thai/English | Free | iOS ratings 2.78-4.03 (SCB EASY 3.41 from 25,960; Krungthai NEXT 3.42 from 53,799; ttb 3.62 from 27,808; krungsri 2.84 from 13,298; MyMo 2.78 from 21,125; Bualuang 4.03 from 37,431) | all updated Oct-Nov 2026 | https://www.scb.co.th/th/personal-banking/digital-banking/scb-easy/news/just4u ; store ids 568388474, 436753378, 884079963, 571873195, 987047466, 660238716 |
| Kept (Krungsri) | Savings-jar app | - | - | - | None | Thai | Free | iOS 4.20 from 3,600; Play 4.30 from 11,334, 1M+ | iOS 2026-10-19 | https://apps.apple.com/th/app/kept-saving-with-your-style/id1473826356 |
| Finnomena, Dime! (KKP), Settrade Streaming, Jitta / Jitta Wealth | Investment platforms / brokers / stock analysis | Not loan tools | Own-platform holdings only (Dime! "Analytics" covers its own portfolio; Streaming "track your portfolio" at the broker) | - | None | Thai | Free | Finnomena iOS 4.67 from 24,095; Dime! 3.94 from 4,149; Streaming (new app) 3.95 from 43; Jitta 4.68 from 267 | Oct-Nov 2026 | https://apps.apple.com/th/app/id1182358897 ; https://apps.apple.com/th/app/id1612798502 ; https://apps.apple.com/th/app/id6770574709 |
| Debit & Credit (Ivan Pavlov Pty) | Desktop-style ledger with net-wealth forecasting | Scheduled transactions | Accounts | Scheduled reminders | None | No Thai (11 languages listed, Thai not among them) | Free; Premium 399/year | iOS 4.9 from 16 | v7.9.2 | https://apps.apple.com/th/app/debit-credit/id882637543 |

Notes on rows:
- All figures in the table are [V] from the stores unless a cell says otherwise. Store data are snapshots of 2026-11-21.
- "None seen" for LINE means the store and web pages I read do not advertise a LINE channel. It is not proof.
- Finnomena asset tracking: the TH iOS listing describes buying funds, managed portfolios and bonds; it does not describe tracking outside assets [V]. A web search found nothing on cross-institution portfolio tracking [S]. I could not open the in-app portfolio screen. UNVERIFIED whether Finnomena can import outside holdings.
- Ookbee-style finance apps: I found no Thai finance tracker from that kind of publisher. UNVERIFIED (absence from my searches only).
- Convest (named in `03-market-business.md`) did not come up in the TH App Store name search, so I did not re-verify it. Keep its existing [S].

### 2.1 Free tools and calculators (loan arithmetic)

| Tool | What it does | Source and tag |
|---|---|---|
| Bank of Thailand "โปรแกรมแก้หนี้ DIY" | Enter each loan (balance, rate, instalment), rough income and expenses, debt priority; shows total monthly debt payment, surplus or shortfall, options per debt with estimated total interest, a revised payment projection and document templates; uses reducing-balance interest; stores data anonymously. Page does not describe extra-payment or early-payoff scenarios and shows no schedule. Page footer shows 2566 (2023), no last-updated date | https://www.bot.or.th/th/satang-story/financial-tools/debt-diy.html [V, fetch summary] |
| "คำนวณค่างวดเงินกู้ง่ายๆ" (Saran S) | Computes the instalment and, if you pay more per month, interest saved and months saved, with a table of interest, principal and remaining principal per month. Free on Google Play, 4.44 from 138 ratings, updated 2026-08-10 | https://play.google.com/store/apps/details?id=com.saranomy.loan [V] |
| "การเงินง่ายๆ" (Saran S) | Bundle: home and car instalments (flat-to-effective rate conversion, tables), loan, retirement, provident fund, compound interest. 528 ratings, 4.94, last update 2024-07-14 | https://play.google.com/store/apps/details?id=com.saranomy.easyfinance [V] |
| Retention/refinance calculator (Saran S) | Home-loan refinance savings after year 3 | https://play.google.com/store/apps/details?id=com.saranomy.easyrefinance [V] |
| GHB ALL GEN (Government Housing Bank) | Apply for and pay GHB loans (scheduled or extra payment); not a tracker | https://play.google.com/store/apps/details?id=th.co.ghbank.allgen [V] |
| Global debt-payoff apps with Thai storefront listing (e.g., TinyDebt, Debt Payoff Pro, Debt Tracker) | English or multi-language, tiny rating counts (TinyDebt 1 rating) | App Store listings found by search; not opened in depth. UNVERIFIED for Thai usability |
| Thai-language Notion/Excel templates | The Thai-localised Notion gallery pages exist; a Gumroad "Notion Finance and Budget Planner" has assets/liabilities/net worth but is English and paid. I found no verified free Thai net-worth spreadsheet | https://www.notion.com/th/templates/collections/top-10-free-personal-finance-templates-in-notion [S]; https://gumroad.com/a/755535571/uewlx [S] |

I did not find a loan tool from the SEC or the Office of the Consumer Protection Board in the search results [S]; only BOT. Not an exhaustive crawl of those sites.

### 2.2 LINE channels

- No LINE-native personal balance-sheet tracker found. Brand Inside's 2024 list and Thai Rath's 2025 list of expense apps contain no LINE-based tracker [V] https://brandinside.asia/?p=172886 (published 2024-04-13); https://www.thairath.co.th/money/personal_finance/finance_banking/2864056 (published 2025-06-12).
- Paypers (LINE OA): expense and receipt bookkeeping for small businesses and freelancers; plans Free (8 receipts/month), 189, 299, 1,259. No debt or asset feature on the page [V, fetch summary] https://page.line.me/paypers/showcase/1891738196161778.
- ขุนทอง KhunThong (LINE OA linked to KBank): splitting and collecting shared bills; not a personal ledger [V, fetch summary] https://page.line.me/340lhckg.
- SCB Just4U: automatic monthly summary of money in and out in SCB EASY and SCB Connect (LINE). SCB's page says the service ends 2026-01-10 and a new format "with broader display and analysis" will follow; no launch date or name given [V, fetch summary] https://www.scb.co.th/th/personal-banking/digital-banking/scb-easy/news/just4u. I could not confirm whether the replacement has launched.
- PEAK (accounting software) has a LINE OA to save business expenses [S] https://intercom.help/peak/en/articles/5947432. Business, not personal.
- Hobby GitHub/n8n LINE-bot expense recorders exist [S]; they are DIY, not products.
- Implication [I]: "inside LINE" is open space for a personal tracker, but it is a channel, not a moat, and the scope doc does not currently make LINE a feature.

## 3. Feature-vs-competitor matrix for scope features 1-11

Scope numbering has two items numbered 1 (Asset list at P0, Slip OCR at P2). I split them as 1a and 1b.

Key: Y = advertised on a primary page I read; p = partial or ambiguous; blank = not advertised on pages read (not proof of absence). Competitor columns are the closest rivals only.

| # | Our feature | MeowJot | SET Happy Money | Wallet Story | MeeTang | AssetFlows | Moneypod | Piggipo GO | Global (Money Lover, Wallet BB) | Banks (K PLUS/MAKE) | BOT DIY / free calculators |
|---|---|---|---|---|---|---|---|---|---|---|---|
| 1a Asset list (multi-type; manual; market prices; CSV) | | Y (assets categorised, manual) | Y (land/house/car, stocks/funds; CSV import of statements) | p (gold, Thai funds with SEC NAV) | Y (stocks, funds, bonds, crypto/gold "Other"; manual NAV) | p ("net worth tracking") | | p (Wallet BB: investments in net worth) | own accounts only | |
| 1b Slip OCR / import (P2) | Y (slips from many apps; statements paid) | | Y | Y (OCR local + AI) | | p (receipt scan) | | | | |
| 2 Incomes | p (income in summaries, recurring paid) | Y | Y (payslip item by item) | Y | p | Y | Y | Y | Y (summary) | |
| 3 Expenses | Y | Y | Y | Y | Y | Y | Y | Y | Y | |
| 4 Due in X days / month-end | | p (bill reminders) | Y (card statement and due-date reminders; to-pay list) | Y (card due dates, bills, instalments) | p (reminders) | | Y | Y (bill reminders) | p (Krungthai NEXT scheduled payments) | |
| 5 Next-month income estimate (rule-based) | | | p (cash-flow forecast) | p (monthly remaining) | | | | | | |
| 6 Not-yet-owner checklist (financed assets) | none found on any page read | | | | | | | | | |
| 7 Dashboard | Y | Y | Y | Y | Y | Y | Y | Y | Y | p (BOT: debts vs income summary) |
| 8 Principal/interest split (amortization) | | | p (installment plans; split not stated) | p (instalment schedule; split not stated) | Y (v1.4.3/1.4.4) | p ("Debt Estimate") | p (card interest/minimum) | | | Y in calculators (Saran S tables); p BOT DIY (reducing balance, total interest, no schedule) |
| 9 Descriptive AI explanation in Thai from own numbers | | p (rule-based "คำแนะนำ", not LLM) | p (AI coach and ask-in-plain-language; AI language not stated) | Y (Thai AI assistant answering from user data, consent-gated, disclaimer) | | p (AI receipt scan only) | | | | |
| 10 Debt health indicators (DSR, DTI, cash margin) | | p (financial health rating includes debt repayment; ratio names not stated) | p (Financial Health score) | | p (Financial Health, integrates loans) | | | | | p (BOT DIY: monthly debt payment vs income surplus/shortfall) |
| 11 What-if payoff simulator | | | | | Y (v1.4.3) | | p (calculation of paying minimum) | | | Y (Saran S: extra payment, months and interest saved) |

How to read it: no row 1-11 is empty of competitors except row 6. Rows 8, 10 and 11 (our P0/P1 core) are thinly covered by trackers and are covered by free calculators. Row 9 is advertised by MeeTang and partially by Wallet Story. Row 6 is the only feature with zero hits in my search [I: absence only].

Evidence for the key cells:
- AssetFlows loan/amortization/simulator: version history on the App Store page [V] https://apps.apple.com/th/app/assetflows/id6760455098 (v1.4.3 "Loan and mortgage accounts with amortization, a payoff simulator, and Financial Health integration"; v1.4.4 "Loans & Liabilities ... payoff estimates and amortization schedules"; v1.4.5 "Other asset category for crypto, gold, commodities").
- MeeTang schedule and Thai AI: iOS release note "หนี้ผ่อน: ตั้งวันงวดแรกครบกำหนด ให้ตารางผ่อนตรงกับงวดที่จ่ายไปก่อนเริ่มใช้แอป"; description: "น้องตังค์ ผู้ช่วย AI ... ถามเรื่องเงินเป็นภาษาไทย ... ตอบจากข้อมูลของคุณ (เปิดใช้เมื่อคุณยินยอมเท่านั้น) ... ไม่ใช่คำแนะนำทางการเงิน" [V] https://apps.apple.com/th/app/id6818940531 and Play listing com.mamat1123.meetang.
- Wallet Story: description lines "Debts and money owed to you, including installment plans", "statement and due-date reminders", "Track land, a house or a car for your true net worth", "Cash-flow forecast ... warning before it runs low", "A Financial Health score", "Ask questions about your money in plain language" [V] https://apps.apple.com/th/app/wallet-story-expense-manager/id1482496559.
- SET Happy Money: Play/App Store text "บันทึกรายการสินทรัพย์และหนี้สินปัจจุบัน ... แยกหมวดหมู่หนี้สิน เพื่อวิเคราะห์และจัดลำดับในการจัดการหนี้ ... เปรียบเทียบฐานะการเงินในอดีตและปัจจุบัน ... วิเคราะห์สุขภาพการเงิน ... สรุปคำแนะนำ ... ตั้งเตือนบิลค่าใช้จ่าย" [V] https://play.google.com/store/apps/details?id=th.or.set.happymoney ; https://www.set.or.th/happymoney/app.html [V, fetch summary].
- BOT DIY: see 2.1.

## 4. Verdict on the "no competitor" claim

Verdict: not proven; in the strong form ("no Thai app combines all assets, liabilities and cash flow", `docs/research/...direction §2`, tagged [I]) it is contradicted by evidence. SET Happy Money has been on the iOS store since 2019-11-12 [V] and its current listing advertises assets, liabilities, income and expenses with a financial-health analysis [V]; I did not check when each feature was added, and Wallet Story, AssetFlows and MeeTang each cover large parts of it [V].

What the check does and does not show:
- Done: Thai and English web searches, direct queries on App Store TH and Google Play TH, official-site reads, bank-app store text. This satisfies the CLAUDE.md requirement to do the check; it does not make the gap "proven".
- Not done: installing and using the apps, reading in-app screens, paid-tier features, bank apps' in-app menus (K PLUS pages returned 403 to my fetcher), Krungthai/ttb/GSB/Krungsri budgeting screens, and any exhaustive crawl. Store text omits things that exist in-app.

Safe wording the team may use (each line is limited to what was checked):
1. "As of 21 November 2026, a Thai-language and app-store search (Apple App Store TH, Google Play TH, and web) found no single app that advertises, with a Thai interface, all of: multi-type asset list, loan principal/interest schedule, due-date view, debt-health ratios and a what-if payoff simulator. This is based on public listings only, not on testing the apps."
2. "Existing apps cover parts of this: SET Happy Money (assets, liabilities, net worth), Wallet Story (debts, instalments, due dates, net worth), AssetFlows (loan amortization and payoff simulator, English only), MeeTang (Thai AI assistant, instalment schedule), and free loan calculators including a Bank of Thailand tool."
3. "We position on integration and on descriptive, explainable output, not on being the only one."

Wording to avoid: "no competitor", "first in Thailand", "unique", "nothing like it exists", "the market is empty", and "gap proven". Keep "gap" labelled [I] until a user-tested comparison exists.

Also relevant to open decisions in `_decisions-market.md`:
- D-M6: cross-bank slip capture is not an edge (MeowJot, Wallet Story, MeeTang all advertise it) [V]. This check supports options A or C there (drop cross-bank as an edge; rebuild the edge around balance-sheet/loan features), but note that loan features also have competitors, so even option C needs the narrowed wording above.
- D-M7: I now hold [V] with URL and access date for the MeowJot pages (website, App Store IAP list, Play page), so Option A of D-M7 is satisfiable for these pages. The website pricing page is a fetch-tool summary; re-open it in a browser before quoting figures in outward material.

## 5. Pricing and willingness-to-pay evidence

### 5.1 MeowJot price status (feeds D-M3 and D-M4)
- App Store IAP list on the TH listing: MeowSilver 79 and 499, MeowGold 149 and 799 (four items, no label for monthly or annual in the list text I scraped, but the pricing page gives monthly and annual) [V] https://apps.apple.com/th/app/meowjot-auto-track-expenses/id6457365306.
- MeowJot pricing page (fetch summary): Free tier (20 bank apps, category summaries, 1 month of history); Silver "42 THB/month" in the comparison section, annual 499; Gold "67 THB/month", annual 799; 14-day free trial on Silver and Gold; the page header contained "7942฿/เดือน" and "14967฿/เดือน", which the summary read as a struck-through 79 beside 42 and a struck-through 149 beside 67 [V as to page text; the strikethrough reading is [I]] https://www.meowjot.com/pricing.
- Reading [I]: 79 and 149 are list monthly prices and 42 and 67 are the effective monthly price on annual billing (499/12 = 41.6; 799/12 = 66.6). That would mean 499/799 are annual prices, not launch promotions, which would contradict the UNVERIFIED press claim in D-M4. This is my inference from arithmetic and needs a browser check. Do not upgrade.

### 5.2 Other Thai-storefront price points (App Store TH, all [V])

| App | Items seen (THB) |
|---|---|
| Piggipo GO | Subscription monthly 59; yearly 599; category packs 35 and 179 |
| Wallet Story | Monthly subscription 49; yearly subscription 499; PRO 499; account books and icon packs 249 |
| Moneypod | Premium monthly 49; yearly 249; lifetime 799; other items 29, 35, 199, 429 |
| AssetFlows | Pro 99 per month; 499 per 6 months; 899 per year |
| Money Lover | Premium lifetime 699; upgrade 399; bank-link (Thailand banks) subscription 79 to 89 per month |
| Wallet BB | Premium 199 / 499 / 999; lifetime 999; 3-year 1,990 |
| Spendee | Premium monthly 149; other tiers 100 to 3,990 |
| Monefy | Premium 1,320 to 1,990 |
| Debit & Credit | Premium 399 per year |
| Paypers (LINE, business) | 189, 299, 1,259 per plan (currency not stated on page; likely THB [I]) |

### 5.3 What this says about the THB 60-80 per month hypothesis
- The hypothesis sits inside the observed Thai-storefront monthly range of 49 to 99 [V]. It is at MeowJot Silver's list price (79) and above Wallet Story and Moneypod (49).
- Free substitutes anchor price down: SET Happy Money (free, Thai, assets and liabilities), BOT DIY (free), free Thai loan calculators, and free tiers of Wallet Story and MeowJot [V].
- Annual pricing is the common structure (499 to 899 per year across MeowJot, Piggipo, Wallet Story, AssetFlows) [V], so a monthly 60-80 headline may understate what a paying user is actually offered.
- Demand evidence: none found. No Thai survey of willingness to pay for a personal-finance app, and no published paying-user counts. A web search returned a 2022 Mastercard figure (81% of Thai consumers used digital tools for a financial task) and a Capco figure on willingness to share data [S]; neither measures paying for an app. Rating counts (e.g., MeowJot 11,378 on iOS) show usage popularity, not payment [I].
- Conclusion [I]: price points are supply-side evidence only. Willingness to pay for a balance-sheet tracker is UNVERIFIED and should be tested in the planned 10 to 15 user interviews.

## 6. Differentiators that survive (all [I] unless stated)

Compared against what competitors advertise:
1. Integrated engine, not a feature: amortization schedule (principal/interest per instalment) feeding due dates, debt-health ratios and the payoff simulator in one balance sheet, with a Thai UI. Parts exist separately (AssetFlows has the engine in English; calculators have the arithmetic; Wallet Story has debts and net worth in Thai). Surviving claim: the bundle, with Thai-first treatment of Thai loan types (e.g., flat-rate to effective-rate conversion, which Saran S's calculator already offers for cars).
2. Debt-health ratios stated in plain terms (debt service ratio, debt-to-income, monthly cash margin). Competitors advertise a "financial health" score or "ฟิต/ฟุบ" without naming the ratios [V as to listing text]. Defensible only as "ratios shown with their inputs".
3. Descriptive-only AI with masking and an audit log. MeeTang already shows a similar disclaimer and consent gate [V]; SET Happy Money and Lumpsum give explicit "คำแนะนำ"/advice [V], which our scope forbids. The stance is a real contrast with those two, but it is a compliance design choice, not a user-visible edge, and not unique against MeeTang.
4. Not-yet-owner checklist for financed assets: no hits in any listing or page read. Keep as a candidate differentiator, but it is low-evidence either way (absence only) and needs user validation.
5. Not surviving as differentiators: cross-bank slip capture (MeowJot, Wallet Story, MeeTang); Thai AI Q&A on own data (MeeTang); credit-card due dates (Piggipo, Wallet Story, MeeTang); asset types including gold and funds (MeeTang, AssetFlows); net worth (SET Happy Money, Wallet Story, Moneypod).

## 7. Threats

1. Regulatory data access (Bank of Thailand "Your Data"). The BOT announcement says the regulation was enacted on 30 October 2025; data covers deposit, loan, and payment (account, e-money, credit card) information; sharing starts with individual deposit data, with providers expected to enable it gradually by the end of 2026, then phased expansion to other data in 2027 to 2028 [V, fetch summary] https://www.bot.or.th/en/news-and-media/news/news-20251110.html. A search summary says personal deposit data by 31 December 2026 and expansion to loans, cards and payments in 2027 to 2028 [S] https://www.thansettakij.com/economy/643632. Consequences:
   - Banks and licensed providers can offer aggregated views of balances and loans from other institutions to their own customers [I]. This attacks the manual-entry burden, which is the biggest friction in a balance-sheet app.
   - The BOT page says data moves between financial service providers under BOT supervision and does not say whether non-supervised third-party apps (like a student project) may receive it [V as to what the page does not say]. Our app probably cannot use it in the build window [I]. The scope doc already defers bank API to a later upgrade.
   - A separate draft data-sharing law was in consultation until 2026-08-31 [S, search summary].
2. Bank spend-analysis already inside banking apps: K PLUS "Expense Summary" (own K PLUS transactions only, per KBank support page) [V]; MAKE by KBank Expense Summary, custom categories, fixed-cost bill pockets (house, car, insurance, loan) [V]; SCB is retiring Just4U and promising a broader analysis service [V, fetch summary]. If banks add cross-bank views after Your Data, they gain a ready channel to tens of millions of installs (K PLUS iOS rating count 34,860 is only a floor on users [I]).
3. Fast-moving direct rivals. Wallet Story shipped v18.5.0 on 2026-11-16 and has 28,490 iOS ratings with a Thai UI and an AI coach [V]. MeeTang, a one-person app, shipped from first release (2026-10-06) to a Thai AI assistant plus instalment schedule inside six weeks [V]. AssetFlows went from first release (2026-04-13) to loans with amortization and a payoff simulator by the version dated "10 Sept" on its page (about five months; the page omits years, so 2026 is my inference [I]). A student team starting code in late 2026 would be entering against products that iterate monthly.
4. MeowJot (KBTG / KASIKORN X): 11,378 iOS ratings and 4.87, 500K+ Play installs, a bank-backed distribution channel [V]. It does not advertise assets or loans [V as to its pages]; it could add them [I].
5. SET Happy Money: free, official, already "assets + liabilities + health" [V]. Its last update was 2025-12-31 (iOS), so it looks slow-moving [I]; but it removes any "nobody offers this" story and makes a free anchor.
6. General-purpose AI chat assistants that can read a pasted statement or loan terms and explain it. I did not research this threat; a search result titled "OpenAI has bought AI personal finance startup Hiro" (TechCrunch, 2026-04-13) came up [S, title only, not opened]. UNVERIFIED for Thailand.

## 8. Gaps and what I could not verify

- No app was installed or used. All feature claims are advertised claims. Quality of Thai (wording, number formats, Buddhist-era dates, Thai slip OCR accuracy) was not assessed. Language "declared" on iOS listings is metadata and often differs from the real UI (MeowJot declares English only but its site and Play text are Thai).
- K PLUS website pages returned 403 to my fetcher; its budgeting features are known only from the iOS/Play text and a KBank support page [V] and a search summary of "My Spending"/"My Budget" [S]. Krungthai NEXT, ttb touch, GSB MyMo, krungsri KMA, Bualuang: store text shows no budgeting or expense summary; in-app menus not checked, so "not found" is weak.
- Finnomena: portfolio tracking of outside assets not confirmed either way. Jitta, Dime!, Settrade Streaming: listing text only.
- Google Play numbers are from the page as served to me; whether rating counts are TH-only is unknown. Install buckets are Play's. No download counts for iOS exist.
- MeowJot website price display (42 / 67 with struck-through 79 / 149) is a fetch-tool summary and the strikethrough interpretation is mine. Re-open in a browser.
- Wallet Story: "PRO 499" and "Yearly Subscription 499" appear as separate items on the iOS page; the description says PRO is a one-time purchase. I did not test purchase flows. MeeTang's price labels not captured.
- AssetFlows web page returned only its title; developer's own site not confirmed. Version dates on the App Store page omit the year; years inferred from the 1.0 date of 13 April 2026 and the "8 hr ago"-style relative date.
- Ookbee-style finance apps, any Thai Notion/Excel net-worth template that is free and Thai, SEC and OCPB calculators: not found; UNVERIFIED absence.
- Convest from `03-market-business.md` was not re-verified; keep its [S].
- Thai Rath and Brand Inside lists are editorial roundups; their feature lines are [S], used only to find leads. The 2025 Thai Rath list names Lumpsum "asset/liability tracking" and "Piggipo GO ... consolidates multiple credit cards" [S]; I could not confirm Lumpsum's asset/liability tracking on its own store text [V says only planning, expense log, tax, funds, insurance, loan referral].
- No survey of Thai willingness to pay was found. No paying-user counts for any competitor.
- LINE: I could not enumerate LINE Official Accounts (no public directory). The absence of a personal-finance tracker in LINE is from searches only.
- Legal check of whether "summary" outputs fall outside investment advice is out of scope here (already an open item in the scope doc). Note that SET Happy Money and Lumpsum use "advice" wording in their own listings [V].

## 9. Suggested follow-ups (not actions I took)
1. 90-minute hands-on trial of Wallet Story, MeeTang, AssetFlows and SET Happy Money with synthetic data: record whether each splits principal and interest, shows payoff dates, and what the Thai text looks like. This would turn "advertised" into [V-tested].
2. Re-open meowjot.com/pricing in a browser and log the exact prices and labels.
3. Update `docs/consolidated/03-market-business.md` section 3.2 with the competitors above once the team agrees; the file's existing list (Convest, Finnomena, Settrade, MeowJot) misses SET Happy Money, Wallet Story, AssetFlows, MeeTang, Moneypod and Piggipo GO.
4. Add a WTP question block to the user interviews.
