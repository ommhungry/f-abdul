# PO-02 Legal and regulatory research (Thai balance-sheet tracker, descriptive-only AI)

Date: 2026-11-21. Author: regulatory researcher (AI). **This is research, not legal advice.** A Thai lawyer must confirm everything marked for counsel.

Tags: [V] seen on a primary/vendor page opened this session; [S] secondary (law-firm/press summary, or search-result snippet); [I] inference; UNVERIFIED = not confirmed. Tags are not upgraded. No section/article numbers are cited unless a source stated them; others are UNVERIFIED.

Read of project files (read-only): scope spec (descriptive-only, mask before LLM, 4 safeguards, bank API later, slip OCR P2), consolidated 02 (LINE vs app pending D-A1, Gmail/bank "later" D-A2, financial-data sensitivity disputed D-A5, no retention periods D-A7, provider terms unchecked).

## 1. Findings by topic

### 1. SEC / investment advice
- Securities and Exchange Act defines investment advisory service as giving advice to the public, directly or indirectly, on the value of securities or the suitability of investing in them, or on buying/selling securities, "for a fee or other remuneration"; SEC can issue notifications on how advice may be given. [S] https://www.tilleke.com/insights/regulation-investment-advisors-thailand/ . Statutory section number: UNVERIFIED. Primary text (ratchakitcha/SEC) not opened.
- Practical reading [I]: the trigger words are *value*, *suitability*, *buy/sell*. Your safeguards (no suitability, no buy/sell/switch, no target prices, no product comparison) track those exactly. Descriptive statistics on the user's own numbers (net worth, debt ratios, allocation percentages) do not obviously fall in the definition, but "allocation is too concentrated" or "your DSR is high, reduce debt" edges toward suitability judgment. Keep indicators as numbers with neutral labels, no "good/bad/should".
- Paid subscription: the definition includes "fee or other remuneration", so charging removes the "free" defence if the content were advice; it does not by itself turn descriptive output into advice [I]. Does subscription change status? Not answered by any source opened. UNVERIFIED -> lawyer.
- Licensing: securities business needs a licence, generally a limited/public company or financial institution, via Finance Minister on SEC recommendation. [S] same Tilleke page. A student team or individual could not hold one [I].
- SEC AI/ML governance framework for capital markets applies to SEC-supervised operators incl. investment advisers and robo-advisers; principle-based, non-binding. [S] https://www.tilleke.com/insights/thailands-ai-and-machine-learning-governance-framework-for-capital-markets . Not binding on an unlicensed information tool [I]. I found no SEC statement on "information service" exemptions or data aggregators; absence of evidence only. UNVERIFIED.
- Tilleke snippet: an SEC 2024 statement (180924) mentions an exemption for advisers serving not more than 15 clients [S, search snippet only; not opened]. https://sec.or.th/EN/Template3/SECStatement/2024/SECstatementEN-180924.pdf . Do not rely on it.
- Note: the whole securities definition is about *securities*. Loan payoff simulation, DSR, cash margin are not securities advice [I]; but credit/loan "advice" could touch BOT/consumer-finance rules. UNVERIFIED.

### 2. PDPA
- Sensitive data list (race, ethnicity, politics, religion, sexual orientation, criminal record, health, disability, union, genetic, biometric, or other as prescribed): financial data is not on it. [S] https://www.linklaters.com/en/insights/data-protected/data-protected---thailand . So vj's position (not sensitive) is better supported than Jay's "treat as sensitive". Section number (s.26) is cited by project docs; I did not see it on a primary page: UNVERIFIED. Recommendation [I]: still treat financial data as high-risk and get explicit consent anyway (cheap, and safer if a lawyer or PDPC takes a broader view). Note ID-card images may reveal religion/blood group (sensitive) -> your "no ID images" rule is right [I].
- Breach notification: to PDPC without undue delay and where feasible within 72 hours of becoming aware, after a preliminary credibility assessment; data subjects told if high risk. [S] https://iapp.org/news/a/thailand-s-pdpc-clarifies-data-breach-notification-requirements
- DPO: required where core activity is large-scale processing of sensitive data or regular large-scale monitoring [S] (Linklaters/Tilleke snippets). A small pilot with non-sensitive financial data probably not [I]; UNVERIFIED -> lawyer.
- ROPA: a PDPC notification (reported date 8 Jan 2025) exempts small-business controllers/processors from record-keeping; thresholds not confirmed (the "fewer than 100 employees" figure is NOT confirmed). [S, search snippet] Keep a ROPA anyway; it is one table and you need it for the privacy notice [I].
- Cross-border: two PDPC notifications gazetted 25 Dec 2023, in force 24 Mar 2024; routes are adequacy list, appropriate safeguards (SCCs incl. ASEAN MCC and EU SCCs recognised, certification, BCR-type), and exceptions such as consent. [S] https://www.tilleke.com/insights/thailand-unveils-regulations-for-cross-border-personal-data-transfer ; https://www.clydeco.com/en/insights/2024/01/thailand-personal-data-protection-regulation-notif . No adequacy list confirmed. Practical route [I]: sign the provider's DPA (Google DPA is incorporated for paid Gemini) and rely on safeguards/consent; lawyer to confirm.
- Pseudonymisation: I could not find a primary statement. PDPC anonymisation guideline is risk-based and says following it is not itself compliance. [S] https://www.nstda.or.th/nstdaxpdpc/wp-content/uploads/2023/10/Anonymization-Guideline-TH.pdf . Position [I]: masked data where you hold the reversible mapping is still personal data for you (the controller) and probably for the recipient if linkable via amounts, dates, lenders. Treat masking as risk reduction, not exemption (matches scope §5).
- Minors: PDPA parental-consent rules for minors exist, thresholds (e.g. under 10 / under 20) not verified. UNVERIFIED. Simplest: terms require 20+ (or 18+; age of majority is a lawyer question) and pilot adults only.
- Deletion/de-identification: PDPC criteria notification effective 11 Nov 2024 [S] https://www.tilleke.com/insights/thailand-issues-criteria-for-deletion-destruction-and-de-identification-of-personal-data/ . PDPC AI guidelines exist, advisory [S] https://www.tilleke.com/insights/thailand-issues-personal-data-protection-guidelines-for-ai-systems .
- DSR rights (access, copy, portability, object, erase, restrict, rectify, withdraw consent) exist [I from general PDPA knowledge; not re-verified]; build export + delete (already in vj's foundations). Statutory response deadline: UNVERIFIED (commonly cited 30 days).
- Lawful basis [I]: consent for collection and for AI processing; contract basis for core service; consent must be separate, withdrawable, in Thai. Legitimate interest is a weak fit.

### 3. LLM provider terms (all [V] unless noted)
- Google Gemini API, Additional Terms effective 23 Mar 2026: https://aistudio.google.com/docs/terms
  - Unpaid (free tier): content used to improve products, human reviewers may read it; "Do not submit sensitive, confidential, or personal information to the Unpaid Services." Exception: EEA/CH/UK get paid-terms.
  - Paid (billed Cloud project): prompts/responses not used to improve products; logged for a limited time for abuse/legal; processed under Google's DPA; logged data may be cached in any country where Google has facilities.
  - **Consequence: free-tier Gemini is unusable for any real-user data. Even synthetic data is fine, but never real slips.** Region: no Thai region guarantee; processing location not fixed in the page.
- OpenAI API: https://developers.openai.com/api/docs/guides/your-data : API data not used for training by default; abuse-monitoring logs up to 30 days; ZDR needs approval; Responses API `store` retains state at least 30 days unless set false; regional data residency exists (details elsewhere). Set `store=false`.
- Anthropic API: https://privacy.claude.com/en/articles/7996866-how-long-do-you-store-my-organization-s-data : inputs/outputs auto-deleted within 30 days by default; ZDR by agreement; policy-flagged content up to 2 years. Training-use statement for commercial data not on that page; check separate commercial terms. UNVERIFIED for training.
- DPA/region/ZDR terms usually require business account, not a student personal key [I].

### 4. LINE, Gmail
- LINE quota: push, multicast, broadcast, narrowcast count; reply messages do not. [V] https://developers.line.biz/en/docs/messaging-api/pricing/ (plans vary by country; Thai table not on this page).
- Thai plans: Free 300 msgs/month, Basic THB 1,280 (15,000), Pro THB 1,780 (30,000 or 35,000, sources disagree), extra messages THB 0.10/0.06. [S] https://website.z.com/th/en/lineoa/ ; https://business.true.th/en/solutions/line-business-solution . Not verified on LINE's Thai page (page I tried 404'd). Check Official Account Manager.
- Reported change: LINE Thailand notice dated 30 Oct 2026: from 1 Feb 2027, 25,000 free **Reply API** messages per OA per month; above that, pay via Agency Partner (Unlimited THB 5,000/month); must register via Agency Global Platform. [S, single secondary summary; original lineforbusiness.com/th/news/20261030 not opened] https://www.relevantaudience.com/social-media-marketing/line-reply-api-25000-free-messages-agency-global-platform/ . Pilot of ~20 users is far below 25k [I], but a demand to register with an agency partner may affect a student OA: UNVERIFIED.
- LINE content policy for financial content, LIFF rules, LINE Login data retention: I did not reach primary pages. UNVERIFIED. User-sent content is auto-deleted by LINE after some period (so fetch images promptly). [V] same messaging overview page.
- Gmail scopes: `gmail.readonly`, `gmail.metadata`, `gmail.modify` etc. are **restricted**; `gmail.send` sensitive. [V] https://developers.google.com/workspace/gmail/api/auth/scopes . Public apps storing/transmitting restricted data on servers need verification and a third-party CASA security assessment, reassessed at least every 12 months; verification "can take several weeks". [V] https://developers.google.com/identity/protocols/oauth2/production-readiness/restricted-scope-verification . Cost of CASA not on Google page: UNVERIFIED (commonly reported in the low-thousands USD per year [S, not checked]). Exceptions: personal use/few known users, and testing status with a test-user cap and short refresh-token life [V], number not stated. [I] A pilot on "Testing" mode with named testers works but tokens expire (about 7 days is commonly reported, UNVERIFIED) and users see warning screens. Recommendation: no Gmail OAuth in pilot.
- Gmail forwarding abuse (earlier review finding). Gmail verification sends a **link** to the forwarding address; whoever can read that mailbox clicks it. [V] https://support.google.com/mail/answer/10957 . If the backend auto-clicks the link, anyone who knows or guesses the user's bot address (e.g. `u123@in.example.com`) can set Gmail forwarding from their own account to it, or send mail directly, and inject fake bills. Safe design [I]:
  1. Never auto-confirm verification mails. Detect Gmail confirmation mails and show the code/link to the **already-authenticated** owner of that bot address in LINE/web; the user clicks.
  2. Unguessable per-user addresses (128-bit random token, not sequential or derived from name), rotatable, revocable.
  3. Bind sender: accept only after the user declares and confirms the sender mailbox; require SPF/DKIM/DMARC pass plus ARC for forwards from the original bank domain; drop mail failing authentication.
  4. Every parsed record from email lands in a "pending, unconfirmed" state like OCR-only data; never in trusted totals until the user confirms.
  5. Rate-limit, size-limit, strip HTML/active content, treat body as untrusted (prompt-injection) and never pass raw mail to the LLM without masking.
  6. Retention: store extracted fields, delete raw mail in days.
  Cheaper: defer all email ingestion (matches decision D-A2 option A).

### 5. Bank of Thailand "Your Data"
- [V] https://www.bot.or.th/en/financial-innovation/digital-finance/open-data.html : implementation begins late 2026 with personal deposit data; other categories (loans, payments incl. e-Money and credit cards) phased 2027-2028; consultation 30 Nov 2023; regulation enacted 11 Nov 2025 (per that page's news list); providers are BOT-supervised financial service providers; page does not name third-party/consumer-app eligibility or consent rules.
- Press: consent-based ("explicit consent of the individual") [S] https://www.nationthailand.com/business/banking-finance/40065294 (quote via search summary). Initial mandate binds BOT-supervised providers only [S].
- Whether a non-bank/student app can be a data consumer: not answered by BOT page. UNVERIFIED. Likely requires registration, accreditation, security standards and possibly a licence [I]. Treat as future work, as the scope already says. As of 2026-11-21 I found no confirmation that sharing has gone live [S, absence of evidence].

### 6. Slip verification
- EasySlip: requires KYC for production; verification returns QR payload, transaction ref, date, amount, **sender and receiver bank and account-holder names** (Thai/English); duplicate detection. Pricing, quota, terms, retention: not on the docs page. [V] https://document-partners.easyslip.com/en/guide/getting-started . The "about 100 slips/month free" claim in project docs stays [S, UNVERIFIED]. SlipOK: not opened.
- Slip data is personal data (names, partial account numbers; identifies counterparty who never consented) [I]. Sending the QR payload or image to a third party is a disclosure/processor relationship: needs notice, consent, DPA. Names of third parties (payees) are personal data of people not in your app [I]; minimise storage.
- QR payload parsing legality: the slip QR follows EMV/Thai QR conventions; I found no restriction on a user decoding their own slip locally. Local decode on the server of a user-submitted slip is not the same as calling bank APIs [I]. Legality of using bank-slip verification APIs under bank terms: UNVERIFIED. Offline checks (CRC, parse, user confirmation, dedupe on transaction ref) need no third party. Recommend skip third-party verification for pilot.

### 7. Other
- Apple App Store: money-management/investing apps "must be submitted by the financial institution that performs the services" and hold licences (3.2.1(viii)); highly regulated-field apps should come from a legal entity not an individual; privacy policy, consent, data minimisation; third-party AI sharing must be disclosed. [V] https://developer.apple.com/app-store/review/guidelines/ . A tracker that only displays user data is arguably not "performing" services [I]; reviewer discretion. LINE/web avoids store review.
- E-commerce registration: MoC notification effective 5 Jun 2024 exempts limited companies and certain entities from e-commerce registration; sources conflict on individuals; direct-marketing licence still mentioned. [S] https://www.tilleke.com/insights/thailand-exempts-certain-legal-entities-from-e-commerce-registration-requirements/ . Only relevant if charging; UNVERIFIED who must register.
- Charging: VAT, income tax of the earner, payment processor KYC, Consumer Protection (contract terms, unfair terms, Thai-language) UNVERIFIED -> lawyer. Free pilot avoids most.
- BOT draft AI risk guidelines for financial service providers exist; target regulated FSPs, not this app [S] https://www.tilleke.com/insights/thailand-issues-ai-risk-management-guidelines-for-financial-service-providers/ .
- Computer Crime Act / cybersecurity duties, Credit Information Business Act (if showing credit-bureau data; not planned) UNVERIFIED.
- Thai-law caution: wording/disclaimer in Thai must be in the UI, not only English.

## 2. Risk register

| # | Risk | Sev | Lik | Source (tag) | Mitigation | Owner (suggested) |
|---|---|---|---|---|---|---|
| R1 | AI output or indicators read as investment advice (suitability/buy-sell), esp. if charged | High | Med | Tilleke definition [S]; scope §4 | Fixed templates, phrase filter, no "good/bad/should", disclaimer in Thai, free pilot, lawyer reads output samples | Jay (product) + lawyer |
| R2 | Real-user data sent to free-tier Gemini (used for training, human review) | High | Med if student uses free key | Gemini terms [V] | Billed project only for any real data; env check blocks free key; synthetic only otherwise | Backend lead |
| R3 | Cross-border transfer to foreign LLM without valid mechanism | High | Med | PDPC notifs [S] | Provider DPA/SCC, consent text naming provider and country, minimise fields, record in ROPA | Omm (privacy) + lawyer |
| R4 | Masking assumed to remove PDPA duties; reversible mapping leaks | High | Med | scope §5; pseudonymisation [I] | Treat as personal data; consent+notice anyway; separate mapping store; log what was sent | Whole team |
| R5 | Breach (DB of balances, LINE IDs) not reported in 72h | High | Low | IAPP [S] | Incident runbook, named contact, encryption at rest, access logs, minimal retention | Backend lead |
| R6 | No retention/deletion rules; audit log conflicts with deletion (D-A7) | Med | High | consolidated §6.5 | Define periods (e.g. raw images 24-72h, logs 90d) as proposal; log masked payloads only; delete on request incl. provider state (`store=false`) | Omm |
| R7 | Gmail auto-confirm forwarding -> fake mail injection | High | Med if built | Gmail help [V]; design [I] | Do not build for pilot; if built, use safe design in §1.4 | Omm |
| R8 | Gmail restricted-scope verification + annual CASA infeasible | Med | High if OAuth used | Google [V] | No OAuth; testers only in Testing mode; or forwarding with user-clicked verification | Omm |
| R9 | Third-party slip API discloses sender/receiver names to a vendor with unknown terms | Med | Med | EasySlip docs [V] | Skip for pilot; local QR parse + user confirm; if used, DPA + consent | Backend lead |
| R10 | Payees' (third parties) personal data stored without their involvement | Low-Med | High | [I] | Store counterparty as user-given label; drop account numbers; mask | Backend lead |
| R11 | LINE costs/rules change (Reply API 25k free from Feb 2027; push 300 free) | Low for pilot | Med | relevantaudience [S] | Prefer reply over push; opt-in reminders; check OA Manager; budget cap | Omm |
| R12 | LINE/LIFF/Login policy for financial content unverified | Med | Low | not found: UNVERIFIED | Read LINE OA and Messaging API policies before launch | Omm |
| R13 | Minors using bot | Med | Low-Med | UNVERIFIED | 20+ declaration at onboarding, pilot invitations adults only | Jay |
| R14 | App Store rejection if native app | Low | Med | Apple [V] | Use LINE/web for pilot | Jay |
| R15 | Student team has no legal entity: contracts (DPA, LINE agency, EasySlip KYC) need a legal person; liability to users | Med | High | Apple 5.1.1(ix) [V] (entity); [I] | Advisor/university sponsor as entity or restrict to class demo; written pilot terms | Jay + advisor |
| R16 | Prompt injection via emailed/imaged content makes LLM output prohibited wording | Med | Med | [I] | Output filter independent of prompt; treat inputs as untrusted | Backend lead |
| R17 | "Sensitive" classification disputed (D-A5) | Med | Low | Linklaters [S] | Treat as high-risk, explicit consent; ask lawyer | Jay |
| R18 | Consumer-protection/e-commerce/tax duties on charging | Low (free) / Med | Low | Tilleke [S] | Do not charge in pilot | Jay |

## 3. Must do before a pilot with real users
1. Lawyer review session (1-2 hours) on §4 open questions, esp. advice boundary and cross-border.
2. Named legal entity or sponsor (advisor/university) as controller; contact person for PDPC/breach.
3. Thai-language privacy notice (what, why, retention, provider and country, rights, contact) and separate, withdrawable consent for (a) storage, (b) sending masked data to the LLM.
4. Paid/billed LLM account with DPA; verify training-off and retention for the chosen endpoint on its live page; set `store=false` (OpenAI) or equivalent; keep keys server-side.
5. Free-tier keys blocked in any environment holding real data.
6. Retention and deletion rules implemented: export and delete-my-data command; deletion covers raw images, summaries, logs.
7. ROPA table (even if exempt), cross-border record, DPA copies.
8. Breach runbook with 72-hour clock.
9. Descriptive-only enforcement tested: fixed templates, phrase filter, disclaimer in Thai on every AI output, audit log (masked).
10. Ban unconfirmed OCR/email data from totals; no ID card images; no third-party slip API unless DPA+consent.
11. Terms of use: adults only, informational only, no reliance, not advice, no payment.
12. Security basics: TLS, encryption at rest, per-user isolation, LINE webhook signature check, access control, secrets management.
13. Small, invited cohort; no charging; written pilot terms; ability to stop.
14. Check live LINE OA plan and the 1 Feb 2027 Reply API notice against your usage.
15. Any email ingestion built only with the safe design in §1.4, otherwise not in the pilot.

## 4. Safe to ignore for a class demo with synthetic data
- Consent flows, privacy notice, ROPA/DPO, DSR tooling, 72h breach runbook (still good practice; no real data subjects). [I]
- Cross-border transfer analysis (synthetic data is not personal data [I], provided no real names or numbers slip in).
- Gmail OAuth verification/CASA (use test accounts or skip).
- BOT Your Data eligibility (future-work slide).
- Slip verification vendor KYC/terms (use sample images).
- Licensing, e-commerce registration, VAT (no charging).
- App Store rules (demo on LINE/web/emulator).
- Do NOT ignore even in the demo: descriptive-only wording and disclaimer (advice boundary is a presentation risk too), no real people's data in screenshots or test sets (CLAUDE.md rule), do not put any real data on a free-tier LLM, and keep the "no competitor gap" claim unproven.

## 5. Open questions for a Thai lawyer
1. Does descriptive portfolio/debt analysis, shown as ratios and a payoff simulator, fall outside "investment advisory service"? Does a paid subscription or AI-written Thai text change that? Is any SEC no-action/ruling route available?
2. Do statements like "debt service ratio exceeds X%" count as suitability judgment? What neutral wording is safe?
3. Is financial data "sensitive" under PDPA, and what does the PDPC expect for explicit consent anyway?
4. Is sending masked/pseudonymised data to Gemini/OpenAI/Anthropic a cross-border transfer, and is a provider DPA plus consent sufficient absent an adequacy list?
5. Exact small-business exemption from ROPA (criteria, date) and DPO thresholds; does a student project with a sponsor count as controller?
6. Minors: age thresholds and parental-consent rules for under-20s.
7. Can the team lawfully process slips containing third-party names; is a counterparty notice needed?
8. Bank/e-wallet terms on slip QR parsing and use of verification APIs.
9. Consumer protection and e-commerce registration if a THB 79 plan is introduced; who must register with DBD.
10. Liability and entity structure for a pilot run by students (university IP and ethics rules).
11. Could a non-bank app become a "Your Data" consumer, and what accreditation applies?
12. Data retention duties (tax/accounting) versus deletion requests and audit logs.

## 6. Primary sources not reached (to open next)
Ratchakitcha text of the Securities and Exchange Act and PDPA B.E. 2562; SEC statement PDF (180924); PDPC announcements on cross-border and ROPA exemption; LINE OA Thai pricing and news 20261030; LINE Developers policy on financial content/LIFF; EasySlip/SlipOK pricing and terms; Anthropic commercial terms for training; CASA price list.
