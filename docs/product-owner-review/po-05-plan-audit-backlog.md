# PO plan audit and product backlog: Thai personal balance-sheet tracker

- Date: 2026-11-21 (proposal due 2026-11-22, project due 2027-04-30)
- Role: sceptical product owner reviewing the PLAN, not the market. Read-only audit; no repo file was changed.
- Status: everything in this file that is not copied from a source is an `[I]` PO proposal needing team approval. Nothing here edits or proposes a direct edit to `CLAUDE.md`; where CLAUDE.md wording is affected, the item is phrased as "propose to the team".
- Evidence tags: copied from sources, never upgraded. Source citations: (Jay: ...) = CLAUDE.md, scope spec, direction research; (consolidated) = `docs/consolidated/*`; (f-abdul) = `f-abdul/docs/*`. Hour figures that are mine are `[I]` and are seeds for a work breakdown, not commitments.
- Data in all examples is synthetic. The persona "Nok" below is invented.

---

## 0. Read this first: the six findings that matter most

1. **The P0 list contains no AI.** P0 = features 1,2,3,8,4,7 (asset list, income, expenses, amortization, due items, dashboard). The only LLM features are 9 (P2) and slip OCR (P2). A project titled "AI Financial Assistant" whose MVP has no AI will be judged against its title. Either the title/promise changes or one AI feature must enter the MVP (my proposal: a split "explain-my-numbers" feature, section 2.3). `[I]`
2. **There is no "debts/liabilities" feature in the list of 11, yet debt is "the right center".** Feature 8 (amortization), 4, 10 and 11 all need a loan entity (lender, principal, rate, term, payment, start date, rate type), but no feature owns creating, editing, closing or valuing liabilities, and feature 7 "Dashboard: assets, incomes, expenses" omits liabilities and net worth. The balance sheet cannot balance. Needs a new P0 item (E1-S03 below). `[I]`
3. **Nothing in the repo defines success.** No problem statement with a measurable goal, no definition of done, no acceptance criteria, no hour estimates in Jay's files, no sprint plan, no roles (README team list is `<member 1..3>`), no advisor grading criteria. The only hour numbers (vj's) disagree with their own rows and rely on f-abdul capacity figures Omm says are invalid for this scope (D-S17, D-S18).
4. **Two leading authority questions block everything**: which document wins (D-S19) and whether the scope is "team approved" (CLAUDE.md) or "Draft ... not yet reviewed" (spec header) (D-S20). CLAUDE.md and the spec contradict each other on the same fact. Proposal due tomorrow.
5. **No user has been talked to.** Target user, price, retention and demand are all unvalidated (consolidated 01 section 1 item 8). Every priority below is a hypothesis until 10-15 interviews (listed as open in Jay's spec section 8) are done.
6. **Domain risk the plan has not noticed `[I]`**: the amortization engine is specified generically, but many Thai consumer loans quote *flat* rates (car hire-purchase is the canonical case), mortgages often have stepped or floating rates, and credit cards/personal loans use minimum-payment rules. A reducing-balance engine fed a flat-rate quote produces wrong "interest" numbers and breaks the "code computes every number, trust it" promise. This needs a rate-type field and a golden-test set before feature 8 is called done. (Sources do not cover Thai loan-rate conventions; this is general knowledge, UNVERIFIED for Thai specifics and must be checked with lender documents or a bank calculator.)

---

## 1. Gap analysis: what a buildable plan needs

Legend: MISSING = no content anywhere; PARTIAL = mentioned, not buildable; OK = adequate for now.

| # | Item | State | What exists | What is missing (PO proposal `[I]`) |
|---|---|---|---|---|
| 1 | Problem statement | PARTIAL | One-sentence product (spec section 1); household debt context `[S]` (direction section 7) | A problem statement in the user's words: e.g. "People with 2+ loans cannot say what they owe, what is due when, and whether next month's cash covers it." Needs interview evidence; current demand evidence is "weak" (direction section 7). |
| 2 | Measurable goals | MISSING | None | Proposed goals (hypotheses, thresholds to be agreed before testing, as Omm's board also asks): G1 first balance sheet in <= 10 min for a new synthetic-persona tester; G2 net worth and cash margin match hand calculation to the baht on 100% of golden cases; G3 0 forbidden phrases in 200+ AI outputs from the eval set; G4 >= 4/5 test users can say "what is due in the next 30 days" correctly from the app; G5 SUS >= 68 on 5-8 testers. |
| 3 | Target user decision | PARTIAL | Open: loan-holders vs investors vs students vs payday salaried (D-S4, D-M2) | One decision, one persona, one "job to be done". Proposal in section 2.2. |
| 4 | MVP boundary / definition of done | MISSING | Priority tiers only; build-order says "a working demo exists after step 4" (spec section 6) | Written MVP statement and per-story DoD (section 3.4). |
| 5 | User stories and acceptance criteria for all 11 features | MISSING | One-line feature rows with notes | Section 3 writes stories for all 11 (Given/When/Then for P0/P1). |
| 6 | Data model and glossary | PARTIAL | Omm's logical entity list for the LINE/bill design (consolidated 02 section 7); Jay names entities only at feature level | Glossary and valuation rules in section 1.1 below. No schema for assets, liabilities, loan terms, recurring items. |
| 7 | Currency, locale, calendar | MISSING | None | THB only; integer satang or Decimal with explicit currency (Omm already says exact decimals, consolidated 02 section 7); Asia/Bangkok; Thai Buddhist-era (BE) year display and input; Thai numerals optional. |
| 8 | Recurring items | MISSING | "Recurring expenses" mentioned (feature 4); Omm: expected items kept separate from confirmed transactions | Recurrence model (frequency, anchor day, end, month-end rule for day 29-31, non-banking-day rule, skip/pause). |
| 9 | Onboarding and empty state | MISSING | None | Section 1.2. The app is useless until 3 data types are entered; first-session drop-off is the main product risk for a manual-first tracker (D-S2; Omm: "second job" risk). |
| 10 | Error and correction flows | PARTIAL | Omm: undo/edit, confirmation policy open (D-A6) | Edit/delete/restore of any record; "backdated change recalculates schedule"; duplicate detection; correcting a wrong loan rate. |
| 11 | Security and account model | MISSING for this scope | Omm architecture L-level items (isolation, encryption) not in Jay's docs; f-abdul charter F01/F06 is for the old product | Decide auth (LINE Login vs email/OTP vs local-only), per-user isolation, encryption at rest, secrets handling, backup. D-A1 (channel) decides this. |
| 12 | Data deletion / export | PARTIAL | PDPA duty noted; no retention period anywhere (consolidated 00) ; D-A7 open | Delete-account and export user stories, a retention table, audit-log treatment. |
| 13 | Accessibility and Thai UX | MISSING | None | Thai-first copy, font and number rendering, BE dates, large tap targets, colour not sole signal in charts, screen-reader labels, plain-language terms (e.g. "ดอกเบี้ย" explained inline). Section 1.2. |
| 14 | Notification / reminder design | PARTIAL | Feature 4 "items due in X days"; Omm reminder states (arch s.7, D-S15); f-abdul reminder engine (reuse unresolved, D-S21) | Is a reminder a push or only an in-app list? Channel decides cost and complexity. Proposal: in-app "Due soon" list in MVP; push only if LINE is chosen and after freeze. |
| 15 | Test and evaluation plan | MISSING | Omm acceptance scenarios for safeguards (consolidated 02 s.4.4); "OCR test set" listed open | Section 1.3: unit/golden tests for the engine, AI eval set, output-filter tests, usability tests. |
| 16 | AI output eval set and output filter spec | MISSING | Filter is named as "implementation note" only (spec section 4) | Section 1.3 gives a concrete spec (denylist plus allowlist templates, Thai + English, tests). |
| 17 | Analytics | MISSING | Board mentions retention/first-use measures (consolidated 03 s.9) | Minimal, privacy-safe event list and 5 metrics, section 1.4. |
| 18 | Demo script for final presentation | MISSING | None | Section 5.4. |
| 19 | Grading / advisor criteria alignment | MISSING | The advisor's only recorded input is "too wide, pick one field" (progress 2026-11-17) | Ask the advisor for the rubric (question Q3). Provisional mapping in section 5.5. |
| 20 | Team roles | MISSING | README placeholders; f-abdul charter has an unassigned role structure (old product) | Propose 3 roles in section 4.5. |
| 21 | Timeline with milestones and risks | MISSING for new scope | f-abdul timeline is for the old product; "Dates and capacity still apply" (f-abdul docs README) | Sprint plan in section 3.6, risks in section 4. |
| 22 | Dependency map | PARTIAL | Spec: 8 feeds 4, 10, 11; build order | Full map in section 2.4 (includes the missing liability entity and consent/privacy gates). |
| 23 | Legal gate | PARTIAL | SEC and PDPA items UNVERIFIED, listed open | Make them dated gates with owners (section 5.2, Q2). |
| 24 | Name, team names, approver | MISSING | CLAUDE.md approver `<TODO>`, README members `<member n>` | Blocks the "team agreement" mechanism CLAUDE.md relies on (Q4). |

### 1.1 Proposed glossary and valuation rules `[I]` (for team approval)

| Term | Proposed definition | Notes |
|---|---|---|
| Asset | Anything the user owns with a positive current value, entered with a *value, as-of date and value source*. Types for MVP: cash/deposit, investment (stock/fund/gold/crypto), property, vehicle, other. | Spec lists stocks, crypto, forex, CSV import under feature 1; keep MVP to manual value + as-of date (D-S7 option B). |
| Liability (debt) | An obligation with an outstanding principal and a payment schedule or minimum payment: mortgage, car hire-purchase, personal loan, credit card, student loan, informal debt. | Must carry rate type (reducing balance / flat / minimum-payment / zero), because the engine differs. |
| Net worth | Sum of asset values minus sum of outstanding liability principal, as of a date, with a "data completeness" and "oldest valuation" line beside it. | Vj and Omm both require stating completeness (consolidated 01 s.1 item 12). |
| Monthly income | Confirmed recurring income normalised to one calendar month (salary, rent, dividends). Irregular income excluded unless the user marks it recurring. | Keep "expected" and "received" separate (Omm arch L310). |
| Monthly cash margin | Monthly income minus recurring expenses minus scheduled debt payments. Negative = shortfall. | Source cites "negative cash margin" for 23.8% of indebted households `[S, secondary report]` (direction s.7); our formula must be shown in-app and may not match that report's definition. Must say so. |
| Debt service ratio (DSR) | Scheduled monthly debt payments / monthly income. Choose gross or net income once and label it. | Source figure "40.1% of income on debt service" is `[S]`, definition of income not confirmed. |
| Debt-to-income (DTI) | Total outstanding debt / annual income. | Two ratios exist in the spec (feature 10) but neither is defined; pick definitions before coding. |
| Valuation: property, vehicle | User-entered estimate with date; app never auto-values and never depreciates silently. Stale warning after N days (proposal 180). | Direction s.9 lists "real estate and vehicle valuation sources" as a gap. |
| Valuation: listed stock, fund, gold, crypto | MVP: quantity x user-entered unit price with date. Live price feed is Could (licence for SET data UNVERIFIED, vj). | |
| Currency | THB only in MVP; foreign-currency holdings entered in THB-equivalent with the rate used recorded. | Feature 1 mentions forex; park it. |
| Recurring item | Template (amount, frequency, anchor day, start/end, category, kind=income/expense/debt payment) that generates *expected* entries; the user confirms *actual* ones. | Month-end: day 31 maps to last day of shorter months. |
| Due item | A dated obligation (loan instalment, recurring expense) within the next X days, status expected / paid (user-marked). | "Paid" by user tap only in MVP (slip matching is P2, D-S15 option A). |

### 1.2 Onboarding, empty state, Thai UX `[I]`

- First-run flow: 1) disclaimer and consent (informational only; data use; LLM masking), 2) "Add your first debt" (the differentiator, ~2 min), 3) add monthly income, 4) add 3 recurring expenses, 5) add assets (skippable), 6) first dashboard.
- Empty dashboard shows three cards, each with one action button and a "load synthetic sample data" link (also the demo and tester path; no real data needed).
- Net worth with no assets shows "Not enough data" with the completeness bar, never 0 baht.
- Progressive entry: a loan can be saved with only balance and monthly payment ("basic mode") and refined to rate/term later; indicators show "estimate" until refined.
- Thai UX checklist: Thai default language, BE years, baht formatting (1,234.50), no jargon without a tap-to-explain, no red-only negatives, minimum 44 px targets, test with Thai screen reader labels, avoid financial-shaming wording (e.g. "negative cash margin" phrased neutrally).

### 1.3 Test and evaluation plan `[I]`

| Layer | What | Pass bar (to agree before testing) |
|---|---|---|
| Engine unit/golden tests | 30+ synthetic loan cases (reducing, flat, zero-rate, balloon, extra payment, early payoff, leap year, day-31 start); expected values computed independently (spreadsheet) | 100% match to the satang |
| Property tests | Sum of principal paid = original principal; balance never negative; schedule monotone | all pass |
| Indicator tests | DSR, DTI, cash margin on 20 synthetic households incl. zero income, no debt | no divide-by-zero crash; defined text for edge cases |
| AI eval set | 150+ prompts/contexts: 100 benign explain tasks, 50 adversarial ("should I sell my fund", "which loan is better", "is this a good time", Thai and English, indirect phrasing) | 0 forbidden outputs reaching the user; explanation numbers equal engine numbers in 100% (automated check that every number in the text appears in the supplied facts) |
| Output filter spec | Two layers: (a) structured output: the LLM fills a fixed template with slots only; (b) post-filter: denylist of Thai/English patterns (buy, sell, switch, recommend, "should", "ควรซื้อ", "ควรขาย", "แนะนำ", target price, product names) plus a number-whitelist check. Block = replace with a safe fallback text and log. | filter unit tests: 100% of a labelled red-team list blocked; false-block rate on benign set reported |
| Masking tests | Synthetic PII corpus (names, 10-digit accounts, phones, addresses, 13-digit IDs, Thai and Latin script); assert no raw identifier in the logged outbound payload | 0 leaks |
| Usability | 2 rounds, 5 testers each, scripted tasks on synthetic data | G4/G5 |
| Accessibility | automated audit plus manual keyboard/screen-reader pass | no blocker |
| Regression | CI runs engine + filter + masking tests on every merge | green before demo |

### 1.4 Analytics (privacy-safe) `[I]`

Events (no amounts, no identifiers): onboarding_step_completed, first_loan_added, first_dashboard_viewed, due_item_marked_paid, whatif_run, ai_summary_requested, ai_summary_blocked_by_filter, data_exported, account_deleted. Metrics: time to first dashboard, % completing onboarding, 7-day return, % AI outputs blocked, tasks completed in usability tests. Stored locally/in own DB; no third-party analytics SDK until PDPA review.

---

## 2. Contradictions and weak logic in the plan

### 2.1 Internal contradictions

| # | Issue | Evidence | Why it matters | PO fix `[I]` |
|---|---|---|---|---|
| C1 | CLAUDE.md says decisions "team approved"; the spec header says "Draft ... Not yet reviewed by the full team or the advisor" | CLAUDE.md vs spec L4 (D-S20) | Weight of the whole baseline | Hold a 30-min review and record the result in both places (propose to the team; CLAUDE.md edit only by team agreement). |
| C2 | P0 has no AI; product is titled and pitched as AI-based | spec section 3 | Title/promise mismatch; fails expectations of an AI-themed project `[I]` | Add AI "explain" slice to MVP (section 2.3) or rename the claim. |
| C3 | No liability entity or feature; dashboard omits liabilities | spec features 1-11 | Net worth impossible; 8/4/10/11 have no input | Add E1-S03 liabilities CRUD as P0. |
| C4 | Feature 4 "reads the loan schedule from 8 and recurring expenses", yet 8 is only an engine and recurring items have no feature | spec s.3 | Hidden dependency: recurring-item model | Add recurring-item model inside E1 (income/expense/debt payment share it). |
| C5 | Feature 7 (dashboard) is P0 but a *useful* dashboard needs 8 (liabilities), 4, and later 10 | spec s.3 | Dashboard built before data exists shows nothing | Build dashboard last in P0, as a view over existing entities; accept two iterations (net worth first, indicators after). |
| C6 | Feature 10 value depends on complete income AND expense AND debt entry; feature 3 (expenses) is the most effortful manual input | spec s.3; D-S2 | The headline differentiator depends on the highest-friction input | Let F10 degrade gracefully: DSR and DTI need only income + debts (no expenses); cash margin needs expenses and is shown only when expenses are entered. Reorder: DSR/DTI first. |
| C7 | Feature 6 "not-yet-owner checklist" is P1 although mortgage steps are unresearched (spec s.8) and vj says a mortgage buyer already owns the house for some loans `[I]` | spec s.3; D-S8 | P1 feature built on a research gap; low reach (RICE ~1) | Demote to Could; car hire-purchase only; rename "financed-asset checklist". |
| C8 | Feature 5 (income estimate) needs history of features 2 and 3; in a 4-month pilot with few users there is no history | spec s.3 | Cannot be demonstrated honestly except on synthetic data | Keep P2; demo only on a seeded synthetic history and say so. |
| C9 | Feature 9 is "LLM analysis of the user's own assets" (portfolio commentary) and is exactly the part near the SEC line; but the debt-side explanation (loans) is outside securities | direction s.5 | Legal risk concentrated in a P2 feature, while the safe, valuable explanation is not scoped | Split F9 (section 2.3). |
| C10 | Feature numbering: "1" appears twice (Asset list in P0, "Slip OCR" row labelled 1 in P2); consolidated doc renamed it 3a | consolidated 01 s.5.1 | Traceability errors | Use 1 (assets) and 12 (slip OCR) in the backlog. |
| C11 | "Manual input first" (Jay) vs "without typing it in" (vj) | D-S2 | Defines the product promise and MVP evaluation | Recommendation in 2.5. |
| C12 | Bank API "later upgrade" vs Omm's three-path ingestion with email/API in the build sequence | D-S16, D-A2 | Largest scope-creep vector (Gmail verification) | Exclude (recommendation 2.5). |
| C13 | Hour figures: vj Must 55-77 stated vs rows 49-70; stated tiers 140-219 vs rows 124-199; capacity 121-198 h reused from f-abdul though Omm says invalid; "130 h" vs arithmetic 138 h | consolidated 00 and 01 s.2.2 | Every "fits in capacity" claim is unsafe | Re-estimate bottom-up (section 3.5). |
| C14 | Scope creep from the other authors: splits/IOUs, ask-about-money, streaks, payment links, email ingestion, voice, model tiers/personalities, subscription module | D-S12-D-S16, consolidated 01 s.5.2 | Roughly +40-80 h of unestimated work vs 150-200 usable hours `[I]` | Park all behind MVP freeze (recommendations 2.5). |
| C15 | LINE-only (Omm, owner confirmation 18-19 Nov) vs channel-neutral baseline; "owner" role vs "advisor" role undefined | consolidated 01 s.2.1 note | A channel mandate changes auth, UI, cost, and 15-25 h | Q1: who is the owner, is LINE a requirement? |
| C16 | Safeguard 4 "audit log of AI outputs" vs PDPA deletion; no retention period in any source | D-A7; consolidated 00 | Compliance gap on day one of storing data | Decide before any real-user data (recommendation D-A7). |
| C17 | Disclaimer says "not advice", but a paid subscription may supply the "fee" element; revenue model is a legal input | direction s.5 `[S]`; D-M5 | Pricing choice can change legal status | Keep pilot free until legal check. |
| C18 | Debt-health thresholds: any "healthy/unhealthy" label for DSR is a judgement about the user | spec s.4 (no suitability) | A traffic-light on DSR may breach "descriptive only" in spirit | Show the ratio and a neutral reference line sourced and tagged, never "good/bad/you should". Needs legal wording check. `[I]` |

### 2.2 Value that depends on an unbuilt thing

- Dashboard (7) depends on liabilities (missing), 8, 4. 
- 4 depends on 8 (loan schedule) and the recurring model.
- 10 depends on 2, 3, 8 (and income/debt completeness). 
- 11 depends on 8 only; cheapest P1 item.
- 9 depends on masking, filter, audit log, consent, provider terms, legal check: about 40% of its effort is not "the feature". 
- Slip OCR depends on D-A4 (QR payload cannot be masked), consent, test set, matching logic, and F4 for marking bills paid.

### 2.3 Proposed split of feature 9 `[I]` (PO proposal, team approval needed)

- **9a "Explain my numbers" (MVP, P1):** LLM turns code-computed facts about the user's debts, due items, cash margin, and payoff what-if into plain Thai from a fixed template with slots. Facts-only, no comparisons across products, no suitability. Low legal risk because loans/cash flow are not securities (direction s.5 `[I]`), though a lawyer check is still needed.
- **9b "Asset spread summary" (P2):** allocation/concentration commentary on investments. The part nearest the SEC advice definition (direction s.5 `[S]`/`[I]`); after legal check.
This gives the MVP a genuine, safe AI feature that reuses the safeguards needed anyway.

### 2.4 Dependency map `[I]`

```
Decisions D-S19/20 -> D-S1/D-S4 (target user) -> interviews
Foundation (repo, CI, DB, auth, deploy, consent screen)
  -> Data model: user, asset, liability(loan terms), income, expense, recurring template
     -> E1 CRUD (assets 1, income 2, expenses 3, liabilities NEW)
     -> Loan engine 8  -> due items 4  ----------> Dashboard 7 (net worth, cash margin, due list)
                       -> what-if 11
        income+debt -> DSR/DTI (10)  ; +expenses -> cash margin (10)
Masking + output filter + audit log + provider terms check + legal check
     -> 9a explain-my-numbers  -> (later) 9b asset summary
Privacy: consent, export, delete, retention -> gate for ANY real user data
Stretch: CSV import (1), slip QR demo (12), income estimate (5), checklist (6) -> all after MVP freeze
Parallel: interviews -> persona -> priorities; eval set -> filter; usability tests
```

### 2.5 Open decision log: PO recommendations (all PROPOSALS needing team approval)

Authority and approval questions come first; the rest depend on them. "Change if" states the evidence that would flip the recommendation. Entries that restate another entry say so.

| ID | Question | PO recommendation `[I]` | Reasoning | Evidence that would change it |
|---|---|---|---|---|
| D-S19 | Which source outranks | Option C: team votes to ratify one order; until the vote use A (Jay's baseline) as default, and treat Omm/vj/board items as proposals | The baseline was explicitly decided by the team (CLAUDE.md); later documents label themselves proposals; but if the "owner" has authority over the grade, B may be binding | Q1 answer: if the owner/advisor is the approver and has mandated LINE or bills-first, adopt B for those points |
| D-S20 | Approved or draft | Option C: hold a short review with all 3 members, record the result in the spec header; propose to the team that CLAUDE.md wording be aligned afterwards (do not edit directly) | Two files disagree on the same fact | n/a (process) |
| D-S1 / D-M1 | Product framing | A balance-sheet tracker with debt at the centre; "what is due soon" (option C's job) is a first-visible screen inside it, not a separate product | Advisor wanted one narrow field; F4 is already P0; MeowJot already advertises slips/categories, so a bill-first LINE product meets the strongest competitor head-on (consolidated 03 s.3, `[V]` page check opened 2026-11-20 per Omm) | Interviews show payday-bills pain dominates and loan-holders will not enter loan data |
| D-S2 | Manual vs "without typing" | C: manual-first build, "without typing it in" kept as stated direction and future work; measure time-to-first-dashboard and abandonment | 3 people x 3-4 h/week cannot deliver a reliable capture pipeline plus the balance-sheet core `[I]` | Interviews show entry effort is the first reason to quit: then pull a QR-only slip flow forward |
| D-S3 / D-A1 | Channel and dashboard | C: channel-neutral API; first client a mobile-first web app (LIFF-compatible later); LINE Flex summary card as a post-freeze Should | Balance sheet with tables and a schedule is poor in chat; 15-25 h LIFF vs 6-8 h card (vj hours `[I]`); owner mandate unknown | Q1: if LINE is mandatory, do LIFF app + card and drop push |
| D-S4 / D-M2 | Target user | A: salaried workers with loans (payday-bill salaried adults overlap); investors later | Loan maths is deterministic, demo-able, and debt is the researched centre (direction s.7 `[S]`); investor side needs price licensing (vj, UNVERIFIED) | Interviews: loan-holders do not track or will not participate; investors easier to recruit |
| D-S5 / D-A3 | Slips as P2 or Must | A with a go/no-go: QR-only, one bank format, as a demo flow in sprint 8 if the MVP froze on time | Largest single estimate (25-35 h, vj `[I]`); D-A4 masking conflict; needs OCR test set that does not exist (direction s.4 UNVERIFIED) | Interviews show typing is the top abandonment reason, or a 1-day spike hits the agreed QR-read rate |
| D-S6 | Amortization P0 or Should | C: P0, reducing-balance first, with rate-type field and flat-rate converter flagged as a known limitation | Features 4, 10, 11 and the dashboard depend on it | Interviews show most users cannot supply rate/term (then basic-mode loans dominate) |
| D-S7 | Asset list priority | B: P0 manual entry with value + as-of date; live prices and CSV Could | Licence and API-key security cost (vj `[I]`/UNVERIFIED) buys little for loan-led user | Investor segment chosen |
| D-S8 | Ownership checklist | B: Could, car hire-purchase only, after MVP | RICE ~1; mortgage research gap (spec s.8); some mortgagors already own the property (vj `[I]`) | Interviews show document/transfer confusion is a top pain |
| D-S9 | Income estimate vs money left | Defer. Implement F10 monthly cash margin (historical, from entered data) in P1; revisit forecast as Could | The "money left" forecast needs months of history we will not have; cash margin gives most of the value | Pilot data after 2 payday cycles shows stable recurring patterns |
| D-S10 | Feature 9 tier | C, plus the 9a/9b split: 9a explain-my-numbers in MVP after the safeguard stack; 9b after legal check | Gives the MVP an AI feature on the safer (debt) side | Lawyer/SEC reply says even debt explanation needs special handling |
| D-S11 | Features 10 and 11 | A: keep both, estimate separately (about 7 h each `[I]`) | They are the differentiators over MeowJot-style expense apps (debt maths) | Early test: users ignore indicators |
| D-S12 | Splits/IOUs | A: out of scope | Social features excluded by the baseline; +10-15 h (vj `[I]`) | Interviews: shared expenses are the top need |
| D-S13 | Ask about your money | C later: fixed query templates, Could, post-freeze | Widens LLM data exposure; 8-12 h (vj `[I]`) | 9a goes well and time remains |
| D-S14 | Streaks | A: out of scope; if retention is a problem, use milestones after MVP | Baseline excludes gamification; unevidenced | Pilot return rate poor |
| D-S15 | Payment links | A: status tracking only, no payment links | "Bill payment" is out of scope; link upkeep is ongoing work | Interviews show paying friction is the pain |
| D-S16 / D-A2 | Email/API/batch import | A: exclude; future-work text only | Gmail restricted-scope verification and coverage testing could dominate effort (D-S16 why-it-matters) | Owner mandate; a coverage test shows an easy path |
| D-S17 | Which hours stand | C: re-estimate bottom-up (section 3.5), foundations counted once | All existing figures are internally inconsistent | n/a |
| D-S18 | Capacity basis | B: derive fresh capacity from calendar, with an explicit exam/holiday table and a 25% coordination allowance (the f-abdul timeline's own method) | Gross 204-273 h; after 25% allowance 153-205 h before exams `[I]` (arithmetic: 9-12 h/week x 22.7 weeks x 0.75) | Team supplies real availability |
| D-S21 | Reuse f-abdul | C: reuse only governance/process templates and privacy/LINE findings after re-opening; no code or reminder engine assumed | f-abdul scope is superseded; Omm says historical (D-S21) | A component is re-opened and shown to fit |
| D-A3 | Slip ingestion | see D-S5 | | |
| D-A4 | Verification call and QR masking | C: defer any verification service until after pilot; QR payload holds account data (vj) | Conflicts with "mask before external call" | Provider terms clear plus consent flow built |
| D-A5 | PDPA class of financial data | A: treat as sensitive (explicit consent) until a lawyer says otherwise | Sources disagree `[S, conflicting]`; the cost of over-protecting is a consent screen | Lawyer opinion |
| D-A6 | Confirmation policy | A for MVP: every write is an explicit form submit; C later | Volumes are small and entry is manual; AI-derived entries only exist in stretch | Parser quality measured high (stretch phase) |
| D-A7 | Audit log vs deletion | C: keep AI-output log without user linkage after deletion, disclosed; log stores masked inputs only; fixed retention proposal 90 days for raw logs, to be confirmed by lawyer | Satisfies safeguard 4 and deletion; no source gives a period (consolidated 00) | Lawyer says logs are personal data requiring deletion |
| D-A8 | Provider gating | Split: benchmark models on synthetic data now (no PDPA exposure); hard gate = terms/no-training/cross-border check before any real-user data reaches a provider | Order in sources is undefined; synthetic benchmarking is risk-free | Provider terms unacceptable: switch provider or self-host |
| D-A9 | LLM role | A: strict extract-and-explain; code selects data and actions | Matches CLAUDE.md "LLM only extracts and explains"; smallest masking/audit surface | n/a |
| D-A10 | Image extraction method | C: QR and typed text only until redaction is proven on a test set | No source settles OCR-then-mask vs redact-then-vision | OCR spike results |
| D-M3 | Price | C: no price until a willingness-to-pay test; pilot free | No demand evidence; price may change the SEC analysis | Paid-pilot commitments from interviewees |
| D-M4 | MeowJot prices regular or promo | A (label 79/149 regular, 499/799 possibly promotional, `[S]` UNVERIFIED) and move on | Low impact on a student build; do not spend hours | Checkout terms test is cheap if someone is already on the site |
| D-M5 | Paid vs free | B: free during the academic pilot; paid decided after legal check | Direction s.5: fee element UNVERIFIED | Lawyer reply |
| D-M6 | Competitive edge wording | C with A: rewrite the edge around balance sheet + loan maths + descriptive explanation; drop cross-bank capture as an edge; never say "no competitor" until the Thai-language/app-store check is done (CLAUDE.md rule) | MeowJot advertises cross-bank slips `[V]` per Omm | Competitor check finds a Thai balance-sheet app with loans |
| D-M7 | Tag for page check | A: `[V]` only where URL + access date are logged and the page was opened; otherwise `[S]` | Matches repo-conventions | Jay re-checks pages |
| D-M8 | Streaks/splits/links | A: out of scope for MVP; list as post-pilot upgrades | See D-S12/14/15 | Pilot retention data |
| D-M9 | Board scores | C: drop from outward material; if cited, state "simulated opinion, bills-first concept only" | Simulated consumers and board are not demand evidence (consolidated 01 s.1 item 13); no cited source for the 3.3/10 comparison | A real board/advisor review |

Note on D-S19 consequence: if team picks B, section 3 backlog must be rebuilt around bills-before-payday; the stories in E3 (due items) would move to the front and E4 (debt indicators) would drop to later.

---

## 3. Prioritised product backlog

### 3.1 Persona for stories `[I]` (synthetic, hypothesis until interviews)

"Nok", 29, salaried office worker in Bangkok, take-home about 32,000 baht (synthetic), a car hire-purchase, a condo mortgage, a credit card balance, a small savings account and some mutual funds. Pain: "I do not know my total debt, or whether I can cover next month's payments."

### 3.2 Epics

| Epic | Contents | Features |
|---|---|---|
| E0 Discovery and decisions | Interviews, decision votes, legal questions, advisor rubric | none (gates) |
| E1 Balance-sheet core | Data model, assets, liabilities (NEW), income, expenses, recurring items | 1, 2, 3, NEW |
| E2 Loan engine | Amortization, rate types, schedule | 8 |
| E3 Due items and dashboard | Due list, net worth, cash margin, completeness | 4, 7 |
| E4 Debt insights | Indicators, what-if | 10, 11 |
| E5 Safe AI explain | Masking, templates, filter, audit log, 9a | 9a |
| E6 Privacy and account | Auth, consent, export, deletion, retention, disclaimer | cross-cutting |
| E7 Onboarding and Thai UX | First-run, empty states, accessibility, BE dates | cross-cutting |
| E8 Checklist | financed-asset checklist | 6 |
| E9 Stretch | CSV import, live prices, slip QR demo (12), income estimate (5), asset summary 9b | 1, 12, 5, 9b |
| E10 Quality and delivery | Test suites, eval set, usability tests, demo, final report | cross-cutting |

### 3.3 Stories with acceptance criteria (Given/When/Then) for P0 and P1

Conventions: IDs `E<epic>-S<nn>`. MoSCoW in column "M". P0/P1 stories have full GWT; others have a one-line story.

**E1 Balance-sheet core (P0, Must)**

- **E1-S01 Record an asset.** As Nok, I want to add an asset with value and date, so that my net worth includes what I own.
  - G: no assets exist. W: I add "Savings account", type cash, value 150,000, as-of date today. T: it appears in the asset list and net worth increases by 150,000.
  - G: an asset valued more than 180 days ago. W: I open the list. T: it shows a "value may be out of date" marker.
  - G: I enter a negative or non-numeric value. W: I save. T: save is rejected with a plain-Thai error and nothing is stored.
- **E1-S02 Record income.** As Nok, I want to add salary as a recurring monthly income, so that cash margin and ratios use it.
  - G: no income. W: I add salary 32,000, monthly, day 25. T: an expected income entry exists for each month and monthly income = 32,000.
  - G: day 31 chosen, February. W: the month is generated. T: the expected date is the last day of February.
  - G: an irregular bonus. W: I add it as one-off. T: it is excluded from monthly income and shown separately.
- **E1-S03 Record a debt (NEW).** As Nok, I want to add a loan with its terms, so that the app knows what I owe and when.
  - G: no debts. W: I add a car loan: lender label "Lender A", principal 400,000, rate 2.5% flat, 60 months, payment 8,500, start date. T: it is saved with rate type "flat" and outstanding principal computed by code.
  - G: I know only balance and monthly payment. W: I save in basic mode. T: the debt is stored, flagged "estimate", and excluded from interest-based outputs.
  - G: a debt exists. W: I mark it closed. T: it leaves net worth and due items; history retained.
- **E1-S04 Record expenses.** As Nok, I want to log recurring and one-off expenses, so that I see monthly outflow.
  - G: I add rent 9,000 monthly. W: month starts. T: it appears as expected in the due list and in monthly recurring expenses.
  - G: a one-off 250. W: saved. T: counted only in its month; editable and deletable.
- **E1-S05 Edit, delete, undo.** (Must) As a user, I want to correct or delete any record. G: any record. W: I edit its amount. T: all derived numbers update and the change is shown with previous value for one session undo. 
- **E1-S06 Recurring template** (Must). Shared model for S02/S04/E2 payments; GWT as S02 plus pause/end.

**E2 Loan engine (P0, Must)**

- **E2-S01 Schedule.** As Nok, I want the principal/interest split of each payment, so that I know what portion is interest.
  - G: reducing-balance loan 1,000,000 at 5% for 120 months (synthetic). W: schedule generated. T: payment, per-month interest and principal match the independent golden values to the satang, and total principal paid equals 1,000,000.
  - G: flat-rate loan. W: generated. T: the app labels the effective rate, shows both flat and effective if computable, never silently treats flat as reducing.
  - G: zero-rate loan. W: generated. T: interest 0 on every row, no division error.
- **E2-S02 Past-date handling.** G: start date 14 months ago. W: I open the schedule. T: payments before today are shown as scheduled-past with outstanding principal at today computed from the schedule, marked "estimate unless confirmed by the lender statement".
- **E2-S03 Correct a loan.** G: I discover the rate was wrong. W: I change it. T: schedule and every dependent number recalculate; audit shows change.
- **E2-S04 Rate types** (Should): floating and step rates entered as rate periods.

**E3 Due items and dashboard (P0, Must)**

- **E3-S01 Due in X days.** As Nok, I want to see what is due in the next X days or by month-end, so that I can keep cash ready.
  - G: today is the 20th; loan payment on the 25th, rent on the 1st. W: I choose "next 10 days". T: only the loan payment shows, with amount, date and days left.
  - G: "by month-end". W: selected. T: items up to the last day of this month appear and total is shown.
  - G: an item is marked paid. W: refreshed. T: it moves to paid and the total drops.
- **E3-S02 Net worth card.** G: assets and debts entered. W: I open dashboard. T: net worth = assets minus outstanding principal, with "as of" date, completeness note (e.g. "2 of 3 sections filled"), and oldest-valuation date.
- **E3-S03 Cash margin card** (see E4-S02 for formula). 
- **E3-S04 Disclaimer on every screen with AI text or ratios.**

**E4 Debt insights (P1, Should)**

- **E4-S01 DSR and DTI.** As Nok, I want my debt ratios, so that I understand my debt load.
  - G: income 32,000 and scheduled payments 14,000. W: dashboard computed. T: DSR displays 43.75% with its formula in a tap-to-see note.
  - G: no income entered. W: computed. T: ratio shows "need income" and no number; no crash.
  - G: any ratio. T: no "good/bad" label and no advice text; neutral reference line only if sourced and tagged.
- **E4-S02 Monthly cash margin.** G: income 32,000, recurring expenses 11,000, debt payments 14,000. W: computed. T: margin 7,000. G: negative result. T: displayed in neutral wording, with the three inputs.
- **E4-S03 What-if payoff.** As Nok, I want to see the effect of paying extra, so that I understand the interest/time trade.
  - G: golden loan. W: I enter extra 2,000 per month. T: new end date and total interest saved match engine values; original schedule unchanged until I save a scenario.
  - G: extra larger than balance. T: capped at payoff, message explains.
  - T: output is phrased as a calculation result ("with these inputs, the loan ends N months earlier"), not "you should".

**E5 Safe AI explain (P1 for 9a, Should; filter and masking are Must prerequisites)**

- **E5-S01 Masking layer.** G: payload with names, 10-digit account, phone, address, 13-digit ID. W: sent to the LLM client. T: outbound payload contains placeholders only; mapping stays server-side; outbound payload logged.
- **E5-S02 Template + filter.** G: any AI request. W: model returns text. T: filter passes it only if (a) it fits the template, (b) all numbers appear in the facts, (c) no denylist match; otherwise a safe fallback shows and the event is logged.
- **E5-S03 Explain my numbers (9a).** As Nok, I want a plain-Thai summary of my debt situation, so that I can understand it. G: computed facts for Nok. W: I tap "Explain". T: a 3-6 sentence summary appears, every number matches the dashboard, with the informational-only disclaimer.
- **E5-S04 Adversarial input.** G: a free-text request "which loan should I close". W: submitted. T: the app returns the refusal/neutral text; no recommendation reaches the user; logged.
- **E5-S05 Audit log.** G: any AI output. T: stored with timestamp, masked input, output, filter verdict, model id.

**E6 Privacy and account (Must; gate for real data)**

- **E6-S01 Consent and notice.** G: first launch. W: I continue. T: I must accept a privacy notice that names the LLM provider and cross-border transfer, and can use the app without AI if I decline AI consent.
- **E6-S02 Export** (Should→Must before pilot). G: signed-in user. W: I tap export. T: I receive all my records in a machine-readable file.
- **E6-S03 Delete account and data.** G: signed-in user. W: I confirm deletion. T: all records, attachments and mapping are deleted; AI log is unlinked per the D-A7 decision; confirmation shown.
- **E6-S04 Auth and isolation.** G: two users. W: A requests B's record id. T: access denied (tested).
- **E6-S05 No real data in the repo** (CI check for obviously real identifiers in fixtures).

**E7 Onboarding and Thai UX (Must)**

- **E7-S01 First-run flow** (see 1.2). G: new user. T: reaches first dashboard in <= 10 min on the scripted test.
- **E7-S02 Sample data toggle.** G: new user. W: tap "try with sample data". T: synthetic household loads; clearly labelled; one-tap removal.
- **E7-S03 Thai-language and BE dates.** G: Thai locale. T: dates and numbers formatted per convention; all screens have Thai copy.
- **E7-S04 Accessibility pass** (Should).

**E8 Checklist (Could)**: E8-S01 As a car-loan holder I want a document checklist for closing my hire-purchase, so that I know what to prepare (sources and dates shown; the app files nothing). 

**E9 Stretch (Could/Won't-this-time)**: CSV import of assets; live prices; slip QR demo (one bank, user confirms, OCR-only excluded from totals); income estimate range; 9b asset summary after legal check; LINE summary card.

### 3.4 MVP definition and definition of done `[I]`

**MVP (target freeze 28 Feb 2027):** a signed-in user who has consented can (1) add debts, assets, income, expenses and recurring items; (2) see net worth with completeness, monthly cash margin, DSR and DTI; (3) see items due in X days or by month-end; (4) run a what-if payoff on any loan; (5) tap "Explain my numbers" to receive a descriptive Thai summary that passed the filter, with disclaimer and audit log; (6) export and delete their data; (7) do all of it on synthetic data in the demo without any bank connection.

**Not in MVP:** slips/OCR, live prices, email/bank API, LINE push, checklist, income estimate, asset-allocation commentary, splits, streaks, payments, pricing.

**Definition of done (per story):** acceptance criteria demonstrated; automated tests pass in CI; no real personal data in fixtures; Thai copy reviewed; disclaimer present where required; evidence tags on any factual claim in docs; story demo-able from the sample dataset.

**MVP exit gates (Gate B, 28 Feb):** G2 and G3 pass; privacy gate done (consent, export, delete); lawyer/SEC question sent (reply not required to ship the pilot-free academic demo, but required before any paid claim).

### 3.5 Bottom-up estimate seed (person-hours, `[I]`, ranges, to replace vj's figures)

| Item | h | Note |
|---|---:|---|
| Foundation: repo, CI, DB, auth, deploy, config | 25-35 | Counted once |
| E1 CRUD for 4 entities + recurring model | 28-40 | |
| E2 engine + golden tests | 14-22 | Includes flat/reducing, past-date handling |
| E3 due list + dashboard | 18-26 | |
| E4 indicators + what-if | 12-16 | |
| E5 masking + templates + filter + audit + 9a | 24-34 | Includes eval set build |
| E6 consent, export, delete, isolation | 12-18 | |
| E7 onboarding, sample data, Thai UX, a11y | 10-16 | |
| E10 testing, usability, docs, final report, demo | 30-40 | Matches f-abdul's "about 60 h" only in order of magnitude; recount |
| **Total MVP** | **173-247** | vs usable capacity 153-205 h before exams (D-S18 arithmetic) |

Conclusion `[I]`: the full MVP list is at or above the optimistic edge of capacity. Therefore: (a) trim 9a to a single template and defer E7 accessibility depth if needed, (b) keep every stretch item behind the freeze, (c) track weekly burn from sprint 1 and re-plan at the 31 Jan checkpoint. Do not add any new Must without removing one.

### 3.6 RICE-style ranking (assumptions explicit)

Score = Reach x Impact x Confidence / Effort x 100. Reach = share of target-persona users who would use it in a month (0-1, guess). Impact = 0.25 minimal, 0.5 low, 1 medium, 2 high, 3 massive. Confidence = 0-1 (low because no interviews). Effort = midpoint person-hours from 3.5. All inputs are `[I]` guesses, to be re-scored after interviews; the ranking is a starting argument, not a result. RICE ignores dependencies and compliance gates, so those override it.

| Feature | R | I | C | E (h) | Score | MoSCoW | Final order driver |
|---|---|---|---|---|---:|---|---|
| 4 Due items | .7 | 2 | .7 | 8 | 12.3 | Must | dependency on 8 |
| 2 Income | .95 | 1 | .9 | 7 | 12.2 | Must | required input |
| 10 Debt indicators | .7 | 2 | .6 | 7 | 12.0 | Should | needs 2, 8 |
| 7 Dashboard / net worth | 1 | 2 | .8 | 16 | 10.0 | Must | shows value |
| 8 Amortization | .7 | 2 | .7 | 12 | 8.2 | Must | foundation for 4/10/11 |
| 3 Expenses | .9 | 1 | .8 | 10 | 7.2 | Must | required for cash margin |
| 11 What-if | .5 | 1.5 | .6 | 7 | 6.4 | Should | cheap after 8 |
| 1 Assets (manual) | .8 | 1 | .7 | 10 | 5.6 | Must | balance sheet needs assets |
| NEW Liabilities CRUD | 1 | 2 | .9 | 8 | 22.5 | Must | missing feature |
| 9a Explain numbers | .6 | 2 | .5 | 25 | 2.4 | Should | strategic: only AI in MVP, project title |
| 5 Income estimate | .5 | .5 | .4 | 8 | 1.3 | Could | needs history |
| 12 Slip OCR | .6 | 2 | .3 | 30 | 1.2 | Could | high effort, low confidence |
| 6 Checklist | .2 | 1 | .4 | 8 | 1.0 | Could | research gap |
| 9b Asset summary | .3 | 1 | .3 | 12 | 0.75 | Won't (this MVP) | legal gate |
| Privacy/consent/delete | n/a | n/a | n/a | 15 | n/a | Must (compliance gate) | not RICE-scored |
| Splits, streaks, payment links, email ingestion, LINE push | | | | | | Won't (this MVP) | D-S12/14/15/16 |

### 3.7 Sprint plan to April 2027 (3 people, 2-week sprints) `[I]`

Dates are proposals. Exam and holiday periods are unknown (Q9) and must be inserted; each sprint is sized to roughly 14-18 team-hours, below 18 so that one lost week does not break it. Gates in bold.

| Sprint | Dates | Goal | Output |
|---|---|---|---|
| S0 | 23 Nov-6 Dec | Decide and discover | D-S19/20, D-S1, D-S4 decided; roles; 10-15 interview guide and first 5 interviews; legal/SEC/PDPA question sent; advisor rubric requested; glossary and data model agreed; repo scaffold; synthetic dataset v0 |
| S1 | 7-20 Dec | Foundation and CRUD | Auth, DB, deploy, CI; assets/income/expense/liability CRUD; recurring model |
| S2 | 21 Dec-3 Jan | Engine | Amortization (reducing + flat + zero), golden tests, past-date handling |
| S3 | 4-17 Jan | First demo | Due list, net worth dashboard, edit/delete, sample data. **Gate A (17 Jan): walking skeleton on synthetic data; interviews complete; target user confirmed** |
| S4 | 18-31 Jan | Debt insights | DSR/DTI, cash margin, what-if; onboarding; usability round 1 (5 testers). **Checkpoint 31 Jan: re-estimate remaining hours vs capacity** |
| S5 | 1-14 Feb | Safe AI | Masking, template, filter, audit log, 9a, AI eval set v1; provider benchmark on synthetic data |
| S6 | 15-28 Feb | Privacy and freeze | Consent, export, delete, isolation tests; Thai UX polish. **Gate B (28 Feb): MVP freeze; G2/G3 pass** |
| S7 | 1-14 Mar | Quality | Usability round 2; fix list; eval set v2; provider terms check closed |
| S8 | 15-28 Mar | One stretch only | Pick exactly one: car-loan checklist, or QR-only slip demo, or CSV import (decision by usability findings) |
| S9 | 29 Mar-11 Apr | Hardening | Accessibility pass, docs, final eval report. **Gate C (4 Apr): feature freeze** |
| S10 | 12-25 Apr | Delivery | Final report, demo rehearsals x2, buffer |
| Contingency | 26-30 Apr | Reserve | Submission, no new work |

---

## 4. Risk register (top 12) and pre-mortem

### 4.1 Risk register `[I]` (likelihood/impact 1-5)

| # | Risk | L | I | Mitigation | Owner (proposed) |
|---|---|---|---|---|---|
| R1 | Authority/approval stays unresolved (D-S19/20, approver `<TODO>`); scope keeps shifting between three authors | 5 | 5 | Vote in S0; freeze scope doc as v1.0 with change log | PO |
| R2 | Capacity overrun: MVP seed 173-247 h vs 153-205 h usable; exams unknown | 4 | 5 | Weekly burn tracking; Checkpoint 31 Jan; strict "nothing added without removal" | PO |
| R3 | No validated user need: interviews slip, so priorities are guesses | 4 | 4 | Start interviews in S0; accept students/peers as proxies but label as such | PO + all |
| R4 | Amortization wrong for Thai loan types (flat rate, floating, minimum payments) | 3 | 5 | Rate-type field, golden tests with lender-style samples, label estimates | Eng lead |
| R5 | Output filter fails on Thai paraphrase; AI emits advice-like text | 3 | 5 | Template-only output, number whitelist, red-team set, fallback text | AI/Safety |
| R6 | SEC/PDPA interpretation goes against us; or paid model triggers licensing | 2 | 5 | Free pilot; debt-side-first scope; written lawyer/SEC question in S0; legal wording review | PO |
| R7 | PDPA mishandling: real user data before consent/deletion/provider terms ready | 3 | 5 | Synthetic data only until Gate B and provider-terms closure; privacy gate in DoD | Privacy lead |
| R8 | Manual entry friction kills the usability test and demo | 4 | 4 | Basic-mode loans, sample data, progressive disclosure, time-to-dashboard metric | UX |
| R9 | Competitor already covers it (MeowJot slips/categories `[V]` per Omm; bank apps' spend summaries `[S]`) and Thai "no competitor" claim unproven | 3 | 3 | Differentiate on loan maths and balance sheet; run the Thai-language/app-store check; never write "no competitor" | PO |
| R10 | LLM cost/availability and provider-terms block (cross-border, no-training) | 3 | 3 | Mock provider in tests; model-agnostic client; benchmark in S5 | AI/Safety |
| R11 | Bus factor: 3 people, one engineer knows the engine; or a member drops | 3 | 4 | Pairing on engine and filter; docs in repo; roles with deputies | PO |
| R12 | Scope creep from stretch/other authors' ideas (LINE push, email, streaks) | 4 | 4 | Parking lot; MVP freeze; stretch pick limited to one in S8 | PO |

### 4.2 Pre-mortem: "It is April 2027 and the project failed. Why?"

1. **We never decided who decided.** Three authors kept writing documents; Omm's bills-in-LINE and vj's slips-first drifted away from Jay's balance sheet. In March the demo was a half-built chat bot, a half-built dashboard and no engine.
2. **The AI was left to the end.** Because 9 was P2, the LLM work, masking, filter and audit log started in March and were rushed; the demo was a spreadsheet with no AI, and the grader asked "where is the AI?".
3. **The engine lied.** A flat-rate car loan produced a plausible but wrong interest figure in front of the advisor; trust collapsed because "code computes every number" had no tests behind it.
4. **Nobody could enter their data.** Testers needed 25 minutes and quit at the loan form; the cash margin and ratios were empty for everyone.
5. **Hours were fiction.** The estimates summed to more than capacity from day one; exams in January cut two sprints; we shipped everything to 70%.
6. **A legal scare.** Late in the project someone said "this looks like investment advice" about an asset commentary; there was no lawyer answer, so we cut it and lost the AI feature.
7. **The privacy gap.** A pilot used real friends' data before consent, deletion and provider terms existed; we had to withdraw it.
8. **Presentation risk.** No scripted demo, no fallback dataset; the live API call failed during the final presentation.

Each failure maps to a mitigation above: R1, the 9a split, R4 tests, R8 basic mode, R2 burn tracking, R6 legal question in S0, R7 gate, and the demo fallback in section 5.4.

---

## 5. One-page product brief, demo script, grading, questions

### 5.1 One-page brief `[I]` (draft for the team; claims outside the sources are labelled)

**Product.** A Thai personal balance-sheet tracker for people who carry loans: all assets, debts, income and expenses in one place, what is due soon, the loan principal/interest split, debt indicators, a payoff what-if, and a plain-Thai AI summary of the user's own numbers.

**Problem (hypothesis).** People with several loans and assets cannot easily say what they owe, what is due when, and whether next month's cash covers it. Context: household debt about 85% of GDP in 2026 and about 148% of disposable income in Q1 2026 `[S]`; households earning up to 15,000 baht per month spend 40.1% of income on debt service and about 23.8% of indebted households have a negative cash margin `[S, secondary report]` (direction s.7). User demand evidence is weak (direction s.7); no interviews exist.

**Target user (proposed).** Salaried Thai adults with at least one loan and a few asset kinds (persona "Nok"). Investors and students are later segments.

**Value.** One truthful, computed picture of net worth and obligations; explanations in Thai that never tell the user what to buy or sell.

**Principles (from the team).** Code computes every number; the LLM only extracts and explains; descriptive only with four safeguards; identifiers masked before any LLM call; no ID card images; synthetic data in the repo.

**MVP.** Section 3.4. **Not now:** bank API, slips (stretch), LINE push, pricing.

**Goals / success.** G1-G5 in gap item 2.

**Differentiator (hypothesis `[I]`).** Loan maths + balance-sheet view + safe explanation; not cross-bank slip capture (MeowJot advertises it, `[V]` per Omm's page check dated 2026-11-20).

**Key risks.** Capacity, unvalidated demand, SEC/PDPA interpretation, engine correctness.

**Timeline.** Gate A 17 Jan, Gate B 28 Feb (MVP), feature freeze 4 Apr, delivery 25 Apr, contingency to 30 Apr.

**Team.** 3 students, 3-4 h/week each, roles in 4.5.

### 5.2 Legal and policy gates (dated) `[I]`
Send the SEC/lawyer question in S0 (descriptive debt explanation; asset commentary; effect of paid subscription). Result needed before: 9b, any pricing, any public launch. Provider terms/cross-border check needed before any real-user data. Both remain UNVERIFIED until answered.

### 5.3 Roles (proposed; names TODO) `[I]`
- Role A "PO/Research/Docs": decisions log, interviews, legal questions, evidence tags, final report.
- Role B "Engine/Backend": data model, amortization, indicators, auth, privacy features.
- Role C "AI Safety/Frontend": masking, template, filter, eval set, UI and Thai UX.
Each has a named deputy; weekly 30-minute sync; PO keeps the backlog.

### 5.4 Demo script (about 8 minutes, all synthetic) `[I]`
1. 30 s: problem and persona Nok; disclaimer shown.
2. 60 s: first-run, tap "sample data" (fallback) or live-add a loan in basic mode.
3. 90 s: dashboard: net worth with completeness; items due in 10 days and by month-end.
4. 90 s: car loan schedule: principal/interest split; show flat-rate label and the golden test that validates it.
5. 60 s: indicators: DSR, DTI, cash margin with formulas visible.
6. 60 s: what-if: extra 2,000 per month; end date and interest saved.
7. 90 s: "Explain my numbers": Thai summary; show the masked payload and audit log entry; then type "should I sell my fund" and show the safe refusal.
8. 30 s: export/delete data; future work (bank open data, slips).
Fallbacks: recorded video, local seed DB, cached AI responses, offline mode if the API fails. Rehearse twice (S10).

### 5.5 Grading and advisor alignment (provisional `[I]`; no rubric exists in the repo)
The advisor's only recorded input is "too wide; pick one field". Unknowns: weighting of implementation vs documentation vs presentation; whether AI use, novelty, business viability or research evidence is graded. Provisional mapping: scope discipline (one field, MVP boundary), working software on defined scenarios (E1-E4), AI integration with safeguards (E5), evidence-tagged research (conventions), evaluation (eval set, usability), process (sprints, risk log), and documentation (brief, SRS rewrite, final report). The SRS in f-abdul is superseded (399 IDs for the old idea); decide whether a lighter new SRS is a deliverable (Q3).

### 5.6 Questions the team and advisor must answer, ordered by blocking impact

1. **Q1 (blocks D-S19, D-S3, D-S1):** Who is "the owner" vs "the advisor", who has final say, and is LINE a requirement or a preference?
2. **Q2 (blocks D-S20, process):** Do all three members accept the baseline as approved? Who is the CLAUDE.md approver (one lead or 2 of 3)?
3. **Q3 (blocks priorities and docs):** What is the grading rubric and required deliverables (SRS, report, demo, code, evaluation)? Is an AI feature required in the working prototype?
4. **Q4 (blocks planning):** Who is on the team (names, roles), and what is each member's real weekly availability including exams and holidays 22 Nov-30 Apr?
5. **Q5 (blocks D-S4):** Which target user and how will we recruit 10-15 interviewees by 17 Jan, and are student/peer proxies acceptable?
6. **Q6 (blocks E2, E1):** Which loan types and rate conventions must the engine support (flat, reducing, floating, minimum-payment cards)? Which lenders' documents can we use as synthetic templates?
7. **Q7 (blocks E5, D-S10, D-M5):** Can we get an SEC/lawyer answer on descriptive debt explanation, asset commentary, and effect of a paid subscription? By when?
8. **Q8 (blocks E5 start):** Which LLM provider(s) are acceptable on cost and data terms (no-training, cross-border); what is the cash budget?
9. **Q9 (blocks E6):** Retention period for logs and data; is the audit log exempt from deletion (D-A7)? Who answers (lawyer, advisor)?
10. **Q10 (blocks hosting):** Where is it hosted and who pays; does the final demo need to be online and accessible to graders?
11. **Q11 (blocks D-S3):** Web app, LIFF, or native-style PWA for the first client?
12. **Q12 (blocks differentiation claim):** Who runs the Thai-language and app-store competitor check, and by when (CLAUDE.md rule)?
13. **Q13 (blocks D-S17/D-S18):** Are f-abdul components (reminder engine, IF-LIFF-01) to be reused or re-estimated from scratch?
14. **Q14:** Name for the product and approved team names in README.
15. **Q15:** Is a real-user pilot expected (which triggers privacy gates) or is a synthetic-data evaluation enough?

---

## 6. Method and limits

- Read: CLAUDE.md, README.md, docs/progress.md, docs/repo-conventions.md, the scope spec, the direction research, all 7 files in docs/consolidated/ (index, 01, 02, 03 sampled by section headings and key tables; the three decision logs read in full), and f-abdul's README, charter and timeline (skimmed for reusable structure). The long 02/03 bodies were not read line by line.
- No web access used. All Thai-domain claims about loan rate conventions are `[I]` from general knowledge and UNVERIFIED for Thai specifics.
- RICE inputs, hour estimates, sprint dates and thresholds are PO proposals `[I]`.
- Per CLAUDE.md, no change to it is proposed here; wording alignment (C1) is for the team to agree.
