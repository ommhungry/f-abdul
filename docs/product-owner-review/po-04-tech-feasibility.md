# PO review 04: technical feasibility (loan engine, data, LLM, ingestion, architecture, effort)

Date of research: 2026-11-21. Author: technical-feasibility researcher. No repo files modified. No real personal data used; every number below is synthetic.

Tag key (per docs/repo-conventions.md): [V] seen on a primary page by me; [S] secondary or search-summary; [I] my inference; UNVERIFIED = not confirmed; [V-calc] computed by me, method stated. All web pages accessed 2026-11-21 unless stated. Internal repo files are cited as [S-repo] (they carry their own tags).

Calculation files (scratchpad, reproducible): /private/tmp/claude-501/-Users-jay-Desktop-Senior/7f165d63-f1fc-4879-9a7d-fd156f2f402f/scratchpad/calc/ (eng.py, vec.py, v2.py, cc.py).

---

## 0. Bottom line for the product owner

1. The amortization engine is feasible and is the best-defined part of the project. Reducing-balance, flat-rate-to-effective-rate, stepped floating mortgage, credit card minimum payment and what-if payoff are all closed-form or short loops. Budget 24-36 h with tests (12-16 h for reducing-balance only).
2. The biggest feasibility risks are not maths. They are (a) price data licensing for SET stocks (paid, redistribution is a separate licence), (b) the Thai OCR accuracy claims, which I could verify only as paper claims, and (c) the LLM masking plus output filter plus eval set (30-45 h on its own).
3. LLM cost is small if a cheap model explains code-computed numbers. At 30 interactions per month a Haiku-class or Flash-Lite-class model costs about THB 0.5-2.3 per user; a Sonnet-class model about THB 12-23. Cost is not the constraint; quality in Thai and filter reliability are.
4. Recommended architecture: one TypeScript web app (responsive PWA) with the engine as a pure module, Postgres, and LINE as a thin add-on (reply-only bot plus LIFF opening the same web app). Cheapest alternative: Python engine plus Streamlit-style single app on SQLite, no LINE.
5. Effort: all 11 features plus foundations and tests is about 215-330 h, which does not fit any capacity figure in the docs. A working-demo MVP is 120-175 h (without the LLM feature) or 140-205 h (with a minimal LLM explain feature). Details in section 6.

---

## 1. Thai loan maths and the amortization engine

### 1.1 Rules that drive the engine (external facts)

| # | Fact | Tag | Source |
|---|---|---|---|
| 1 | BOT credit card cap: not more than 16% a year; personal loans (including cash cards) not more than 25% a year (March 2026 article). | [S] | https://en.thairath.co.th/money/personal_finance/financial_planning/2918376 |
| 2 | Credit card minimum payment is 8%; BOT extended the reduced rate (8%) to end of 2026, the scheduled return to 10% was postponed. After 2026 is UNVERIFIED, so the engine must take min% as a dated parameter. | [S] | https://www.nationthailand.com/business/banking-finance/40059264 (search summary) |
| 3 | BOT example: debt 80,000 at 16%, minimum 8%, cleared in about 6 years 3 months, interest about 16,000. My simulation with a 100 baht floor gives 74 months and 15,628.18 interest (section 1.4, TV-6 note). Agreement is approximate; the exact minimum-payment definition (floor, rounding, whether 8% applies to balance or to balance plus interest) is UNVERIFIED. | [S] example, [V-calc] my side | https://www.prachachat.net/finance/news-1368667 (search summary) |
| 4 | Hire purchase for car and motorcycle: caps 10% new car, 15% used car, 23% motorcycle, default interest max 5%, quoted as effective rate; BOT notices 55/2568 and 56/2568, gazette 3 Dec 2025. The caps themselves are not new: they date from the OCPB contract-control notice of 10 Jan 2023. | [V] numbers per the repo red-team file; [S] OCPB history | [S-repo] docs/research/2026-11-03-verify-hire-purchase.md sections 1-2 |
| 5 | Hire purchase royal decree in force 2 Dec 2025, criteria phased 3 Dec 2025 to 1 Jun 2026; criterion 5 requires providers to state early-closure conditions. No fee numbers on the page. | [V] | https://www.bot.or.th/th/research-and-publications/articles-and-publications/bot-magazine-issues/phrasiam-68-3/hire-purchase-leasing.html |
| 6 | Hire purchase early-payoff discount: at least 60% of unearned interest if up to one third of instalments have been paid, rising after that (OCPB). Exact schedule UNVERIFIED by me. | [S-repo] | docs/research/2026-11-03-verify-hire-purchase.md |
| 7 | BOT directive of 7 Jan 2020 (as explained by SCB): early-repayment penalty is charged on remaining principal, not the original loan (example 2% x 7,000,000 = 140,000 instead of 200,000); overdue interest charged on the overdue instalment principal, not the whole balance (example 10,000 x 8% x 30/365 = 65.75 instead of 31,364.38 on 4,770,000). | [V] | https://www.scb.co.th/th/personal-banking/stories/tips-for-you/interest-rate-method.html |
| 8 | Day count: every time-based example on the SCB page divides by 365 (fee proration 300 x 90/365 = 73.97). The page does not state a general convention. Actual/365 is therefore the default for Thai retail products; 365/360 (actual/360) is a bank-specific variant and must be a per-loan parameter. | [V] examples, [I] convention | same SCB page |
| 9 | Mortgage refinance: banks may still charge a fee on refinancing in the first 3 years (Kasikorn statement; GSB sale sheet shows 3% of remaining principal if closed within 3 years to move to another institution). Not a universal BOT ban. | [S] | https://www.kasikornbank.com/th/announcement/debtsolution-measure/pages/prepayment-fee.aspx (page empty to my fetch; claim from search summary); https://www.gsb.or.th/media/2024/01/sale-sheet-สินเชื่อเคหะ.pdf |
| 10 | Floating mortgages are priced as MRR minus x for early years and MRR (or MRR plus) afterward. Policy rate 1.25% since 17 Dec 2025; GSB MRR 6.195% from 22 Dec 2025, BAAC MRR 6.575% from 1 Apr 2026. No bank step table (MRR-x for years 1-3) was found; current values for the big banks are UNVERIFIED. The engine therefore needs a user-entered rate-step table, not hard-coded bank data. | [S] | https://www.thairath.co.th/money/personal_finance/finance_banking/2903481 and search summary |
| 11 | Student loan (Gor Kor Sor): after the 2023 amendment, interest not more than 1% a year, penalty 0.5% a year (was 18%), payment order principal then interest then penalty. How interest is accrued (daily on actual balance?) and how early payoff is computed are UNVERIFIED, so the engine models it as a plain 1% reducing-balance loan with a flagged "estimate". | [S] | https://www.thaipost.net/news-update/347559/ (search summary) |
| 12 | Flat-rate hire purchase is a quoting convention: interest = principal x flat rate x years, spread equally. The effective rate is about 1.7-1.9 times the flat rate (computed below), so any "x% flat" must be converted before comparing with a bank loan. | [V-calc] | section 1.4 |

### 1.2 Engine specification

Money type: integer satang (1/100 baht) or Decimal with 2 places; never binary floats. Rates as exact decimals or rationals. All arithmetic in the engine uses a single rounding rule, below.

Inputs (loan record): principal P; start date; first payment date; payment day; periods n; payment frequency (monthly in v1); rate model; day-count (ACT/365 default, ACT/360, 30/360); rounding mode; optional payment override; optional extra-payment plan; optional rate-step table (date, annual rate); optional fees; optional promotion/grace.

Rate models:
- R1 reducing balance, level payment (mortgage, personal loan, student loan estimate).
- R2 flat rate: schedule = P x (1 + flat x years) / n each period; engine also reports the effective rate (IRR) and builds an amortization on the effective rate (used for split, payoff).
- R3 stepped floating: rate table by date. Two policies must both be supported and the user picks (default re-amortize at each step, because that is how Thai mortgage statements normally behave [I]): (a) re-amortize payment over remaining periods at the new rate; (b) keep payment and let the term change. Policy (b) can fail: see TV-4b.
- R4 revolving card: balance, APR, minimum payment rule (pct, floor, as-of-date table), statement/due-date logic, daily interest option.
- R5 interest-only / grace period: optional, P1.

Periodic interest:
- Monthly nominal (r = annual/12) is used when the contract states "per month" level payments. Dated ACT/365: interest for a period = round(balance x annual x days / 365). Both are in the vectors.
- Rounding: round half up to 0.01 per period on interest; payment fixed to 0.01; principal = payment - interest; last payment = remaining balance + that period's interest (this absorbs residual). Without the last-payment rule the schedule leaves a residue (my first run produced a 241st payment of 0.28 baht on TV-1). Store the rule with the loan so schedules are reproducible.
- Bank statements may differ by a few satang to a few baht because of their own rounding; the app must show "estimate" and let the user enter the actual instalment, then re-derive.

Outputs: schedule rows (n, date, payment, interest, principal, balance); totals; effective annual rate; next-due date and amount; remaining balance as of a date; payoff quote as of a date (remaining principal + accrued interest to date + optional fee on remaining principal); what-if (extra monthly payment, lump sum on date, shorter term) returning end date, months saved and interest saved.

Edge cases the tests must cover (each is a named test):
1. Rate 0% (promotion): PMT = P/n, interest 0.
2. Payment below monthly interest (negative amortization), for example floating step with constant payment: must raise a clear error, not loop (TV-4b shows 3,000,000 at 5% needs 12,500 interest per month versus a 11,853.63 payment).
3. Extra payment larger than remaining balance: final payment clipped to payoff.
4. Payment date on 29/30/31 and February; leap year 2028 (Feb has 29 days): ACT/365 versus ACT/366 differ (TV-8).
5. Holidays: the due date shifts but interest accrues to the actual paid date; v1 policy: use the contractual date, flag as estimate [I].
6. Buddhist Era dates on input; store ISO Gregorian.
7. Rate step in the middle of a period: split the period at the step date (dated model) or apply from the next period (monthly model); document which.
8. Prepayment before first instalment.
9. Large n (360 months) with 2-decimal rounding drift: tolerance in section 1.3.
10. Card: balance below floor, floor above due amount, minimum % changing on a date, new purchases mid-cycle (v1 ignores purchases; user edits balance).
11. Flat rate with fees financed or a balloon: out of v1 scope; warn.
12. Early payoff on flat-rate contract: three methods give different numbers (EIR basis, Rule of 78, naive remaining instalments). TV-9 shows 380,041.08 vs 381,245.90 vs 414,000. Thai law governs the rebate (item 6); the engine must label which method it used.

### 1.3 Method and tolerance of the cross-check

Method A: closed form. Level payment PMT = P i / (1 - (1+i)^-n), balance after k payments B_k = P (1+i)^k - PMT ((1+i)^k - 1)/i, total interest = n PMT - P. Computed in exact rational arithmetic (Python Fraction) and rounded once at the end.
Method B: period-by-period schedule in Decimal with rounding every period and the last-payment rule.
Differences: because B rounds the payment and interest every period, totals differ from A by up to about n x 0.005 baht in theory; observed 0.14 baht over 240 periods, 0.01 over 24, 1.28 baht over a 3-segment 360-period mortgage. Test tolerance proposal: schedule total interest within 0.01 x n baht of the exact value, balances within 0.50 baht at any k, and the final balance exactly zero.
For effective rates: bisection in Decimal (200 iterations) versus Newton in floats, agreement to 12 significant digits. For payoff-month counts: schedule versus closed-form n = -ln(1 - P i / pay) / ln(1+i).
For cards and dated schedules: a decimal implementation versus an independent integer-satang implementation (half-up using integer division).

### 1.4 Test vectors (all [V-calc], computed 2026-11-21 with the scripts listed above)

TV-1 Level-payment loan. P = 1,000,000.00; 6.00% nominal a year, monthly (i = 0.5%); n = 240.
- A (exact): PMT 7,164.31; total interest 719,434.54; balance after 12 = 973,302.01; after 120 = 645,314.20.
- B (schedule, last-payment rule): PMT 7,164.31; 240 payments; last payment 7,164.59; total interest 719,434.68; total paid 1,719,434.68; month-1 interest 5,000.00, principal 2,164.31; balance after 12 = 973,302.02; after 120 = 645,314.31.
- Cross-check result: payment equal; interest differs by 0.14 baht (rounding). Pass within tolerance.

TV-2 Car flat rate. Financed 600,000; flat 3.00% a year; 60 months.
- Interest 600,000 x 0.03 x 5 = 90,000; instalment (600,000 + 90,000)/60 = 11,500.00.
- Effective rate by bisection: monthly 0.4701473372%, nominal annual 5.641768%, effective annual (compounded) 5.789964%.
- Newton in floats: monthly 0.004701473372186, nominal 5.6417680%, effective 5.789964%. Agree to 12 digits.
- Rule-of-thumb 2 n f/(n+1) gives 5.9016%: an overestimate, do not use in production.

TV-3 Motorcycle flat rate. Financed 40,000; flat 20% a year; 24 months.
- Interest 16,000; instalment 2,333.33.
- Effective: nominal annual 34.6485% (monthly 2.8874%), effective annual 40.7165%. Both methods agree to 12 digits. Rule of thumb gives 38.4%.
- Consequence for the app: a 20% "flat" motorcycle contract is 34.6% nominal, far above the 23% effective cap reported for motorcycles [V-repo], so the engine's effective-rate output is a useful compliance and comprehension check. I do not claim legal conclusions; the app only states the number [I].

TV-4 Stepped floating mortgage, re-amortize at each step. P = 3,000,000; 360 months; 2.50% for months 1-12, 3.50% for 13-24, 5.00% from month 25.
- Segment 1 PMT 11,853.63 (n=360); balance after 12 = 2,931,980.58.
- Segment 2 PMT 13,423.52 (n=348); balance after 12 = 2,872,570.64.
- Segment 3 PMT 15,901.80 (n=336); ends at 0.00.
- Total interest B = 2,646,332.85; A (exact rational, no per-period rounding) = 2,646,331.57; difference 1.28 baht over 360 periods. Pass (< 0.01 x 360 = 3.60).

TV-4b Same loan, payment kept at 11,853.63 after the step. At 5.00% the monthly interest on 2.87m is above 11,853.63, so the loan never amortizes. Engine must return an error "payment below interest at month 25", not a schedule. Edge case 2.

TV-5 What-if extra payment on TV-1: +5,000 a month from month 1.
- Schedule: payment 12,164.31; clears in 107 months (last payment 1,747.83); total interest 291,164.69; interest saved 428,269.99; months saved 133.
- Closed form n = -ln(1 - 1,000,000 x 0.005 / 12,164.31)/ln(1.005) = 106.14, rounded up to 107. Agree.

TV-6 Credit card. Balance 80,000; 16% a year; interest = balance x 16%/12; minimum = max(8% of (balance + interest), 500 baht), capped at amount due; no new purchases.
- Decimal: 51 months, total interest 15,148.95. Integer satang: 51 months, 1,514,895 satang. Agree.
- Note on the BOT example (fact 3): with floor 100 the same simulation gives 74 months and 15,628.18; with floor 0 it never finishes. The minimum-payment definition therefore decides the result; keep the vector for engine regression, not as a statement about any bank.
- Second card vector: 30,000; 18%; 8%; floor 500: 37 months, interest 6,103.62 (decimal) = 610,362 satang (integer). Agree. (18% is above the 16% cap and is used only as a test input.)

TV-7 Dated ACT/365 loan. P = 500,000; 7.00% a year; first draw 2027-01-05; 12 payments on the 5th; interest = round(balance x 7% x days/365).
- Level payment found by bisection: 43,256.80 (last payment 43,256.76); total interest 19,081.56. Period 1 (Jan 5 to Feb 5, 31 days) interest 2,972.60; period 2 (28 days) 2,468.61; period 3 (31 days) 2,490.61; period 12 (31 days) 255.65.
- Independent integer-satang run with the same payment: interest 1,908,156 satang, last payment 4,325,676 satang. Agree.
- Same payment under ACT/360: interest 19,358.33 and a final payment of 43,533.53. Day-count alone moves total interest by 276.77 baht (1.45%) on a one-year loan.

TV-8 Day-count single-period and rules.
- 1,000,000 at 6% for 31 days: ACT/365 = 5,095.89; ACT/360 = 5,166.67; 30/360 monthly = 5,000.00.
- 29 days of Feb 2028 on 1,000,000 at 6%: /365 = 4,767.12; /366 = 4,754.10.
- Overdue interest on instalment principal 10,000 at 8% for 30 days = 65.75 (matches the SCB example, fact 7).
- Prepayment fee 2% on remaining principal 7,000,000 = 140,000.00 (matches SCB example).

TV-9 Flat-rate car contract early payoff. TV-2 contract after 24 payments paid, 36 remaining.
- EIR basis (present value of the 36 remaining 11,500 payments at 0.4701473372% monthly): 380,041.08. Cross-check by rolling the balance forward 24 periods from 600,000: 380,041.08. Agree.
- Rule of 78 on the 90,000 interest: rebate 32,754.10; payoff 381,245.90.
- Naive (36 x 11,500): 414,000.00.
- The three differ by up to 33,958.92 baht. Which one a lender uses is contractual and, for personal-use vehicle contracts, governed by the OCPB/BOT discount rule (fact 6). The engine must label the method and never present one as "the" answer.

TV-10 Student-loan estimate and personal loan.
- Personal loan 100,000 at 25% (cap), 24 months: exact PMT 5,337.15, exact interest 28,091.65; schedule PMT 5,337.15, last 5,337.21, interest 28,091.66.
- Student loan estimate 200,000 at 1%, 180 months: exact PMT 1,196.99, interest 15,458.03; schedule 1,196.99, last 1,196.83, interest 15,458.04.
Both within one satang of exact.

Totals: 10 vectors (plus sub-vectors), each cross-checked by two independent methods.

### 1.5 Effort and risks specific to the engine

- Logic is small; the effort is in the tests, the explanation of results to the user, and the rate-step and payoff conventions that vary by lender. Reducing-balance monthly: 12-16 h. Add flat-to-effective, card, stepped floating, dated ACT/365, what-if, payoff quote: 24-36 h in total.
- Never promise to match a bank statement exactly. Ship the "enter actual instalment, then derive" flow as the accuracy fix.

---

## 2. Market-price data

| Asset | Option | Facts | Cost / limits / licence | Tag | Source |
|---|---|---|---|---|---|
| SET stocks | SET SMART Marketplace API (JSON, key header); real-time or 15-minute delayed; separate service types "For Personal Use / Internal Use" and "For Re-distribution". | The marketplace page states the API exists and delayed data must be labelled delayed. The pages I fetched give no price or licence text. One search summary quoted THB 15,000 a month for a natural-person tier; I could not confirm the product or period. | Paid. Redistribution to end users likely needs the redistribution licence. Treat as not available to a student project. | [V] existence; price UNVERIFIED | https://set.or.th/en/services/connectivity-and-data/data/smart-marketplace ; https://media.set.or.th/set/Documents/2023/May/Market_Data_API_Service_Specification.pdf |
| SET stocks, cheap path | User types the price and date ("last updated"); or user imports a broker CSV. | Consistent with vj's "manual entry with last-updated date" (docs/consolidated/_decisions-scope.md D-S5). | Free. | [I] | |
| SET stocks, unofficial | Scraping free quote sites or Yahoo-style tickers (.BK). | Terms normally forbid redistribution; not verified here. Not recommended for a public demo. | | UNVERIFIED | |
| Thai mutual funds NAV | SEC Open Data "Fund Factsheet" and "Fund Daily Info" APIs; key through the SEC developer portal; a third-party Go library documents a v2 base `api.sec.or.th` with `/v2/fund/daily-info/nav` and states a limit of 5,000 calls per 300 seconds. | The SEC open-data page confirms an "Open API: Fund Factsheet" exists. Two community sources disagree on whether the portal is `api-portal.sec.or.th` (v1) or `api.sec.or.th` (v2); my fetch of api-portal.sec.or.th failed with a DNS error, and the secopendata page returned HTTP 403 to my fetcher. Redistribution licence not seen. | Free key (per community sources). Cache NAV nightly in our DB; never call per user request. | [S] throughout; limit UNVERIFIED | https://pkg.go.dev/github.com/jwitmann/sec-go ; https://github.com/Zummation/SEC-API ; https://secopendata.sec.or.th/Fund/MF_IPO |
| Crypto | CoinGecko Demo plan: 10,000 calls/month cap, 100 calls/min, attribution required. Paid plans from USD 35 a month (Basic) upward; commercial licences only listed for paid plans. | Prices are read from the pricing page. | Demo is for non-commercial prototypes in practice [I]; a thesis demo is fine, a product is not. | [V] | https://www.coingecko.com/en/api/pricing |
| Crypto, Thai exchange | Bitkub public market endpoints (docs list v3/v4; old ticker endpoint deprecated 9 Dec 2025). | Auth, limits and terms not shown on the page I could read. | | [V] deprecation; rest UNVERIFIED | https://github.com/bitkub/bitkub-official-api-docs |
| FX | Bank of Thailand API portal (token after free account; daily weighted-average interbank USD/THB, based on deals of at least USD 1m). Frankfurter's BOT provider re-serves BOT rates for 20 currencies, no key, no limit stated, data from 2002. | BOT portal terms, limits and pricing not confirmed. | Free (Frankfurter). | [S] BOT portal (via a Ruby wrapper doc); [V] Frankfurter | https://frankfurter.dev/providers/bot/ ; https://rubydoc.info/gems/bank_of_thailand |
| Gold | No official API from the Gold Traders Association was found. A community MIT-licensed wrapper crawls goldtraders.or.th and its README says it cannot yet handle a 404 from the source. The association posts prices many times a day (one report counts 23 changes in a day). | Scraping a site whose terms I have not read; fragile. | Free but unstable; keep gold as manual entry with an optional scrape behind a feature flag. | [V] wrapper README; [S] frequency | https://github.com/max180643/thai-gold-api |

Recommendation: ship manual price entry for everything first (it is already in the baseline); add SEC NAV (nightly cached) and FX (Frankfurter/BOT) second, crypto via CoinGecko Demo only for the demo with attribution; leave SET stocks and gold manual and state "price may be delayed or entered by you". Do not claim real-time anywhere. Effort for the three automatic feeds: 12-20 h together (section 6).

Licence note for the report: redistribution status is UNVERIFIED for SEC, BOT, Gold Traders Association; confirm before any public launch [I].

---

## 3. LLM: models, price, Thai quality, safe output

### 3.1 Prices from official pages (per million tokens, USD, standard tier)

| Model | Input | Output | Cache read | Tag | Source (accessed 2026-11-21) |
|---|---|---|---|---|---|
| Claude Haiku 5.5 (prompt up to 100k tokens) | 0.10 | 0.50 | 0.01 | [V] | https://platform.claude.com/docs/en/about-claude/pricing |
| Claude Sonnet 5.5 | 2.00 | 10.00 | 0.10 | [V] | same |
| Claude Opus 5.5 | 4.00 | 20.00 | 0.20 | [V] | same |
| Claude Fable 5.2 | 8.00 | 40.00 | 0.20 | [V] | same |
| Gemini 3.5 Flash-Lite | 0.30 | 2.50 (incl. thinking) | n/a | [V] | https://aistudio.google.com/docs/pricing (redirect from ai.google.dev) |
| Gemini 2.5 Flash-Lite | 0.10 | 0.40 | n/a | [V] | same |
| Gemini 3.1 Flash-Lite | 0.25 | 1.50 | n/a | [V] | same |
| OpenAI gpt-6-luna | 0.10 | 0.50 | 0.01 | [V] | https://developers.openai.com/api/docs/pricing |
| OpenAI gpt-5.4-mini | 0.75 | 4.50 | 0.075 | [V] | same |

Facts that matter:
- Omm's rate "Gemini 3.5 Flash-Lite USD 0.30 in / 2.50 out" (docs/consolidated/02, UNVERIFIED there) is now [V]: it matches the Google page.
- Gemini free tier content is "used to improve our products" [V]; the free tier must not be used for any user financial data, even masked [I].
- Gemini 2.5 Pro and Flash are marked deprecated, requests route to other models from 1 Dec 2026; several Gemini prices rise on 1 Dec 2026 and 1 Jan 2027 [V]. Model choice must be re-checked at build time.
- Claude models since 4.7 use a tokenizer that produces about 30% more tokens for the same text [V]. Thai text usually tokenizes heavily on every provider; I assume 1x to 2x the English count and show both [I].
- Claude batch API halves price [V], not useful for chat but fine for nightly summaries.
- Prompt-caching: Claude cache reads are 0.05x-0.1x base [V]; useful for a fixed system prompt plus template.

### 3.2 Cost per active user per month [V-calc]

Assumptions [I]: THB 35 per USD (the docs' figure; not re-verified), 3,000 input and 500 output tokens per interaction (Omm's figure), no caching, no retries.

| Model | Light user 8/mo | Typical 30/mo | Heavy 120/mo | Typical 30/mo with 2x tokens (6,000 in, 1,000 out) |
|---|---|---|---|---|
| Claude Haiku 5.5 | THB 0.15 | 0.58 | 2.31 | 1.16 |
| Gemini 2.5 Flash-Lite | 0.14 | 0.53 | 2.10 | 1.05 |
| gpt-6-luna | 0.15 | 0.58 | 2.31 | 1.16 |
| Gemini 3.5 Flash-Lite | 0.60 | 2.26 | 9.03 | 4.52 |
| gpt-5.4-mini | 1.26 | 4.73 | 18.90 | 9.45 |
| Gemini 4 Hydrogen | 2.31 | 8.66 | 34.65 | 17.32 |
| Claude Sonnet 5.5 | 3.08 | 11.55 | 46.20 | 23.10 |
| Claude Opus 5.5 | 6.16 | 23.10 | 92.40 | 46.20 |

Reading: Omm's THB 15-20 per user budget is not binding for any small model; only Opus-class models at heavy use approach it. Excluded: OCR/vision calls, hosting, LINE messages, retries.

### 3.3 Thai quality and structured-output reliability

- Thai-language quality: I did not find a verified, independent, current Thai benchmark comparing these models for explanation text. Treat quality as UNVERIFIED and decide by a local eval (section 3.5). The only Thai-specific data I could verify is the Typhoon OCR paper (section 4), which is about OCR, not chat.
- Structured output: the Anthropic API supports schema-constrained output (`output_config.format`) and strict tool schemas [V, skill reference for the Claude API; claude-api documentation]. Constrained JSON removes most parse failures but not wrong values, so the backend must still validate against the code-computed numbers. Gemini and OpenAI also document JSON schema modes [S, not fetched here]; UNVERIFIED for these models.
- Design rule: the LLM never returns numbers that are displayed. The prompt gets the computed numbers as labelled fields; the output is a template with slots, and a check verifies that every number in the text appears in the input set (see 3.4).

### 3.4 Enforcing descriptive-only output (defence in depth)

1. Role of the model: extraction (slip text to fields) and explanation (fixed facts to Thai sentences). No tools, no browsing, no free chat in the first release (this resolves D-A decision on tool use in the strict direction; it is a recommendation for the PO to confirm).
2. Templates: each analysis has a fixed Thai template with slots (for example "ภาระหนี้ต่อเดือนคิดเป็น {dsr}% ของรายได้ที่บันทึกไว้"). The model fills only the narrative slots; the numbers come from code.
3. Prompt: system prompt states the allowed verbs and bans buy/sell/switch/hold, "should", "recommend", suitability, product comparison, target price. Prompting alone is not relied on.
4. Output filter, applied to every response before display:
   - Deny-list regexes in Thai and English (should buy, ควรซื้อ, ควรขาย, แนะนำ, น่าลงทุน, ซื้อ/ขาย/สลับ, target price, ราคาเป้าหมาย, better than, ดีกว่า, "คุณควร", "เหมาะกับคุณ").
   - Number whitelist: extract every number from the text (Thai and Arabic digits); each must exist in the input facts or be a derived rounding of one; otherwise block.
   - Entity whitelist: only product names that are in the user's own records.
   - On failure: show the fixed template with numbers only (no narrative) and log the block.
5. Disclaimer: constant text outside the model's control.
6. Audit log: store prompt version, masked input, raw model output, filter verdict, shown text, model id, timestamp, user id hash (section 5.4).
7. Regex lists are brittle in Thai (no spaces). Add a second check: a small classifier prompt (cheap model) that answers only "does this text give advice or a recommendation: yes/no" over the final text, used as an additional veto, not the only one [I].

### 3.5 How to test it

- Eval set of at least 150 cases built from synthetic profiles: 60 normal explanation cases; 50 adversarial user inputs ("should I sell X", "which fund is better", "pay off the card or invest", prompt-injection text inside a slip note or an asset name); 20 Thai-specific formats (Buddhist Era dates, Thai digits, mixed names); 20 masking leak probes.
- Metrics: advice-leak rate (target 0 of 50 adversarial), number-fidelity (every displayed number appears in the facts; target 100%), masking-leak rate (target 0), Thai fluency rated by two team members on a 1-5 scale, filter false-block rate.
- Run on every prompt change in CI with a pinned model id; record model and date in the results file.
- Claude API skill documents an eval-building workflow that can be reused [V, skill docs].
- Effort: 30-45 h for masking + templates + filter + eval + audit (section 6).

---

## 4. Ingestion: slips, OCR, statements, LINE images

### 4.1 Slip QR versus OCR

- The Thai transfer slip carries a "mini QR" that does not contain the amount or the names; it carries a sending-bank code and a transaction reference. SCB's documentation describes a tagged payload where tag 00 identifies the API type ("000001" marks the pay-slip verification request), tag 01 the sending bank and tag 02 the transaction reference (up to 25 characters) [S: search summary of https://developer.scb.co.th/assets/documents/documentation/qr-payment/extracting-data-from-mini-qr.pdf; my direct fetch failed with a DNS error]. A community library describes a standard variant (Bank of Thailand Thai QR Payment supplement) and a TrueMoney variant [S: https://thai-qr-payment.js.org/guide/slip-verify/].
- To turn the reference into amount, date and parties, one must call a bank's slip-verification API (SCB has `GET /v1/payment/billpayment/transactions/{transRef}`; Bangkok Bank has a Pull Payment Transaction API) or a commercial aggregator (for example EasySlip, which accepts payload, image or Base64 and offers a duplicate check) [S]. These need merchant/partner onboarding or a paid plan; terms and price UNVERIFIED.
- Consequence [I]: QR-only parsing without a verification service gives a transaction id only, which is useful for duplicate detection but does not give the amount. Getting the amount needs OCR of the slip image or a verification API. This is a real design constraint for "slip QR first, OCR fallback" in the scope spec (P2, one demo flow): the QR step can de-duplicate and verify, the OCR step must supply fields.
- Slips are personal data (names, partial account numbers). The scope rules require masking before any LLM call and synthetic data only in the repo; the ingestion path must run OCR locally or send the image only to a processor with a data-processing agreement, then mask the text.

### 4.2 OCR options

| Option | Verified facts | Unverified | Tag | Source |
|---|---|---|---|---|
| Typhoon OCR (SCB 10X, open weights) | Paper on arXiv (21 Jan 2026) compares with GPT-4o and Gemini 2.5. V1 7B on Thai financial reports: BLEU 0.91, ROUGE-L 0.94, Levenshtein distance 0.07 (lower is better) versus Gemini 2.5 Flash 0.52 / 0.70 / 0.35 and GPT-4o 0.25 / 0.51 / 0.56. Government forms: 7B 0.89 / 0.94 / 0.08, Gemini 2.5 Flash 0.74 / 0.87 / 0.15. V1.5 2B is based on Qwen3-VL 2B. The paper says proprietary models score better on infographics and handwriting. License: model card of the 7B lists Apache-2.0; V1.5 license not read. | No benchmark on transfer slips or mobile photos; scores are from the authors' own benchmark. | [V] numbers as printed in the paper; claim generality [I] | https://arxiv.org/html/2601.14722 ; https://huggingface.co/typhoon-ai/typhoon-ocr-7b (search summary for license) |
| Gemini (vision) | Appears in the Typhoon comparison tables above (Gemini 2.5 Flash, Gemini 2.5 Pro). Price in section 3.1. Free tier data is used for product improvement [V]. | Slip accuracy | [V] | pages above |
| Google Cloud Vision (text or document text detection) | Free first 1,000 units a month and about USD 1.50 per 1,000 after is my recollection; the page content did not load. | Price and Thai accuracy | UNVERIFIED | https://cloud.google.com/vision/pricing |
| Tesseract with `tha` | Open source, offline. No verified accuracy figure found by me. | Accuracy on slips/photos | UNVERIFIED | |
| Typed or QR-only | No OCR needed. | | | |

Recommendation: for the demo do one flow only, with a fixed slip layout from one bank using a synthetic slip (generated image, no real data). Use Typhoon OCR or a vision model in a local/offline test, user confirms every field, and OCR-only values stay out of totals until confirmed (already in the scope). Measure field accuracy on 30-50 synthetic slips before promising anything. Do not quote any accuracy percentage to the advisor until measured [I].

### 4.3 Statement PDF parsing

- Text-based PDFs: deterministic extraction (pdfplumber/pdfminer or pdf.js) with per-bank parsers; each bank needs its own parser; password-protected statements are common in Thai banks [I]. Image-only PDFs need OCR. Each bank format is 8-12 h [I]. I found no verified source about bank statement formats; treat this as P2+ and out of the MVP.
- Using card/bank statements as demo data raises PDPA risk; synthetic generated PDFs only.

### 4.4 LINE Messaging API images

- Users send images; the webhook gives a message id; the bot downloads content by "Get content" using that id [V: the reference lists a Get content endpoint for content sent to the OA, https://developers.line.biz/en/reference/messaging-api/]. Retention period and size limits not retrieved (page returned only navigation; the markdown version returned 403) UNVERIFIED.
- Reply messages are not counted against the message quota; push, multicast, broadcast and narrowcast are [V: https://developers.line.biz/en/docs/messaging-api/pricing/index.html.md].
- Thailand plans: free 300 messages a month; Basic THB 1,280 for 15,000 messages; Pro THB 1,780 for 35,000, excluding 7% VAT [S: https://yespress.io/line-company-thailand, page checked by the search tool on 2026-09-09]. The official Thai page was not read; confirm before budgeting. This is consistent with the "300 pushes" figure vj reuses.
- Design implication: use reply messages (free) for chat answers; limit pushes (due-date reminders) to the free quota for a demo of a few users.

---

## 5. Architecture

### 5.1 Options for a 3-person student team

| | A. LINE OA + LIFF only | B. Standalone responsive web/PWA, LINE as thin add-on (recommended) | C. Native/Flutter mobile app |
|---|---|---|---|
| What it is | Chat card plus LIFF pages inside LINE | Web app with login; LINE bot for slip upload and reminders; LIFF just opens the same web app | Separate app |
| Dev effort (relative) | Medium; LINE-specific auth, LIFF quirks, webhook signature, ngrok-style local dev | Lowest for the core; LINE parts are small and optional | Highest; two stores, review |
| Demo risk | Depends on LINE account approvals and a verified OA | Low; works without LINE | Medium |
| Reach for Thai users | Highest (LINE is the default channel) [I] | Good via LIFF later | Needs install |
| Fit with PO decision D-A1 (channel not decided) | Chooses a side | Channel-neutral core, matches option C "channel-neutral core" in docs/consolidated/_decisions-architecture.md | |
| Notes | Message quota; chat UI is weak for tables and charts | | |

Recommendation: B. Reasons: the engine, data model, masking and audit log are channel-independent; a web UI is better for schedules and charts; LIFF is "a web page opened in LINE" (vj's own description in the decisions file), so the same pages can be wrapped later. Add a LINE webhook only for (a) slip image upload and (b) due-date pushes within the free quota, if time remains.

### 5.2 Recommended stack (one language)

- TypeScript throughout: Next.js (React) app, API routes or a small Node service, engine as a pure TS package using a decimal library (decimal.js or integer satang with BigInt) and a test runner (Vitest). One language lets all three members touch the engine and tests [I]. Alternative if the team prefers Python: FastAPI + React, engine in Python Decimal (my reference implementation is Python, so test vectors port 1:1).
- Database: PostgreSQL (managed free tier such as Supabase or Neon) with row-level security per user [I]; SQLite is acceptable for the cheapest option.
- Auth: email magic link or LINE Login; no ID-card capture.
- Background jobs: a nightly job (cron on the host) for NAV and FX fetch and due-date push.
- LLM calls only from the server, after the masking step; keys server-side.
- Hosting cost: hobby/free tiers for web host and database exist, but I did not fetch current terms; many hobby plans prohibit commercial use and sleep when idle [I]. Budget for a demo: THB 0 using free tiers; for a pilot: assume USD 5-25 a month for one small server plus database [I], UNVERIFIED, check pages before quoting. LINE: free plan 300 messages a month [S]. LLM: section 3.2. Domain name: about USD 10-15 a year [I].
- Cheapest viable alternative: a single Python app (Streamlit or FastAPI + HTMX) with SQLite on one free/cheap host, engine in the same process, no LINE, manual price entry, LLM calls through one provider key. Saves roughly 25-40 h of foundations and UI [I], loses LINE reach and multi-user hardening. Fine for a thesis demo; not for a pilot.

### 5.3 Data model sketch (tables)

- users(id, auth_id, locale, consent_version, consent_at, created_at)
- accounts_assets(id, user_id, kind[cash|deposit|stock|fund|crypto|gold|property|vehicle|other], name_enc, currency, quantity, unit_price, price_source[manual|sec|fx|coingecko], price_asof, value_cached, notes)
- price_cache(source, symbol, price, currency, asof, fetched_at) shared, no user data
- liabilities(id, user_id, kind[mortgage|car_hp|moto_hp|personal|card|student|other], lender_label, principal, start_date, first_due, n_periods, payment_day, rate_model, nominal_rate, flat_rate, daycount, rounding_rule, payment_override, status)
- rate_steps(liability_id, effective_date, annual_rate, policy[reamortize|keep_payment])
- schedule_rows(liability_id, k, due_date, payment, interest, principal, balance, is_actual, actual_paid_on) regenerated by the engine; actual rows entered by the user
- incomes(id, user_id, kind, amount, recurrence_rule, next_date, source)
- expenses(id, user_id, category, amount, date, recurrence_rule, liability_id nullable, source[manual|slip|import], confirmed boolean)
- ingestion_items(id, user_id, kind[slip|statement], status[pending|confirmed|rejected], qr_ref_hash, extracted_json_masked, confirmed_json, created_at) with images deleted after confirmation (retention rule UNVERIFIED legally; propose delete within 24 h [I])
- ai_runs(id, user_id_hash, feature, prompt_version, model_id, masked_input_json, raw_output, filter_verdict, filter_reasons, shown_text, tokens_in, tokens_out, cost_usd, created_at) append-only
- checklists(id, template_key), checklist_items(id, user_id, template_key, step_key, done, due_date) for feature 6
- audit_events(id, user_id, actor, action, entity, entity_id, before_hash, after_hash, at) for data changes and consent

### 5.4 Audit log design

- Append-only table; no UPDATE or DELETE privilege for the app role; periodic hash chain (each row stores SHA-256 of the previous row's hash plus its own content) so tampering is detectable [I]. The scope requires only "audit log of AI outputs" (safeguard 4); the chain is an optional extra (2-3 h).
- Log what was sent (masked), what came back, which filter rule fired, what the user saw. Keep user ids hashed with a server secret so the log can be shown to the advisor without identities.
- Retention: define in the privacy notice (PDPA duties remain; this is not legal advice) [I].

### 5.5 Masking pipeline design

1. Structured data first: send only fields the task needs (rate, remaining months, amounts). Names, lender names, account numbers, phones, addresses, IDs are never selected into the prompt at all (allowlist, not scrubbing).
2. For free text that must be sent (slip OCR text, notes): run, in order, (a) regexes for 13-digit Thai ID, 10-12 digit account numbers, phone numbers (0x-xxx-xxxx, +66), emails; (b) a Thai name/title detector (honorifics นาย, นาง, นางสาว, คุณ followed by tokens; a Thai NER model such as PyThaiNLP could be tried, UNVERIFIED accuracy); (c) a user-specific dictionary built from the user's own saved lender and person names; (d) replace with typed placeholders [ACCOUNT_1], [LENDER_A], keeping the mapping in memory for the request only (or encrypted at rest).
3. After masking, a verifier pass: run the detectors again on the masked text; if anything still matches, block the call.
4. De-mask only at display time, in our server, not in the LLM.
5. Log the masked text only.
6. Test: 100+ synthetic strings with planted identifiers (including Thai digits and spaced numbers) and a measured miss rate; masking-leak target 0 on the planted set. Masking reduces risk but does not remove PDPA duties (consent, notice, processor terms), as the scope spec itself says.

### 5.6 Test strategy

- Unit tests: engine vectors (section 1.4), property tests (balance never negative, sum of principal = P, schedule ends at 0, extra payment never increases total interest), masking tests, filter tests.
- Contract tests for price feeds with recorded fixtures; the nightly job tolerant of source downtime.
- Eval suite for LLM (3.5) as a separate CI job.
- E2E: one happy path (add loan, see schedule, see due items, dashboard) with Playwright.
- Accessibility and Thai text check on mobile widths.
- Manual acceptance on synthetic data.
- Security: secrets scan, no real data in repo (CLAUDE.md rule), dependency audit.

---

## 6. Effort re-estimate

### 6.1 Assumptions

- Unit: one person's hours, including writing tests, excluding meetings, report writing and exam-time slack. Team of 3 students, medium TypeScript/Python skill, first time with Thai finance rules [I].
- Ranges are low (things go smoothly) to high (one rewrite or API surprise). Learning-curve contingency is not added per row; see 6.4.
- Web-first architecture B (section 5). If LINE-only (A) is chosen, add 10-20 h to foundations and subtract the web dashboard work in exchange for LIFF pages (vj estimates 15-25 h for LIFF, 6-8 h for a chat card, [S] docs).

### 6.2 Per-feature estimates

| # | Feature | Hours (low-high) | What is included |
|---|---|---|---|
| F0 | Foundations: repo, CI, auth, DB schema/migrations, deploy, i18n Thai/BE dates, consent screen | 30-45 | f-abdul's 40-55 h is an old-scope number [S]; mine excludes LINE |
| 1 | Asset list: manual CRUD + CSV import | 10-16 | CSV 3-5 of this |
| 1b | Automatic prices: SEC NAV nightly 6-10, FX 2-4, crypto 3-5 (gold/SET stay manual) | 11-19 | caching, staleness label |
| 2 | Incomes: recurring + one-off | 6-10 | |
| 3 | Expenses: categories, recurring, link to loans (typed entry only) | 8-12 | slip reading counted in 1-slip below |
| 8 | Amortization engine | 24-36 | all rate models, 10 vectors, edge cases; reducing-balance monthly only is 12-16 |
| 4 | Due items in X days / month-end | 8-12 | reads schedule + recurring |
| 7 | Dashboard (web, 3-4 charts + totals) | 14-22 | chat card only: 6-8 |
| 10 | Debt health indicators (DSR, DTI, monthly cash margin) | 8-12 | includes agreeing definitions with the team |
| 11 | What-if payoff simulator | 8-12 | reuses engine |
| 6 | Not-yet-owner checklist (car + mortgage, static templates) | 6-10 | content research dominates |
| 1-slip | Slip flow, one bank layout: QR parse + duplicate check 10-16; OCR fallback + confirm UI 16-26 | 26-42 | verification API not included |
| 5 | Next-month income estimate (rule-based, range) | 6-10 | |
| 9 | Descriptive analysis with LLM (masking 8-12, templates+filter 8-12, eval set 8-12, audit log 4-6, integration 4-6) | 32-48 | the safeguards are the cost |
| T | Testing hardening, documentation, PDPA notice, demo prep (not per feature) | 40-60 | f-abdul's "about 60 h" [S] sits at the top of this |

### 6.3 Totals and reconciliation with the docs

- P0 (F0, 1, 2, 3, 8, 4, 7): 30+10+6+8+24+8+14 = 100 low; 45+16+10+12+36+12+22 = 153 high. Result 100-153 h.
- P1 (10, 11, 6): 22-34 h. P0+P1: 122-187 h.
- P2 (1-slip, 5, 9): 26+6+32 = 64 low; 42+10+48 = 100 high. Adding 1b (11-19): total features 197-306 h. Plus T (40-60): 237-366 h.
  With 1b counted inside P0 as the baseline says ("market-price APIs where available"), the whole baseline is about 237-366 h. That is 1.2x to 1.8x the highest capacity figure (198 h).
- Reconciling the conflicting numbers:
  - 60-130 h (vj's build budget): equals the 121-198 h capacity minus about 60 h testing and documentation, which gives 61-138 h by arithmetic (the 130 is an 8 h typo/round-down) [S-repo: docs/consolidated/01 line 63]. It is a capacity figure (what the team can spend), not an estimate of the work.
  - 80-132 h: derived by the docs from vj's rows for the baseline P0 set with a LIFF page standing in for the dashboard (71-115 h with the chat card; 80-132 h with LIFF) [S-repo: docs/consolidated/01 line 233]. It is an estimate of P0 only, excludes features 10 and 11, and includes the old F3 row that contains slip reading.
  - 121-198 h: historical team capacity from f-abdul to 25 April 2027, not re-opened and labelled old scope by Omm [S-repo].
  - My P0 of 100-153 h is consistent with their P0 estimates (71-132 h) once foundations and a real dashboard are counted; their figures were low mainly because foundations (30-45 h) and testing (40-60 h) were counted separately or not at all.
  - Decisions file option B for capacity (gross 204-273 h from 22 Nov to 30 Apr before breaks) is the only capacity derivation from first principles; the 121-198 h range is about 45-75% of it, which is plausible after exams and breaks. I have not seen the underlying weekly-hour assumptions, so I treat 121-198 h as the usable build+test+doc capacity [I].
- Therefore the full 11-feature scope does not fit. Even P0+P1 plus testing (162-247 h) sits at or above the top of the capacity range.

### 6.4 Realistic MVP cut that yields a working demo

Principle: a demo that shows the thesis idea end to end ("my debts, when I pay, what if I pay more, explained in Thai without advice") on synthetic data.

| Item | Hours (low-high) |
|---|---|
| F0 foundations (trimmed: auth by magic link, schema, deploy, CI; no LINE) | 28-38 |
| Assets manual CRUD only (no CSV, no live prices; user-entered price with date) | 6-9 |
| Incomes (recurring + one-off) | 5-8 |
| Expenses (typed, recurring, loan link) | 6-9 |
| Engine: reducing-balance, flat-rate with effective rate, card minimum payment, rate-step mortgage, payoff quote | 20-30 |
| Due items (4) | 6-9 |
| Dashboard (simple web, totals + one debt chart + upcoming list) | 10-16 |
| Debt health (10): DSR, DTI, monthly cash margin | 6-10 |
| What-if simulator (11) | 6-9 |
| Subtotal, core demo | 93-138 |
| Testing and documentation share (about 25% of build) | 25-35 |
| Core demo total | 118-173 |
| Optional LLM explain feature (9-lite): fixed templates, masking of the few free-text fields, deny-list + number-whitelist filter, 60-case eval, audit log, disclaimer | +20-32 |
| Demo with LLM total | 138-205 |

Fits the 121-198 h capacity at the middle. Deferred (stated as future work, consistent with the baseline): automatic prices (11-19 h), CSV, slip flow (26-42 h), income estimate (6-10 h), checklist (6-10 h; the lowest-risk add if 6 h appear), statement parsing, LINE bot, LIFF.

Ordering by dependency (matches the scope spec's build order): schema, engine, due dates and dashboard, then health and simulator (working demo here), then LLM-lite, then the checklist, then the rest.

Risk register for the cut [I]:
1. Engine conventions per lender: mitigated by the "enter the actual instalment" flow.
2. LLM filter false negatives: mitigated by templates and the number whitelist; never show a free-form answer in the demo.
3. Capacity shortfall: cut the dashboard to a list + totals (saves about 6-8 h), then drop the LLM-lite item first.
4. Data sources: a demo needs no live feeds.

---

## 7. Open items needing a human or a primary-source check

1. Exact credit card minimum payment definition (floor, base) and the rate after 2026 (BOT circular).
2. Hire purchase early-payoff discount schedule (OCPB notice text) and which rebate method contracts use.
3. SEC Open Data API: current portal, key process, redistribution terms, rate limit.
4. SET data licence for redistribution (contact infoproducts@set.or.th, per the marketplace page).
5. Gold Traders Association terms for using published prices.
6. LINE Thailand official plan table, content retention period for downloaded images, LIFF limits.
7. Slip-verification provider terms and price (SCB/BBL/EasySlip).
8. Thai OCR accuracy on synthetic slips: measure, do not quote.
9. Google Cloud Vision pricing page (did not load).
10. Whether the free tier of any LLM may be used with masked data (my position: no).

## 8. Source list (all accessed 2026-11-21)

- https://platform.claude.com/docs/en/about-claude/pricing [V]
- https://aistudio.google.com/docs/pricing [V]
- https://developers.openai.com/api/docs/pricing [V]
- https://www.coingecko.com/en/api/pricing [V]
- https://frankfurter.dev/providers/bot/ [V]
- https://github.com/max180643/thai-gold-api [V]
- https://set.or.th/en/services/connectivity-and-data/data/smart-marketplace [V]
- https://www.scb.co.th/th/personal-banking/stories/tips-for-you/interest-rate-method.html [V]
- https://www.bot.or.th/th/research-and-publications/articles-and-publications/bot-magazine-issues/phrasiam-68-3/hire-purchase-leasing.html [V]
- https://arxiv.org/html/2601.14722 [V]
- https://developers.line.biz/en/docs/messaging-api/pricing/index.html.md [V]
- https://developers.line.biz/en/reference/messaging-api/ [V, navigation only]
- https://en.thairath.co.th/money/personal_finance/financial_planning/2918376 [S]
- https://www.nationthailand.com/business/banking-finance/40059264 [S]
- https://www.prachachat.net/finance/news-1368667 [S]
- https://www.thairath.co.th/money/personal_finance/finance_banking/2903481 [S]
- https://www.thaipost.net/news-update/347559/ [S]
- https://www.kasikornbank.com/th/announcement/debtsolution-measure/pages/prepayment-fee.aspx [S]
- https://www.gsb.or.th/media/2024/01/sale-sheet-สินเชื่อเคหะ.pdf [S]
- https://pkg.go.dev/github.com/jwitmann/sec-go ; https://github.com/Zummation/SEC-API ; https://secopendata.sec.or.th/Fund/MF_IPO [S]
- https://rubydoc.info/gems/bank_of_thailand [S]
- https://thai-qr-payment.js.org/guide/slip-verify/ ; https://document-partners.easyslip.com/en/v2/verify/bank [S]
- https://yespress.io/line-company-thailand [S]
- https://github.com/bitkub/bitkub-official-api-docs [V, partial]
- Repo: docs/research/2026-11-03-verify-hire-purchase.md, docs/consolidated/01-scope-features-priorities.md (lines 61-65, 233), docs/consolidated/_decisions-scope.md, docs/consolidated/_decisions-architecture.md, docs/specs/2026-11-17-ai-financial-assistant-scope.md.

## 9. Summary (10 lines)

1. Engine is feasible: 10 test vectors cross-checked two ways (exact rational vs rounded schedule; bisection vs Newton; decimal vs integer satang); engine 24-36 h.
2. Flat-rate contracts are 1.7-1.9x their flat rate in effective terms (3% flat = 5.64% nominal; 20% flat = 34.65% nominal); early payoff method (EIR vs Rule of 78 vs naive) changes the answer by up to THB 33,959 in TV-9, so label the method.
3. Day count matters: ACT/360 vs ACT/365 moved one-year interest by THB 276.77 on 500,000; make it a per-loan setting; negative-amortization is a required error case.
4. Credit card: 16% cap and 25% personal-loan cap [S], minimum 8% until end-2026 [S]; the BOT example is only approximately reproducible because the minimum definition is UNVERIFIED.
5. Price data: SET stock data is paid with a separate redistribution licence, so keep stocks and gold manual; SEC NAV, FX (BOT/Frankfurter) and CoinGecko Demo are free but need terms checks.
6. LLM cost is not a constraint: THB 0.5-2.3 per typical user per month on Haiku/Flash-Lite/Luna class models, THB 12-23 on Sonnet class; Gemini 3.5 Flash-Lite rates in Omm's file are now [V]; the real work is masking + filter + eval (32-48 h).
7. Slip QR holds a bank code and transaction reference, not the amount; amounts need OCR or a paid verification API; Typhoon OCR paper numbers are verified only as author-reported on its own benchmark, not on slips.
8. Recommended architecture: one TypeScript web app/PWA, Postgres, engine as a pure module, LINE bot/LIFF as thin optional layer; cheapest alternative a single Python app on SQLite.
9. Effort: full baseline is about 237-366 h versus capacity 121-198 h; 60-130, 80-132 and 121-198 are respectively a capacity net of testing, a P0-only estimate with LIFF, and a historical capacity.
10. Realistic MVP (core engine, due items, simple dashboard, health, simulator, typed entry) is 118-173 h, or 138-205 h with a templated LLM-lite feature; defer live prices, slips, CSV, income estimate and LINE.
