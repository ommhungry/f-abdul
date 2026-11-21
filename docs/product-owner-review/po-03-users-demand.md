# PO-03: Users, demand and evidence base

Author: user researcher, product-owner review team. Date: 2026-11-21. Repo files were read only, none modified.

## 0. Reading rules

Tags: `[V]` = I opened the page this session and the number was on it. Limit: WebFetch returns a small-model extract, not raw HTML, so [V] means "seen in a fetched extract of the primary or publisher page". `[S]` = secondary: a news article, a search-result summary I did not open, or a repeat of someone else's figure. `[I]` = my inference or arithmetic. `UNVERIFIED` = not seen, or the sources conflict. No tag was upgraded. Search-result snippets (not opened) are tagged `[S]` even when the publisher is official. "Older than 2024" is flagged; none of the figures below are older than 2024 unless marked OLD.

Honest headline: I found no primary Thai data on (a) how many people track expenses, (b) willingness to pay for a finance app, (c) overlap between indebted salaried workers and retail investors, or (d) consumer attitudes to linking bank data after 2022. These stay gaps. The interview and survey in sections 4 to 5 exist to close them.

Repo context read: scope spec (features 1-11, open item "decide target user: salaried with loans or investors"), CLAUDE.md rules, and consolidated 01 and 03 (03 says no personas exist and no interviews, WTP or retention evidence exists; MeowJot is a live competitor).

---

## 1. Size and shape of the problem

### 1.1 Household debt, macro

| Fact | Value | Source (URL, date) | Tag |
|---|---|---|---|
| Household debt / GDP, Q1 2026 | 85.9%, lowest in 6 years. Total household debt THB 16.41 trillion, +0.5% YoY; nominal GDP +2.9% so the ratio fell | SCB EIC via Thaipublica, 16 Jul 2026. https://thaipublica.org/2026/07/scb-eic-on-household-debt-q1-2026/ | [V] (publisher page of an SCB EIC analysis; not the NESDC page) |
| Reason for the fall | SCB EIC: "harder to borrow", not "better off". The fall is partly nominal GDP growth and debt relief programs | same page; headline in search listing: https://thaipublica.org/2026/07/scb-eic-on-household-debt-q1-2026/ | [V] |
| Forecast end-2026 | 83.5-84.5% (SCB EIC) | same | [V] |
| Q2 2026 ratio | 85.2%, lowest in 25 quarters, THB 16.43 trillion, per SCB EIC. Not confirmed on an NESDC or BOT page. NESDC social report text may still carry the Q1 figure of 85.9% | Search summary only; listed pages: https://www.naewna.com/n/?p=117244 and https://www.thansettakij.com/economy/667253 (not opened) | [S] / UNVERIFIED for NESDC Q2 |
| Mix of Q1 2026 loan growth | Mortgage +1.7%, cards and BOT-regulated personal loans +1.0%, other personal consumer loans +4.8%, car and motorcycle hire purchase -7.5%, education loans -2.2%. Pawnshops +18.3%, savings cooperatives +5%. Bank household loans -2.1% for 8 straight quarters | same Thaipublica page | [V] |
| Q2 2025 ratio (for trend) | 86.8%, total THB 16.31 trillion (Nation, 24 Nov 2025) | https://www.nationthailand.com/news/general/40058692 | [V] (news article). Older than 2026 but within 2024-25 |

### 1.2 Per-household debt: two numbers that must not be mixed

| Fact | Value | Source | Tag |
|---|---|---|---|
| UTCC survey, average debt per household | THB 794,945 (+7.3% from 740,597 in 2025); 91.8% of households have debt (95.1% in 2025). n = 1,720, fieldwork 7-13 Sep 2026, reported 17 Sep 2026 | https://www.nationthailand.com/business/40071155 | [V] news report of a university poll. Not a census; sample 1,720, collection mode not stated. |
| Same survey, share of respondents borrowing by type | Credit cards 55.1%, personal consumption loans 45.8%, vehicle loans 41.9%, business loans 33.6%, housing loans 27.2%, Buy Now Pay Later 14.2% (6.2% a year earlier) | same | [V]. Multiple answers allowed (shares sum above 100%). BNPL doubling is the most interesting trend for a debt tracker. |
| NSO Household Socio-Economic Survey, average debt per household | THB 156,770 (2015), 197,255 (2566/2023), 153,038 (2568/2025); H1 2025 144,871. Sample for 2024 round: 57,600 households | NSO documents listed in search: https://nso.go.th/nsoweb/storage/survey_detail/2025/20250321102834_75752.pdf and https://www.nso.go.th/nsoweb/storage/survey_detail/2026/20260402154814_97144.pdf | [S] (read from a search summary, PDFs not opened; the 2025 vs 2568 label is as the summary gave it, possible Buddhist-year mix-up) |
| Why the two disagree | NSO averages over all households including those with no debt, and measures differently; UTCC is a smaller poll, skewed to urban, and likely to report households that borrow | | [I]. Use UTCC only as "indebted-household context", never as the national mean. |
| Share of households with each loan type (national) | Not found in a primary source. | | UNVERIFIED |

Implication [I]: nearly every Thai household carries some debt, and most carry more than one type (cards, personal, vehicle). A "which debts, when due" tool has a wide base. The hard question is whether they would enter the data.

### 1.3 Debt service burden and NPL

| Fact | Value | Source | Tag |
|---|---|---|---|
| Debt service share for low earners | Households up to THB 15,000/month spend 40.1% of income on debt service; 26.3% for all indebted households; 23.8% of indebted households negative cash margin | Quoted in repo direction doc L57 from Kasikorn Research via a secondary report; I did not re-find it | [S] (carried from repo, not re-verified) |
| Household NPL, Q2 2025 | THB 1.24 trillion (90+ days overdue, credit-bureau data), NPL ratio 9.11% of total credit, up from 8.78% in Q1 2025; seizures of low-priced homes +210% | https://www.nationthailand.com/news/general/40058692 | [V] news report quoting NESDC secretary-general; note this is a credit-bureau definition, higher than bank-reported NPL |
| Stage 2+3 consumer loans, Q1 2026 | 10.7% (from 11.1%); hire purchase Stage 2+3 15.0% (from 15.5%). SCB EIC: part of the decline is debt-relief programs, not stronger repayment capacity | https://thaipublica.org/2026/07/scb-eic-on-household-debt-q1-2026/ | [V] |
| Stage 2+3 is not the same as NPL | The article uses IFRS 9 stages, not NPL | | [I] |
| NPL trend direction | Q2 2025 rising; Q1 2026 Stage 2+3 falling for 3 straight quarters. Treat the two series as non-comparable. | | [I] |

### 1.4 Loan types and interest conventions users face

| Loan | Convention | Evidence | Tag |
|---|---|---|---|
| Car hire purchase | Flat rate (interest on the full original principal for the whole term, payments fixed). Law requires the effective rate to be shown too. For personal-use contracts from 3 Dec 2025 the effective rate ceiling is 10% (new car) and 15% (used car). Krungsri's own example: flat 1.98-5.25% = effective 3.81-9.80% | https://www.krungsri.com/th/krungsri-the-coach/loan/auto-loan/update-car-lease-agreement | [V] (a bank educational page, not the BOT notification itself). BOT notices dated 7 Oct 2026 effective 1 Nov 2026 for hire-purchase firms were reported in search: https://www.bot.or.th/content/dam/bot/fipcs/documents/FPG/2569/ThaiPDF/25690214.pdf (not opened) [S] |
| Used car and motorcycle (non-bank lender example) | Tidlor 2026 notice: used car/pickup flat 0.55-0.73% per month (about 11.97-15% effective), motorcycle flat 1.06-1.12% per month (about 22.9-23% effective) | https://www.tidlor.com/tidlor/media/interest-rates-and-fees/StandardInterest-8may26.pdf | [S] (search summary, PDF not opened; one lender, not a ceiling) |
| Mortgage | Floating, linked to bank reference rates (MRR, MLR), often with 1-3 year discounted or fixed-rate starting periods. | Not verified this session. | UNVERIFIED |
| Credit card | Revolving, minimum payment. Cap values (16%, 25% personal loan) were in my query, not in any result. | | UNVERIFIED |
| Student loan (Gor Kor Sor, กยศ.) | Interest 1% a year and penalty cut to 0.5% a year in the recalculation; recalculation done for 556,000 accounts and 430,000 accounts (accounts, not borrowers); about 100,000 defaulters of 4+ years to face lawsuits; repayment restructuring up to 15 years; payroll deduction extra THB 3,000 per account from April 2025 | Search summary of several news pages, e.g. https://www.thaipbs.or.th/news/content/505069 | [S] (not opened; date conflict 2568 vs 2569 noted by the searcher) |
| Informal debt | No figure found. Pawnshop lending +18.3% YoY (Q1 2026) is the nearest signal. | Thaipublica page above | [V] for pawnshop; informal share UNVERIFIED |

Product implication [I]: the amortization engine must support at least (a) flat-rate with an effective-rate display, (b) declining-balance fixed rate, (c) floating mortgage with rate-change dates, (d) revolving card with minimum payment, and (e) a generic "zero-interest installment/BNPL". Flat-rate conversion is the feature that distinguishes the product from a spreadsheet.

### 1.5 Retail investors and overlap

| Fact | Value | Source | Tag |
|---|---|---|---|
| Securities trading accounts | 3,853,530 cumulative at 28 Nov 2025 (Asia Plus Securities) | Search result only, from https://www.kaohooninternational.com/?p=573603 | [S] |
| SET statistics table | "Number of investors opened accounts" 4,553,911 and 4,596,426 for two unlabelled months; net count | https://www.set.or.th/en/market/statistics/market-statistics/trading-statistics (not opened) | [S]; definition differs from line above. UNVERIFIED which to use. |
| Mutual fund accounts | SEC open data page: 11,165,574 accounts, no as-of date on the page; an SEC strategic plan table shows 3.1 million for 2025. Conflict. | https://secopendata.sec.or.th/Investor/INV_MF and https://www.sec.or.th/EN/Documents/AboutSEC/strategicplan-2026-2028-en.pdf (403 on fetch) | UNVERIFIED |
| SEC strategic plan remark | Domestic investor participation has fallen; foreign share of trading value 53% | same strategic plan | [S] (search summary; fetch blocked) |
| Crypto accounts | Not found | | UNVERIFIED |
| Overlap: indebted salaried workers who also invest | No data. | | UNVERIFIED |
| KKP / Thairath Money reader poll: 30.27% have monthly debt service over 40% of income; 34.86% report no debt | n = 3,969 started, 2,423 completed, 1-15 Sep 2025, online readers, mostly Bangkok area | https://www.thairath.co.th/money/personal_finance/finance_banking/2886724 | [V] but self-selected finance-interested readers |

Reading [I]: account counts are accounts, not people, and one person can hold several. Even the larger number sits well below the roughly 56 million LINE users, so investors are a minority segment while debt holders are the mass market. Overlap must be asked directly in the screener (Q-S3 and Q-S4 below).

---

## 2. Behaviour

### 2.1 Tracking and planning

| Fact | Value | Source | Tag |
|---|---|---|---|
| Share who track expenses in Thailand (national) | Not found in a primary source. | | UNVERIFIED |
| Monthly budgeting (finance-reader poll) | Never 42.6%, regularly 29.27%, on and off 28.07% | Thairath Money x KKP poll, Sep 2025, https://www.thairath.co.th/money/personal_finance/finance_banking/2886724 | [V], self-selected sample |
| Know what they spend most on | Clearly 51.84%, roughly 32.65%, unsure 15.51% | same | [V] |
| Spent more than earned in last 3 months | Often 34.69%, sometimes 38.43%, never 26.89% | same | [V] |
| 3-6 months emergency fund | Have 35.27%, building 26.10%, none 38.63% | same | [V] |
| BOT: 77.3% of Thais have emergency savings under 6 months | Khaosod English 27 Feb 2026, blocked by 403 when fetched | https://www.khaosodenglish.com/tech/2026/02/27/most-thais-lack-six-month-emergency-savings-bot-says/ | [S] (headline and search summary only) |
| Digital payments | About 650 digital payments per person per year in 2024 (BOT, as quoted in an article) | Search summary of https://www.posttoday.com/smart-life/733620 | [S] |
| Reading [I] | Roughly four in ten finance-interested readers never budget, and about seven in ten sometimes overspend. These readers are the most engaged group, so the national picture is probably weaker, but direction is not provable. | | [I] |

### 2.2 Tools

| Fact | Value | Source | Tag |
|---|---|---|---|
| Tools in use | No usage-share data found for spreadsheets, notebooks, bank apps, MeowJot or others. Bank apps (K PLUS "My Spending") advertise automatic categories. MeowJot (KBTG, launched 2023) reads transfer slips; pricing THB 79 and 149 per month recorded in repo 03 | repo 03 L70-85; search summary https://www.marketingoops.com/?p=367046 | [S]; usage UNVERIFIED |
| Earlier survey in repo: about 52% say budgeting or spending analysis is missing from their main app | repo direction L58 | [S] carried, not re-verified |

### 2.3 Literacy

| Fact | Value | Source | Tag |
|---|---|---|---|
| BOT/NSO financial literacy survey, fieldwork 2022-2024 (published 15 Aug 2024) | Mean financial skills score 71.4%, above OECD mean; 87.5% have savings (down 2.7); only 2.7% save by investing; sample 12,402 households; every two years | https://www.nationthailand.com/news/general/40040603 | [V] news report. Budget/record-keeping percentages not on the page. |
| Same 71.4% attributed to Krungsri Research in another outlet (up from 67.4% in 2020; OECD 60.5%) | Attribution differs: BOT vs Krungsri | https://www.pattayamail.com/news/krungsri-survey-shows-rising-financial-literacy-among-thais-but-long-term-planning-still-a-challenge-530196 | [S]. UNVERIFIED who ran it. Do not quote OECD 60.5% until reconciled. |
| BOT 2024 report PDF (Mar 2026 file) | Downloaded but the extractor could not read it; contents unverified. A saved copy: /Users/jay/.claude/projects/-Users-jay-Desktop-Senior/7f165d63-f1fc-4879-9a7d-fd156f2f402f/tool-results/webfetch-1795242605470-eujkwh.pdf | https://www.bot.or.th/content/dam/bot/documents/th/research-and-publications/reports/financial-access-survey-of-thai-household/fl-fi-hh-survey-report-2024-th.pdf | UNVERIFIED. Next step: read the PDF locally for budgeting, record-keeping and compound-interest questions. |
| OECD/INFE 2023 (39 economies) | Thailand's row not seen | https://search.oecd.org/finance/oecd-infe-2023-international-survey-of-adult-financial-literacy-56003a32-en.htm | UNVERIFIED |

Reading [I]: a high mean score and low saving by investing show knowledge is not the gap. The gap is behaviour and visibility (what is due, what it costs). That favours a tracker with plain-Thai explanation over education content.

### 2.4 LINE and digital reach

| Fact | Value | Source | Tag |
|---|---|---|---|
| Internet users | 67.8 million, 94.7% of population, Oct 2025 | https://datareportal.com/reports/digital-2026-thailand (published 8 Nov 2025) | [V] |
| LINE | 56 million monthly active users, 78.2% of the population, 82.6% of internet users, flat vs Oct 2024 | same | [V]. DataReportal's own source for LINE is company-reported, not independently audited. |
| Digital 2027 edition (67.6m internet users at end 2026) | Reported by search; the date looks early for today (21 Nov 2026) | https://datareportal.com/reports/digital-2027-thailand | UNVERIFIED |
| LINE push quota (free plan 300/month) | repo 03 L119 | [S] carried |

### 2.5 Willingness to pay

No Thai data on paying for a finance app was found. Statista July 2025 subscription survey (n = 7,830) is paywalled. UOB ASEAN Consumer Sentiment Index for Thailand 47 in 2025 vs regional 54 (search summary, https://www.uobgroup.com/asean-insights/articles/acss-2025-thailand.page) [S]: suggests cost sensitivity, weak support. MeowJot pricing (THB 79 and 149 per month) is a price-point reference, not demand evidence. Conclusion: WTP is UNVERIFIED; test it through behaviour (section 4 concept test, plus a paid-pilot or pre-commit step).

### 2.6 Privacy and linking bank data

| Fact | Value | Source | Tag |
|---|---|---|---|
| BOT data-sharing regulation | Enacted 30 Oct 2025; providers to enable sharing by end-2026, starting with individual deposit account data, other categories 2027-2028 | https://www.bot.or.th/en/news-and-media/news/news-20251110.html | [V]. Consent mechanics not on the page. |
| SCBX thAI Consumer AI Adoption (n = 1,004 banked adults aged 20-60, urban, Aug 2025, published 27 Jan 2026) | 55% worry AI will violate privacy, 66% worry about misuse; over 80% used AI in some form; AI "still not trusted enough" for financial life | https://www.scbx.com/en/scbx-exclusive/thai-consumer-ai-adoption/ | [V]. Bank-sponsored; urban sample. |
| Willingness to share financial data | Capco Dec 2022 (n = 890, Bangkok area) strong propensity to share for tailored services; Accenture undated figures (94% for fast loan approval) | https://thethaiger.com/hot-news/technology/its-bank-of-the-future-for-thai-consumers ; https://www.bangkokpost.com/business/2458357/thais-willing-to-share-data-to-enable-better-online-banking | [S]. Capco and Bangkok Post are 2022: OLD. Accenture undated. Treat as weak. |

Reading [I]: privacy worry is real (more than half), and our stated design (masked LLM calls, no ID images, manual-first) fits. No evidence on consumer willingness to link bank accounts to a student-built app; assume low and test.

---

## 3. Personas (evidence-based, with gaps stated)

These are hypotheses assembled from the numbers above, not interview findings. Each must be confirmed or killed by the interviews in section 4.

### P-A "Installment Juggler" (salaried worker with several loans)

- Who: age 25-40, salaried or small-business employee in Bangkok or a provincial city; income plausibly THB 15,000-40,000/month; has a car or motorbike hire purchase, a credit card, a personal loan or a student loan, sometimes a mortgage or BNPL. [I]
- Evidence: 91.8% of surveyed households carry debt, with cards 55.1%, personal loans 45.8%, vehicle loans 41.9%, BNPL 14.2% (UTCC Sep 2026, [V]); debt service 40.1% of income for those earning up to THB 15,000 ([S]); 30.27% of finance-reader poll respondents spend over 40% of income on debt ([V]); BNPL doubled in a year ([V]).
- Jobs to be done: (1) "Tell me which payments are due before payday and what's left"; (2) "Show me what my loan really costs (flat vs effective) and when it ends"; (3) "Show me if paying extra changes the end date"; (4) "Keep every debt in one place so I do not miss one".
- Top pains: due dates scattered across apps, LINE notices, SMS and paper; flat-rate contracts that hide true cost; no single total; fear of missing a payment (credit record). [I]
- Key risk: the second-job problem (data entry), the bank app already shows cards and loans, MeowJot already reads slips.

### P-B "Slip Collector" (young salaried tracker, spending-focused)

- Who: age 22-35, high digital payment use, already shares slips in LINE; tries budgeting apps but quits. [I]
- Evidence: about 650 digital payments per person per year ([S]); 42.6% never budget, 28.07% budget on and off ([V], self-selected readers); MeowJot's existence suggests demand for slip-based capture (existence only).
- JTBD: "Know where my money went this month with no typing".
- Pains: manual entry fatigue, missing cash and card spending. This is MeowJot's and the banks' home ground; spending charts alone lose to the bank app (repo 03 L79).
- Weakest fit with our P0 scope; best treated as an entry channel, not the lead.

### P-C "Portfolio Plus Debt" (investor with mortgage or car loan)

- Who: age 30-50, owns SET stocks and/or mutual funds (perhaps crypto), also carries a mortgage or car loan; wants one net-worth view. [I]
- Evidence: 3.85 to 4.6 million securities accounts ([S], definitions conflict); only 2.7% of Thais save by investing ([V]); overlap with debtors UNVERIFIED.
- JTBD: "One view of assets minus debts, and does my extra cash cut debt more than it grows elsewhere" (the second half is a recommendation, which the product must not give; it can show both numbers, descriptively).
- Pains: assets spread across brokers and apps; cross-asset totals manual; many brokers already show portfolios.
- Risks: SEC advice-line exposure is highest here; market-price API effort; smallest segment.

### Recommendation (product owner's proposal, not a team decision)

Lead the MVP with P-A, the Installment Juggler, loan side first. Reasoning:

1. Size: indebted households are most Thai households ([V], UTCC); investors are a minority and the overlap is unknown.
2. Fit with the P0 and P1 scope: the amortization engine, due-in-X-days, debt health ratios and the payoff simulator all serve P-A directly. Features 10 and 11 are the differentiators and nobody in the repo's competitor scan appears to offer a flat-rate vs effective-rate payoff view (UNVERIFIED; needs the Thai app-store check CLAUDE.md requires).
3. Lower regulatory exposure: loan arithmetic is a calculation, while portfolio commentary approaches the advisory boundary.
4. Reachable for a student pilot: classmates, young staff and family members often hold a motorbike or car installment, a card and a student loan, so interview recruitment is easy. Caution: students and fresh graduates are a convenience sample and not the same as P-A.
5. Counter-risk [I]: manual entry of each loan is a one-time task (a loan changes rarely), which is an advantage over daily expense tracking; the data-entry-time metric tests this.
6. Keep P-B's capture (slip or manual quick add) only as optional input, and defer P-C's portfolio side to P2.

Kill criteria for the recommendation: if fewer than 6 of 12 interviewees have two or more active loans, or fewer than half say they have missed or nearly missed a due date in the past 12 months, reconsider.

---

## 4. Interview guide (10-15 users, 45 min)

### 4.1 Sampling

Target 12 interviews: 8 salaried or working adults with at least one active loan (P-A), 2 who mainly track spending (P-B), 2 with investments plus a loan (P-C). Stop at 15 or earlier if the last 3 interviews add no new pain or tool. Recruit through personal networks and a LINE post; offer a small thank-you (for example a THB 100 coffee voucher; sourced by the team, not from any dataset).

### 4.2 Screener (2 minutes, by LINE chat or form; store only a code, no names)

1. Age band: under 22 / 22-29 / 30-39 / 40-49 / 50+.
2. Work: salaried employee / civil servant / freelance or own business / student / other.
3. Which of these do you pay monthly now? (multi-select) car loan / motorbike loan / mortgage / credit card balance / personal loan / student loan (กยศ.) / BNPL or store installment / informal loan from family or friends / none. Need at least one for P-A.
4. Do you own investments (stocks, funds, crypto)? none / under 50,000 baht / 50,000 to 500,000 / over 500,000 baht (bands, not exact).
5. How do you track money today? nothing / notebook or notes / spreadsheet / bank app / a tracking app (which?) / LINE chat with myself.
6. Do you use LINE daily? yes / no.
7. Willing to join a 45-minute chat and to show how you track, using fake numbers only? yes / no.
Quota: 8 with Q3 not "none"; 2 with Q5 an app; 2 with Q4 not "none".

### 4.3 Consent script (read aloud; text version also sent)

"Thank you for helping. This is a student project at [university], building a tool that helps people see their debts and due dates. We are not selling anything and we are not a bank.
What we ask: 45 minutes of conversation. We will take notes. With your permission we record audio only, for our own note-taking, and delete the audio within 30 days.
What we do not want: please do not tell us your real name for the notes, real account numbers, ID numbers, phone numbers, lender names that identify you, or show a real slip or statement. If you want to show how you track, use made-up numbers or cover everything with sample data. If you slip and say something real, tell us and we will delete it.
Your answers go into a coded sheet (P01, P02). Only the three team members and our advisor see it. Results will be reported only as group summaries.
You can skip any question or stop at any time, and we will delete your notes if you ask.
Do you agree to take part? Do you agree to audio recording? (Yes or no for each; record the yes in the sheet as 'consent recorded, date'.)"
Note [I]: PDPA basis here is consent; the team should have the university ethics or advisor confirm the wording. Not legal advice.

### 4.4 Core questions (14), grouped; ask open, then probe "last time", "show me", "what happened next"

Context and debt today
1. Tell me about the last time a payment was due and you were not sure whether it was paid or when. What happened?
2. Which debts or installments do you have right now (types only, no amounts)? How many different places do you pay?
3. How do you know what is due this month? Walk me through the last due date you handled.
4. When did you last miss, or nearly miss, a due date? What did it cost you (fee, stress, credit)?
5. Do you know when each loan ends and how much interest you will pay in total? How do you know?
6. For a car or motorbike loan or a store installment, do you know the flat rate versus the real (effective) rate? (Listen for confusion; do not teach.)
Tools and behaviour
7. How do you keep track of money today? What did you use before and why did you stop?
8. What is the most annoying part of recording income and expenses? How much time per week does it take?
9. Have you tried a bank spending summary or a slip-reading app? What did you like, what did you drop?
Money visibility and trust
10. If one screen showed every debt, due date and what is left this month, what would you check first? What would worry you about it?
11. Would you let an app read your bank account? Under what conditions? What would you never share?
12. How do you feel about an AI that explains your numbers in Thai? What would it need to avoid saying?
Value and price
13. In the last 12 months, what have you paid for to manage money (app, course, premium plan)? What made it worth it, or not?
14. What would the tool have to do each month for you to pay for it? What amount would feel too expensive, and what amount would feel suspiciously cheap? (Van Westendorp style; record both numbers in THB.)

### 4.5 Observation task (8 minutes): how they track debt today

Say: "Please show me, using made-up numbers or by describing it, how you check what is due this week. Use your usual place (phone notes, bank app, spreadsheet, LINE). Think aloud. Cover any real figures."
Record: tools opened (count), places visited, time to produce a list of what is due in 7 days (stopwatch), items missing or wrong, workarounds, any reminder set. Note whether the participant could answer "total monthly debt payments" and "money left after them" and how long it took.

### 4.6 Concept test script (5 minutes; top 5 features)

Show paper or slide mock-ups of five cards, one at a time, with fake data. Read the line, then ask the same three questions.
Top five: (A) Due in 7 days or by month-end list; (B) Principal/interest split and loan end date with flat vs effective rate; (C) Debt health card (debt payments ÷ income, monthly cash margin); (D) What-if payoff ("pay 1,000 baht more per month"); (E) AI plain-Thai summary of your own numbers. (Reorder per participant to avoid order bias. Feature 7 dashboard appears as the frame, not a test item.)
Per card ask: (1) "In your own words, what does this show?" (comprehension: 0 wrong, 1 partly, 2 right); (2) "How useful is this for you, 1 to 5?"; (3) "Would you enter your own data to get this? What would stop you?" Then at the end: "Rank the five. If you could keep only one, which?" and "On a scale from 1 to 5, how much do you trust the AI summary card? What is one sentence it must never say?" (Safeguard check for buy/sell language.)

### 4.7 Scoring sheet (one row per participant; fill within 24 hours)

| Field | Scale / values |
|---|---|
| ID, date, interviewer, persona fit | P01.., date, A/B/C |
| Active loan types (count) | 0 to 6 |
| Missed or nearly missed due date in 12 months | 0/1 |
| Tools used today (list) | text |
| Time to list next-7-day dues (seconds) | number |
| Knows total monthly debt payment | 0/1 |
| Knows loan end date and total interest | 0/1/2 |
| Understands flat vs effective | 0/1/2 |
| Pain intensity (what interviewer judges, with quote) | 1-5 |
| Concept A-E usefulness | 1-5 each |
| Concept A-E comprehension | 0-2 each |
| Would enter own data (A-E) | Y/N/Maybe |
| Top 1 feature | A-E |
| Trust in AI summary | 1-5 |
| Privacy stance on linking bank | never / maybe with conditions / yes |
| WTP: acceptable range and "too cheap/too expensive" (THB) | numbers |
| Paid for a finance tool in 12 months | Y/N, what |
| Signal strength | 0 none / 1 weak / 2 strong, with the quote that justifies it |
| Key quote (no real PII) | text |
Synthesis rule: a need counts as "validated" only if at least 6 of 12 describe a concrete recent incident (not an opinion).

---

## 5. Survey draft (Thai and English)

Purpose: size the segments and test hypotheses from the interviews. Anonymous, online (Google Form, no email collection), 5 minutes, no personal identifiers. Distribute through LINE groups, classes and workplaces, with a clear label that the sample is convenience-based.

| # | English | ภาษาไทย | Type |
|---|---|---|---|
| 1 | Age band | ช่วงอายุ | single |
| 2 | Work type | ลักษณะงาน | single |
| 3 | Monthly personal income band | รายได้ต่อเดือนของคุณ (ช่วง) | single bands |
| 4 | Which of these debts do you pay now? | ตอนนี้คุณผ่อนหนี้ประเภทใดบ้าง | multi |
| 5 | How many separate due dates do you have per month? | แต่ละเดือนมีกำหนดจ่ายกี่รายการ | 0, 1-2, 3-4, 5+ |
| 6 | In the last 12 months, did you pay late or nearly miss a payment? | ใน 12 เดือนที่ผ่านมา คุณเคยจ่ายล่าช้าหรือเกือบลืมจ่ายหรือไม่ | never / once / 2+ |
| 7 | Do you know the total interest you will pay on your largest loan? | คุณรู้ดอกเบี้ยรวมตลอดสัญญาของหนี้ก้อนใหญ่สุดหรือไม่ | yes/roughly/no |
| 8 | Do you know the difference between flat rate and effective rate? | คุณทราบความต่างของดอกเบี้ยคงที่ (flat) กับดอกเบี้ยที่แท้จริง (effective) หรือไม่ | yes/a bit/no |
| 9 | How do you track money now? | ตอนนี้คุณติดตามเงินอย่างไร | multi |
| 10 | How often do you update records? | อัปเดตข้อมูลบ่อยแค่ไหน | daily/weekly/monthly/never |
| 11 | Do you own stocks, funds or crypto? | คุณมีหุ้น กองทุน หรือคริปโตหรือไม่ | none/yes (bands) |
| 12 | How useful: due list / loan end date and interest / debt health / payoff what-if / AI Thai summary | ประโยชน์ของ: รายการครบกำหนด / วันสิ้นสุดและดอกเบี้ย / สุขภาพหนี้ / จำลองโปะหนี้ / สรุปด้วย AI | 1-5 each |
| 13 | Would you enter your loans once (about 10 minutes) to see these? | คุณยอมกรอกข้อมูลหนี้ครั้งเดียว (ประมาณ 10 นาที) เพื่อดูสิ่งเหล่านี้หรือไม่ | 1-5 |
| 14 | Which input would you accept? | วิธีใส่ข้อมูลที่คุณยอมรับ | manual / slip photo / statement upload / link bank / none |
| 15 | How much do you trust an AI to explain your own numbers in Thai? | คุณเชื่อใจ AI ในการอธิบายตัวเลขการเงินของคุณเป็นภาษาไทยแค่ไหน | 1-5 |
| 16 | Biggest concern about such an app | ข้อกังวลสูงสุดต่อแอปแบบนี้ | privacy/accuracy/effort/cost/none |
| 17 | Would you use it in LINE or a separate app? | คุณอยากใช้ผ่าน LINE หรือแอปแยก | LINE/app/either |
| 18 | Price too expensive / fair / too cheap, per month, THB | ราคาต่อเดือน: แพงเกินไป / เหมาะสม / ถูกจนไม่น่าเชื่อ (บาท) | number |
| 19 | Would you join a 4-week test? (optional, leave a LINE ID only if you want; stored separately) | สนใจร่วมทดสอบ 4 สัปดาห์หรือไม่ | Y/N |
Intro text (Thai/English): anonymous, no real account or ID numbers, no names; aggregate use only; can quit anytime. Add a disclaimer that this is a student research survey.

### Sample-size reasoning

- Interviews: 10-15. The common rule that most themes appear by about 12 interviews (Guest, Bunce and Johnson 2006, recalled from memory, not re-fetched) [S]/UNVERIFIED. Three segments at 2-8 each is thin for the investor group, so P-C findings are directional only.
- Survey, proportion estimate: n = z²·p(1-p)/e² with z = 1.96 and p = 0.5 gives n = 96 for ±10 percentage points, 196 for ±7, 384 for ±5 [I] (own arithmetic). Target n = 200 completes (±7 points), minimum 100.
- Caveat: convenience sampling carries unmeasured bias; confidence margins describe sampling error only. Report segments only with n of at least 30; below that give counts, not percentages.
- Pilot cohort (section 6): n = 20 to 30 is enough to find usability failures (most issues come from the first 5-8 users, recalled, UNVERIFIED) but not for rates tighter than about ±18 points at 70% observed (my approximation at n = 25) [I].

---

## 6. Success metrics for a student pilot (4 weeks, 20-30 users, P-A first)

Targets are proposals [I] built from the product's first principles, not benchmarks; I found no Thai finance-app benchmark in this session (UNVERIFIED). Set them before the pilot starts so the team cannot move them afterwards.

| Metric | Definition | Target | Reasoning |
|---|---|---|---|
| Activation | % of invited users who enter at least 1 loan and 1 income and view the due list within 48 hours | 60%, floor 40% | Setup is a one-time task; failing 40% means the entry flow is the problem. Pilot users are friends so expect optimism bias; do not over-read a high number. |
| Weekly return | % of activated users who open the due list or dashboard in weeks 2, 3 and 4 (any one of the weeks counts for week metric) | 40% active in week 4, floor 25% | Debt tools are checked weekly or monthly, not daily; so use weekly return rather than daily. LINE reminders count as passive; count only user-initiated opens separately. |
| Data-entry time | Median time from start to first complete loan record, plus time for a full set of 3 loans, stopwatch plus app log | Under 3 minutes per loan, under 10 minutes for the set | Matches the second-job risk in repo 03 L184; above 10 minutes predicts dropout. |
| Due-date accuracy | % of due dates in the app that match the real contract or statement (user checks against synthetic or own paper) | 100% of dates the engine computes from correct inputs (engine test), 95% of user-entered schedules correct | Code computes every number, so engine errors must be zero; user-entry errors are the realistic gap. A wrong due date is the worst failure for a reminder tool. |
| Amortization accuracy | Engine vs independent calculator on 20 synthetic cases | Interest total within THB 1 per loan | Deterministic tests, not user-dependent. |
| AI-summary comprehension | Quiz of 3 questions per summary; share answering correctly | 80% correct, floor 70% | Plain-Thai summaries should be read by a non-expert; below 70% means the wording or numbers confuse. |
| Trust | 1-5 rating: "I trust the numbers shown", and "the AI summary stayed descriptive" | Mean 4.0 or higher; zero summaries containing buy/sell/switch language in the audit log | Zero is the safeguard rule from CLAUDE.md, enforced by an output filter, measured on all logged outputs. |
| Usefulness | % saying they would be disappointed if the tool disappeared | 40% | Sean Ellis style benchmark is from memory (UNVERIFIED); use as a rough guide only. |
| Willingness to pay | % willing to commit to a stated price after the pilot (a signed pledge, not an opinion) | 15-20% pledge; record price points | Opinion-only WTP overstates; a pledge is closer to behaviour (repo 03 L121 agrees). |
| Safeguard compliance | Disclaimer shown on 100% of AI screens; masking test: 0 identifiers in logged LLM prompts | 100% / 0 | Non-negotiable rules in CLAUDE.md. |

Reporting rule: with n of 20-30, report counts (for example "14 of 22") with the Wilson interval, not bare percentages, and list every dropout reason.

---

## 7. Gaps to fix next (priority order)

1. Read the BOT 2024 household financial literacy PDF locally for budgeting, record-keeping and compound-interest findings (saved copy path in section 2.3).
2. Confirm NESDC Q2 2026 household debt/GDP on an NESDC or BOT page (SCB EIC gives 85.2% [S]).
3. Get the SET and SEC investor counts from a primary table, with one as-of date and definition.
4. Thai-language and app-store competitor check (CLAUDE.md rule): MeowJot, bank apps, debt calculators.
5. Find the NSO "share of households with debt by type" table.
6. Verify mortgage (MRR/MLR) and credit-card/personal-loan caps from BOT pages.
7. Run the interviews; they replace most [I] statements above.

## 8. Source list (all opened or surfaced this session)

- https://thaipublica.org/2026/07/scb-eic-on-household-debt-q1-2026/ (16 Jul 2026)
- https://www.nationthailand.com/business/40071155 (17 Sep 2026)
- https://www.nationthailand.com/news/general/40058692 (24 Nov 2025)
- https://datareportal.com/reports/digital-2026-thailand (8 Nov 2025)
- https://www.nationthailand.com/news/general/40040603 (15 Aug 2024)
- https://www.thairath.co.th/money/personal_finance/finance_banking/2886724 (3 Oct 2025)
- https://www.krungsri.com/th/krungsri-the-coach/loan/auto-loan/update-car-lease-agreement
- https://www.bot.or.th/en/news-and-media/news/news-20251110.html (10 Nov 2025)
- https://www.scbx.com/en/scbx-exclusive/thai-consumer-ai-adoption/ (27 Jan 2026)
- Search-only (not opened): NSO survey PDFs, Tidlor notice, thaipbs.or.th/news/content/505069, kaohooninternational.com/?p=573603, secopendata.sec.or.th/Investor/INV_MF, khaosodenglish (403), SEC strategic plan (403), pattayamail Krungsri survey, uobgroup ACSS 2025, Capco/Accenture/Bangkok Post 2022 items.
- Blocked or unreadable: SEC strategic plan PDF (403), Khaosod English (403), BOT 2024 financial literacy PDF (binary, unreadable by extractor).
- Not searched (time): KResearch, Thai Bankers Association, Thailand Digital Stack, Thai PBS survey on debt, LINE Thailand corporate stats, NESDC primary site.
