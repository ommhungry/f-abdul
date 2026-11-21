# Revised scope and plan (product-owner draft)

- Title: AI financial assistant, revised scope, phasing and backlog
- Date: 2026-11-21
- Status: Draft for team review. Not approved. Does not replace `2026-11-17-ai-financial-assistant-scope.md`; the team decides whether to adopt it. `CLAUDE.md` is not changed by this file.
- Scope: product scope, phasing to May 2027, MVP definition, plan changes, open decisions.
- Revision 2 (2026-11-21): adds the strong parts of Omm's and vj's documents, listed with sources in section 15. Their files are unchanged.
- Method: five parallel reviews (market, legal, users, technical, plan audit) on 2026-11-21, merged by the product owner. Full reports are in the working folder, not in the repo yet. Evidence tags are copied from those reports and never upgraded. Anything new is marked `[I]` (proposal).
- Related: `2026-11-17-ai-financial-assistant-scope.md`, `../research/2026-11-17-ai-financial-assistant-direction.md`, `../consolidated/00-index-and-decision-map.md`
- Tags: scope, phasing, MVP, decisions

## 1. Decisions the team made on 2026-11-21

| # | Decision | Effect in this draft |
|---|---|---|
| T1 | Target user: keep both personas, loan side first | Persona A (salaried, several loans) leads. Persona B (investor with a loan) is a later segment. |
| T2 | Plan all 11 features, but build the lean core MVP first (core plus the ownership checklist), then expand | Two phases, section 4. |
| T3 | Split feature 9: "explain my numbers" (9a) goes into the MVP; asset commentary (9b) waits for the legal check | Section 3. |
| T4 | Calendar: proposal presentation on 8 Dec 2026 (low-fi is enough); project runs to May 2027 | Section 5. |
| T5 | This revision lives as a new draft in `docs/specs/` | This file. |

## 2. What changed from the 17 Nov scope, and why

1. **Liabilities become an explicit feature (new P0, "F0 Debts").** Features 4, 8, 10 and 11 all need a loan record, and none of the 11 features defines one. The dashboard (7) must also show debts and net worth. `[I]`
2. **AI moves into the MVP as 9a.** In the 17 Nov scope the only AI features sit at P2, so the P0 build contains no AI. A project called an AI assistant needs a working, safe AI path early, and the safeguards (masking, templates, output filter, audit log) take real time. `[I]`
3. **Engine rules for Thai loans are added.** A rate-type field (reducing balance, flat, minimum payment, zero rate), per-loan day-count setting, labelled early-payoff method, and an error when a payment is below the monthly interest. Computed examples: a flat rate gives an effective rate about 1.7 to 1.9 times higher; the early-payoff method alone changed one test case by THB 33,959; ACT/360 vs ACT/365 changed one year of interest on 500,000 by THB 276.77. `[V-calc, technical review]`
4. **"No competitor" wording is withdrawn.** The Thai-language and app-store check found partial overlaps (section 7). Use the safe wording there.
5. **Success measures, glossary, definition of done and acceptance criteria are added** (sections 6 and 8), because none existed.
6. **Hours are re-counted** (section 5). The earlier 60-130 h, 80-132 h and 121-198 h figures measure different things and should not be compared.

## 3. Feature list (all 11 plus F0), with phase

Numbers 1 to 11 keep the 17 Nov numbering. F0 and 9a/9b are new labels.

| # | Feature | Phase | Priority | Notes |
|---|---|---|---|---|
| F0 | Debts (loan record with rate type) | 1 | P0 | New. Basic mode: balance plus monthly payment only, flagged "estimate". |
| 1 | Asset list | 1 (manual) / 2 (prices, CSV) | P0 | Manual value plus as-of date and value source. Stale marker after 180 days `[I]`. |
| 2 | Incomes | 1 | P0 | Recurring templates, one-off items kept separate. |
| 3 | Expenses | 1 | P0 | Recurring and one-off. |
| 8 | Principal/interest split (engine) | 1 | P0 | Build first. Golden tests to the satang. |
| 4 | Items due in X days or by month-end | 1 | P0 | Reads loan schedule and recurring items. User taps "paid". |
| 7 | Dashboard | 1 | P0 | Net worth with completeness note and oldest-valuation date; cash margin; debts. |
| 10 | Debt health indicators | 1 | P1 | DSR, DTI, cash margin. Formula visible. Neutral wording, no good/bad labels. |
| 11 | What-if payoff simulator | 1 | P1 | Reuses the engine. Worded as a calculation result. |
| 6 | Loan-closing checklist (was "not-yet-owner", adopted from vj) | 1 | P1 | Car hire-purchase list first (researched). Mortgage-redemption list only after the steps are researched. Each list shows source and date, reminds one month before the last instalment, and files nothing. |
| 9a | Explain my numbers (debt side) | 1 | P1 | Fixed Thai template, code-computed facts, masked input, output filter, audit log. |
| 9b | Asset commentary | 2, after legal check | P2 | Stays out until the lawyer or SEC answer is in. |
| 5 | Next-month "money left" estimate (adopted from vj) | 2 | P2 | Expected income minus bills due minus typical spending, shown as a range and labelled an estimate. Needs months of history and features 3 and 4. Rule-based, no LLM. |
| 1 (ext.) | Live prices, CSV import | 2 | P2 | SET data is paid with a separate redistribution licence `[S]`; keep stocks and gold manual. Candidates: SEC fund NAV, FX, CoinGecko Demo, each needing a terms check. |
| Slip | Slip QR demo (the old "feature 1 / slip OCR" stretch item) | 2, one flow | P2 | Slip QR holds bank code and reference, not the amount, so the amount needs OCR or a paid API `[S]`. OCR-only data stays out of totals until the user confirms. |

Later upgrade, unchanged: bank API integration. Bank of Thailand "Your Data" starts with deposit data by end-2026 and loan, card and e-money data in 2027-28 `[V]`. No source shows that an unsupervised app can receive it, so it stays future work.

Still out of scope: product recommendations, filing documents, moving money, bill payment, credit scoring (added from vj's Won't list), social features, gamification, splits and IOUs, streaks.

## 4. Phasing

**Phase 0, now to 8 Dec 2026: proposal and decisions.** Proposal deck, low-fi wireframes (section 9), decision votes, legal question sent, interview recruiting started.

**Phase 1, lean core MVP (Dec 2026 to about end Feb 2027):** F0, 1 (manual), 2, 3, 8, 4, 7, 10, 11, 6, 9a, plus consent, export, delete, onboarding and sample data. All on synthetic data. Demo-able without any bank connection.

**Phase 2, expansion to the full list (Mar to May 2027):** 9b (if cleared), 5, prices and CSV, slip demo, plus whichever of these the team picks, one at a time. Rule: nothing new enters Phase 1 without something leaving.

Dated gates are in section 5.

## 5. Calendar and hours `[I]`

| Date | Gate | Meaning |
|---|---|---|
| 8 Dec 2026 | Proposal presentation | Low-fi wireframes, scope, plan, evidence. Fixed by the team. |
| 17 Jan 2027 | Gate A | Engine golden tests pass; data model done; interviews underway. |
| 31 Jan 2027 | Hours checkpoint | Compare burn against estimate; cut scope if behind. |
| 28 Feb 2027 | Gate B | Phase 1 MVP done: privacy gate (consent, export, delete) and AI eval pass. |
| Apr 2027 (date TODO) | Phase 2 feature freeze | Only bug fixes after. |
| May 2027 (date TODO) | Delivery | Exact date to be confirmed with the advisor. |

Hours (person-hours, ranges):

| Source | Phase 1 estimate | Note |
|---|---|---|
| Technical review | 118-173 core; 138-205 with a templated 9a | Engine 24-36 h (12-16 h if monthly reducing-balance only). AI safety layer (masking, templates, filter, eval set) 32-48 h. |
| Plan audit | 173-247 | Includes foundation 25-35 h and testing, docs and demo 30-40 h. |
| Checklist (6) | +6-10 h | Needs mortgage step research first. Matches vj's 6-10 h row. |
| vj row estimates (cross-check) | see section 15.2 | Use vj's rows, not its tier subtotals, which do not match its rows. |

The two estimates do not agree and both touch or exceed the earlier usable capacity figure of 153-205 h, which ran to April and assumed unknown exams. With May as the end date the capacity is probably larger, but it is not yet re-counted. Action: each member states real weekly hours for 22 Nov to May, then reconcile the two estimates by story. Until then, treat Phase 1 as at risk and keep 9a to one template.

## 6. Goals and measures `[I]` (targets are proposals; no Thai benchmark found)

| ID | Goal | Measure |
|---|---|---|
| G1 | Numbers are correct | 100% match to the satang on 30+ synthetic loan cases; all property tests pass |
| G2 | AI stays descriptive | 0 forbidden outputs on a 150-prompt eval set (100 benign, 50 adversarial, Thai and English); every number in an explanation exists in the supplied facts |
| G3 | Data entry is bearable | First dashboard in 10 minutes or less on a scripted test; one loan in under 3 minutes |
| G4 | People understand it | 80% of testers correctly answer what is due next and what the AI summary said |
| G5 | People trust it | Trust rating 4.0 of 5 or higher; no buy/sell language in audit logs |
| G6 | Privacy basics work | Consent, export, delete tested; 0 raw identifiers in logged LLM payloads |

Pilot-style targets (activation 60%, week-4 return 40%) apply only if a real-user pilot happens; see decision O9.

## 7. Market position (as of 2026-11-21)

Safe wording: "As of 21 Nov 2026, a Thai and English search of the TH App Store, Google Play TH and the web found no single app advertising, with a Thai UI, all of: multi-type asset list, loan principal/interest schedule, due dates, debt-health ratios and a what-if payoff simulator. Based on public listings, not tested. Parts exist in SET Happy Money, Wallet Story, AssetFlows, MeeTang and free calculators." Do not write "no competitor", "first", "unique" or "gap proven".

- Partial overlaps `[V]` from store listings, not tested: SET Happy Money (assets, liabilities, net worth, bill reminders); Wallet Story (debts, instalments, reminders, AI coach); AssetFlows (amortization and payoff simulator, English listing); MeeTang (Thai AI assistant over the user's data, instalment schedule, first released 2026-10-06). The Bank of Thailand debt tool and free calculators already do payoff arithmetic.
- Differentiators that survive `[I]`: one Thai-first balance sheet joining schedule, due dates, ratios and what-if; ratios shown with their inputs; descriptive-only AI with masking and an audit log. The checklist (6) had no hits, but that is absence in listings only. Cross-bank slip capture is not an edge.
- Threats: banks' built-in spend summaries; rivals ship fast; Bank of Thailand open data later.
- Unclosed: K PLUS pages returned 403; budgeting screens of several other banks not checked; no app was installed. Next step: hands-on trial of four apps with synthetic data.
- Price evidence: Thai monthly prices run THB 49-99; the team's THB 60-80 idea sits inside that range. Free substitutes anchor low. No Thai willingness-to-pay data exists `[UNVERIFIED]`. MeowJot's regular vs promotional price needs a browser check before anyone quotes it (D-M4).

## 8. Backlog summary and definition of done

Epics: E0 decisions and discovery; E1 balance-sheet core (assets, debts, income, expenses, recurring); E2 loan engine; E3 due items and dashboard; E4 debt insights; E5 safe AI explain; E6 privacy and account; E7 onboarding and Thai UX; E8 checklist; E9 Phase 2 items; E10 quality and delivery.

About 35 stories with Given/When/Then criteria for all P0 and P1 items are in the working-folder plan audit (`po-05-plan-audit-backlog.md`, section 3.3). They should be moved into the repo once the team accepts this draft.

Definition of done for a story: acceptance criteria demonstrated; automated tests pass in CI; no real personal data in fixtures; Thai copy reviewed; disclaimer shown where AI text or ratios appear; evidence tags on any factual claim in docs; demo-able from the sample dataset.

Glossary (proposal): asset = owned item with value, as-of date and value source; liability = obligation with outstanding principal and a rate type; net worth = assets minus outstanding principal, shown with completeness and oldest valuation date; cash margin = monthly income minus recurring expenses minus scheduled debt payments; DSR = scheduled monthly debt payments divided by monthly income (pick gross or net once and label it); DTI = total outstanding debt divided by annual income. THB only in Phase 1. Full table is in the plan audit, section 1.1.

AI safeguards, implementation `[I]`: the disclaimer also says the summary was written by AI from the user's own entered numbers (adopted from vj, section 15.2); the model fills a fixed template with slots only; a post-filter blocks a denylist of Thai and English patterns and any number not in the supplied facts; a blocked output is replaced with fallback text and logged; outbound payload and output are stored in the audit log. Default for the model: benchmark candidates on synthetic data now; use no free-tier LLM with real-user data (Gemini's free tier uses content to improve products and allows human review `[V]`).

## 9. Low-fi wireframes for 8 Dec (suggested set)

1. First run: disclaimer, consent, "try with sample data".
2. Add a debt (basic mode and full mode).
3. Dashboard: net worth with completeness, cash margin, due soon.
4. Loan schedule: principal/interest split, rate-type label.
5. Debt health: DSR, DTI, margin with formulas visible.
6. What-if payoff: extra payment slider, end date and interest saved.
7. Explain my numbers: Thai summary, disclaimer, "what was sent" view.
8. Ownership checklist for a car loan.

Proposal deck outline: problem and evidence, user and persona (marked hypothesis until interviews), competitor table with the safe wording, scope and phases, safeguards, wireframes, plan and risks, what we need decided.

## 10. Risks `[I]` (top items; full register of 12 and a pre-mortem are in the plan audit)

| Risk | Mitigation |
|---|---|
| Approval and authority stay unresolved; scope keeps drifting | Decide O1 and O2; freeze this scope as v1.0 with a change log |
| Hours exceed capacity | Weekly burn tracking; 31 Jan checkpoint; nothing added without removal |
| No validated demand; interviews slip | Start recruiting now; label peer proxies as proxies |
| Engine wrong for Thai loan types | Rate-type field, golden tests, estimate labels |
| Output filter fails on Thai paraphrase | Template-only output, number whitelist, red-team set, fallback text |
| SEC or PDPA reading goes against us | Descriptive scope; written lawyer question now; 9b and pricing wait for the answer |
| Real data used before privacy gate | Synthetic data only until Gate B and provider-terms check |
| Live demo fails | Recorded video, local seed database, cached AI responses |

## 11. Legal and privacy position (research, not legal advice)

- Descriptive wording tracks how the Securities and Exchange Act defines investment advisory service (advice on value or suitability of securities, or on buying or selling, for a fee) `[S, law-firm summary]`. Section number `UNVERIFIED`. Effect of a paid plan `UNVERIFIED`.
- Financial data is not on the PDPA sensitive-data list `[S]`. Treat it as high-risk anyway and take explicit consent. Masked but reversible data probably still counts as personal data `[I]`.
- Fine for a class demo: synthetic data with descriptive wording.
- Blocks a real-user pilot: a student team with no legal entity; Apple's rule that money-management apps come from licensed institutions `[V]`; provider terms check; privacy notice, consent, export, deletion, breach process (72-hour PDPC notice where feasible `[S]`).
- Gmail: skip OAuth (restricted scopes need an annual security assessment `[V]`). If email forwarding is ever built: never auto-confirm Gmail's verification link, use unguessable per-user addresses, require SPF and DKIM, show the requesting address to the user for approval, and keep email-derived records pending until confirmed.
- LINE push quota and a reported February 2027 change are `[S]`, conflicting and unconfirmed; LINE's financial-content policy was not found.
- Twelve questions for a Thai lawyer are in the legal review.

## 12. Defaults applied in this draft (not decided by the team; confirm or change)

| # | Default | Reason |
|---|---|---|
| D1 | Web app first (PWA), engine as a pure module, LINE as an optional thin layer later | Channel-neutral core; cheapest to build and test. Cheapest alternative: one Python app on SQLite, no LINE. |
| D2 | THB only; Thai default language, Buddhist Era dates | Keeps Phase 1 small. |
| D3 | Synthetic data only until Gate B and the provider-terms check | Privacy rule in `CLAUDE.md`. |
| D4 | Bank of Thailand open data, bank API and Gmail stay out of the build | Sources above. |

## 13. Decisions still open (the team should answer these next)

Ordered by blocking impact. Each is a proposal from the product owner, not a ruling.

| # | Question | Proposal |
|---|---|---|
| O1 | Who is "the owner" versus the advisor; is LINE required or a preference? | Ask the advisor; until answered, web first. |
| O2 | Does the 17 Nov baseline count as approved; who approves changes to `CLAUDE.md` (one lead or 2 of 3)? | Team vote; one named lead is faster. |
| O3 | Grading rubric and required deliverables (SRS, report, demo, code, evaluation); is an AI feature required in the prototype? | Ask the advisor. This draft assumes 9a is required. |
| O4 | Real weekly hours per member, 22 Nov to May, including exams | Each member reports; then reconcile the two estimates. |
| O5 | Who sends the SEC or lawyer question, and by when? | Before the 8 Dec presentation. |
| O6 | Which LLM provider(s) and cash budget; acceptable data terms? | Paid tier with no-training terms; benchmark Thai quality on synthetic data. |
| O7 | Which loan types must the engine support in Phase 1? | Reducing balance and flat rate first; floating and card minimum-payment in Phase 2. |
| O8 | Is f-abdul reuse (reminder engine, IF-LIFF-01) wanted or re-estimated from scratch? | Re-estimate; treat old figures as old-scope. |
| O9 | Real-user pilot, or synthetic-data evaluation only? | Synthetic evaluation plus usability tests on synthetic data; pilot only after the privacy gate. |
| O10 | Retention period for logs; is the audit log exempt from deletion? | Ask the lawyer; no source gives a period. |
| O11 | Hosting and who pays; does the final demo need to be online? | Decide before Phase 1 starts. |
| O12 | Product name and team names for the README | Team choice. |
| O13 | Who runs the hands-on competitor trial (four apps, synthetic data) and the interviews? | Assign before 8 Dec. |
| O14 | The 40 logged contradictions (D-S, D-A, D-M) not settled above | Work through the decision map in `consolidated/00-index-and-decision-map.md`, starting with O1 and O2. |

## 14. Known gaps in the evidence

- No interviews exist; the persona and priorities are hypotheses.
- Investor-account counts conflict between sources and overlap with indebted workers has no data; do not use these figures.
- Household debt figures: UTCC (THB 794,945 average, n=1,720, Sep 2026) and NSO (about THB 153k `[S]`) measure different things.
- Credit-card rules (16% cap, 25% personal-loan cap, 8% minimum payment) are secondary-source only.
- Thai chat quality of candidate models is untested.
- Several legal points rest on law-firm and press summaries; primary Thai texts were not reached.

## 15. Strong parts adopted from Omm's and vj's documents

Source: the consolidated documents in `docs/consolidated/`, which carry line references into Omm's and vj's original files (written here as "architecture L283" and so on). I did not reopen the originals, and their evidence tags are unchanged. "Adopted" means written into this draft as a proposal that the team can reject. "Parked" means a good idea that conflicts with a team decision or an open item, so it stays out of Phase 1.

### 15.1 From Omm: adopted

| Item | Source | Where it lands |
|---|---|---|
| Component outline: channel adapter, ingestion with validate-and-reconcile, masking layer, model adapter, output guard, domain services, background worker, PostgreSQL as the authoritative store | architecture L63-L96 | Channel-neutral core (default D1). The adapter is web now and LINE later. |
| Data model entities: source event with a dedup key, attachment with a deletion time, financial record with a confirmation state, bill, reminder, payment evidence, AI output log (user, interaction ID, output, masked input, filter result, disclaimer shown, timestamp), masking map, usage event, action log. Amounts as exact decimals or integer minor units with currency, due dates as dates, every record owned by an authenticated user | architecture L283-L310 | Phase 1 data model (E1). Phase 1 uses the subset: financial record, debt, recurring template, AI output log, masking map, usage event, action log. |
| Record rules: a card purchase counts once and paying the card settles the liability; an incoming transfer is not automatically income; expected and received income are separate states; an obligation and its expense are linked to avoid double counting; amount alone never auto-merges; user corrections survive later syncs | ingestion L83-L87; architecture L275-L279, L308-L310 | Glossary and acceptance criteria for E1. |
| Payment status has three states: user-marked, evidence-attached, externally verified | architecture L318 | Phase 1 uses user-marked only. The other two are reserved for the Phase 2 slip flow. |
| Corrections invalidate derived context; deletion covers summaries, stored attachments and provider-side state where the integration allows | architecture L135-L137 | E6 deletion story. |
| Data boundaries: authorization on every query, model-generated IDs checked against the user's records, secrets kept server-side, text extracted from images or emails is data and cannot grant permissions, minimum fields per model call | architecture L350-L354 | E5 and E6 acceptance criteria. |
| Reliability rules: reuse event identity so duplicates do not repeat changes, schema validation with bounded repair of malformed model output, bounded retries, report success only after the write is confirmed, versioned records for overlapping updates | architecture L326-L346 | Non-functional requirements. |
| Six safeguard acceptance scenarios: placeholder-only payload with a server-side mapping and logged payload; ID card upload rejected and not stored; slip identifiers redacted before a vision call; OCR-only data excluded until confirmed; filter blocks buy/sell/switch wording with a fixed fallback; summary shows the disclaimer and appears in the audit log | architecture L464-L469 | Tests for G2 and G6. The slip redaction scenario waits for Phase 2. |
| Cost method: cost per interaction = uncached input + cached input + output + OCR, verification and tool charges + retries; report median, high-percentile and maximum user cost, not the average; cap output and tool loops; run button actions in code; reserve budget for in-flight requests | architecture L379-L432 | Decision O6 and E5. Omm tags its Gemini example rate UNVERIFIED; the technical review reports it now verified. Re-check before quoting. |
| Evaluation set content: Thai dates, Buddhist Era years, Thai and Arabic numerals, mixed-language names; store canonical values with the source text kept | architecture L246-L248 | AI eval set and any typed-entry parser. |
| Warning that record upkeep can become the user's "second job" | board L27; ingestion L33-L37 | Already risk R8; supports basic-mode loans, recurring templates and sample data. |
| Email controls for later: enforce source filters in backend code, exclude one-time codes and password resets, treat email as untrusted, bind and verify the receiving address, strip quoted history | ingestion L64-L68 | Kept for Phase 2 or later; consistent with section 11. |

### 15.2 From vj: adopted

| Item | Source | Where it lands |
|---|---|---|
| Per-feature hour rows as a cross-check: F1 assets 12-30 (manual plus live prices), F2 incomes 8-12, F3 expenses 25-35 (includes slip reading), F4 due items 10-15, F5 money left 6-10, F6 checklists 6-10, F7 summary card 6-8 and web dashboard 15-25, F8 principal/interest 10-15, F9 descriptive 8-12. Features 10 and 11 have no vj estimate | evaluation L47-L55, L57 | Section 5. Use the rows, not the tier subtotals: Must is stated 55-77 h but the rows sum to 49-70 h, and the F1 row conflicts with the tier lines. |
| Feature 5 reshaped to "estimated money left" (expected income minus bills due minus typical spending), shown as a range, labelled an estimate | evaluation L136-L143 | Section 3, Phase 2. Open item D-S9 stays open for the team. |
| Feature 6 reshaped: "not yet owner" holds for a car hire-purchase, but a mortgage buyer already owns the home and the bank's mortgage is redeemed at the Land Office. Two team-written checklists, each with source and date; reminder one month before the last instalment; never files anything. The legal basis is `[I]`, to be confirmed with a legal source | evaluation L174-L187 | Section 3 row 6. |
| Won't list adds credit scoring, with buy/sell/switch instructions, filing government procedures and moving money | evaluation L200-L205 | Section 3 out-of-scope line. |
| Every report states how complete its data is, shows the source for every number, and labels estimates; loan splits are estimates unless they come from the lender's statement | evaluation L40, L132, L245 | Net worth completeness note already exists. Extend it to the AI summary, which must say which sections the user has filled. |
| Disclose AI use: SEC guidance reportedly suggests disclosing chatbot use `[S]`; our disclaimer did not say the text is AI-written | evaluation L159 | Section 8 disclaimer wording. Needs lawyer check. |
| Privacy basics: no API keys stored unless live crypto prices are built; no slip images kept longer than needed; per-user encryption, export and delete | evaluation L151, L235, L244 | E6, and the foundation epic (isolation, encryption, export, delete, evaluation set; evaluation L235-L237). |
| Slip rules for Phase 2: a verification call needs the QR payload, which holds account data and cannot be masked, so treat it as a third-party transfer with consent and decide whether to use it; slips carry names and partial account numbers; check duplicates; free tiers of about 100 slips a month `[S, UNVERIFIED]` | evaluation L75, L77, L80 | Phase 2 slip story and decision D-A4. |
| Confirmation rule: nothing derived from AI or OCR is saved without the user's tap, and a misread amount is never saved unconfirmed | evaluation L77, L233 | Default for decision D-A6: forms the user types save directly with edit and undo; AI- or OCR-derived entries need a tap. Team confirms. |
| Remembered recipient categories ("is this person your landlord?") | evaluation L71 | Phase 2, with the slip flow. |

### 15.3 Parked: good ideas that need a team decision

| Idea | Who | Decision | Why it stays out of Phase 1 |
|---|---|---|---|
| LINE as the main channel, LINE summary card (6-8 h), LIFF dashboard (15-25 h) | Omm, vj | D-S3, D-A1 | Default D1 is web first. The LINE card is the cheapest later layer. Push quota figures conflict between sources. |
| "Without typing it in" promise and slip reading as Must (25-35 h) | vj | D-S2, D-S5 | Slip QR holds no amount, a competitor already offers cross-bank capture, and the hours do not fit. |
| Splits and IOUs, ask-about-your-money chat, streaks and milestones, payment links in reminders | vj, Omm | D-S12 to D-S15 | The 17 Nov scope excludes them. Ask-about-money widens what the LLM sees. |
| Email, Gmail, bank API and batch statement import | Omm | D-S16, D-A2 | Gmail read scopes need an annual security assessment `[V]`. |
| Model proposes actions and calls tools | Omm | D-A9 | Phase 1 stays strict extract-and-explain. Revisit after the eval results. |
| Subscription THB 60-80 and an AI-cost target of THB 15-20 per payer | Omm | D-M3 | The range sits inside Thai prices (THB 49-99), but there is no demand evidence and the legal effect of a paid plan is open. |
| Bills-before-payday narrowing and its board scores (average 5.0) | Omm | D-M1, D-M9 | The scores apply to that narrowing only; do not cite them for this scope. |
| Memory layers and conversation summaries | Omm | none | Not needed for a template-based 9a. |

Not adopted anywhere: nothing from Omm's or vj's files was dropped as wrong. Every item above is either adopted or parked, and the decision IDs point to the full entries in `docs/consolidated/`.
