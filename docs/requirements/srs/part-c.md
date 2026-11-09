# SRS v0.9, part C: constraints, assumptions, usability and localisation, transition and operations, documentation, verification, traceability

| Field | Value |
|---|---|
| Document | Software Requirements Specification (SRS) v0.9, part C of three. Parts A (`BR`, `STK`, `US`, `UC`, `IF`, `FR`) and B (`AI`, `QR`, `DR`, `SEC`, `PRV`, `LEG`) are written in parallel |
| Sections | §2.5 Constraints; §2.6 Assumptions, dependencies, out of scope; §3.4.4 Usability, localisation, accessibility; §3.8 Transition; §3.9 Operations; §3.10 Documentation and academic deliverables; §4 Verification; §5 Traceability; Appendix D Open issues; Appendix E Change log |
| Prefixes owned | `CON`, `ASM`, `DEP`, `OOS`, `UX`, `L10N`, `A11Y`, `TR`, `OPS`, `DOC`, `VER` |
| Date and status | 9 November 2026. **Proposed**, for team review. Not approved |
| Built from | `docs/combined/combined-proposal.md` v0.2 ("combined"); `docs/requirements/requirement-types-research.md` ("RT"); `docs/review/revised/omm/timeline-and-development-phases.md` v0.2 ("timeline") and `project-charter.md` v0.2 ("charter"); `docs/research/abdul-recon/manual-blackbox-test-script.md` ("black-box script") |

**Evidence tags.** `[V]` checked in a primary source (here, mostly as recorded in the judge-passed files above, with their dates); `[S]` secondary source; `[I]` our inference; `UNVERIFIED` not checked. Every number in a requirement is a **proposal for the team to agree** unless it is tagged as a fact. Hours are estimates.

**How to read a requirement.** Each table row is one requirement with these attributes (RT §2.3):

- **ID**: never reused. IDs taken from RT keep their meaning. Items from other parts are cited by their RT ID or `SEED-` ID only.
- **Type**: given by the table heading.
- **Statement**: one "shall" sentence.
- **Pri**: MoSCoW. **M** Must, **S** Should, **C** Could, **W** Won't. "M if built" means Must for any build that includes the named Should or Could item.
- **Trace up / source**: the business goal, law, platform rule or team decision it comes from. "G1–G4" are the goals in combined §3.3. "M1–M7" and "S0–S4" are the scope packages in combined §6.1.
- **Rationale**: why it exists.
- **Acceptance criterion**: the measurable condition that shows it is met.
- **V**: verification method. **I** Inspection, **A** Analysis, **D** Demonstration, **T** Test, **E** Evaluation on a labelled set, **U** User study (pilot).

The **Status** of every row is *Proposed*. **Owner** roles (PD product/delivery, TL technical lead, QO quality owner, PS privacy and security owner, AI AI lead) are in the §5 trace tables.

**What v0.9 contains** (RT §3). Full text for §2.5, §2.6 and §3.4.4. Must items only for §3.8 and §3.9; the rest is "moved to v1.0". A strategy and acceptance criteria for §4; test links come at the Phase 1 gate (`VER-01`). A skeleton for §5.

---

## 2.5 Constraints (`CON`)

Constraints are fixed limits from outside the design: time, people, money, platform, technology and course rules (RT §1.3.10; ISO/IEC/IEEE 29148 §9.6.16 [V, contents page]).

### 2.5.1 Time

Type: Constraint.

| ID | Requirement | Pri | Trace up / source | Rationale | Acceptance criterion | V |
|---|---|---|---|---|---|---|
| `CON-TIME-01` | The architecture and overall design shall be finalised and submitted with the proposal no later than Sunday 22 November 2026. | M | TEAM-BRIEF; charter G2; timeline §4 | Course deadline. 22 Nov is a Sunday, so the target is "ready on Friday 20 Nov" | A submission receipt dated on or before 22 Nov 2026 exists, and the G2 approval record is dated on or before 20 Nov 2026 | I |
| `CON-TIME-02` | The evaluated prototype and the final report shall be delivered no later than 30 April 2027. | M | TEAM-BRIEF; charter G8–G9 | Project end date, confirmed by the team | A G9 closure record and a final-submission receipt are dated on or before 30 Apr 2027 | I |
| `CON-TIME-03` | No new feature work shall start after the feature-freeze date chosen in TD4 (14 Feb 2027 under option A, 14 Mar 2027 under option B), and after 11 April 2027 only defect fixes and closure work shall be done. | M | timeline §9; combined TD4 | Protects the evaluation window and the Songkran-shortened stabilisation week (13–15 Apr 2027 [S, timeline §2]) | The issue log shows no feature issue opened after the freeze date, except one recorded change request that removes equal work (charter §9); no feature commit after 11 Apr 2027 | I |

### 2.5.2 Team capacity

Type: Constraint.

| ID | Requirement | Pri | Trace up / source | Rationale | Acceptance criterion | V |
|---|---|---|---|---|---|---|
| `CON-TEAM-01` | The Must build-and-test effort (packages M1–M5) shall be estimated at no more than 160 person-hours at the high estimate, within the 120–160 person-hour ceiling. | M | RT `CON-TEAM-01`; combined §6.2 | Three students at 3–4 h a week; build and test may take only about half the total | The bottom-up table in combined §6.2 shows M1–M5 at 80–113 h (midpoint 96.5 h), or 89–125 h with S0. Re-checked with actual hours at the end of P1 | A |
| `CON-TEAM-02` | The midpoint estimate of the whole Must scope (M1–M7) shall not exceed the usable capacity of the planning scenario in force, which is scenario C at 3.5 h per person per week (141.75 h) until the real calendar is recorded at A11. | M | combined §6.3; timeline §2 | Exam dates are unknown, so we plan for the worst recorded case | 134 h ≤ 141.75 h (margin 7.75 h) at the proposal. At each two-weekly forecast, remaining Must work ≤ remaining usable capacity; otherwise the cut order in combined §6.5 is applied and recorded | A |
| `CON-TEAM-03` | Plans and forecasts shall count at most 4 hours per member per week, and shall count any extra hours only when the member has volunteered them and they are recorded in the forecast. | M | timeline §9 "No hidden overtime"; charter §10 | Keeps the plan honest and contributions equal | Each forecast sheet shows hours per member ≤ 4 a week, or a dated note of volunteered extra hours | I |

### 2.5.3 Budget

Type: Constraint.

| ID | Requirement | Pri | Trace up / source | Rationale | Acceptance criterion | V |
|---|---|---|---|---|---|---|
| `CON-BUD-01` | Total running costs during the pilot (LLM API, hosting and database, LINE Official Account plan) shall not exceed the monthly cap approved by the team at D05/D06 (16 Nov 2026). | M | RT `CON-BUD-01`; charter §10 budget table; `AI-COST-01`, `AI-COST-02` | No spending authority exists yet (charter §10). The LLM per-user budget (`AI-COST-01/02`, part B) must sum to a cap this row sets. **Cap value is open (Appendix D, OI-08)** | The monthly cost report (`OPS-07`) shows actual spend ≤ the approved cap for every month from P1 to P8 | I, A |
| `CON-BUD-02` | During the evaluation window, the LINE Official Account shall be on a plan whose monthly message allowance is at least the forecast number of push messages for that month. | M | combined TD5, §7.5; `IF-LINE-02` | Free plan: pushes stop at 300 a month [V, LINE Thailand OA plans, via combined §7.5]. Forecast for 20 testers at about one push a day for 4 weeks is about 560 [I]. Basic plan: ฿1,280 + 7% VAT a month, 15,000 messages [V, via combined TD5] | Before G6, an analysis shows forecast pushes ≤ plan allowance (TD5 option A: Basic plan; option B: Free plan with a cap of 15 pushes per tester for 20 testers). During the pilot, `OPS-06` alerts confirm the allowance was never reached | A, T |

### 2.5.4 Platform

Type: Constraint.

| ID | Requirement | Pri | Trace up / source | Rationale | Acceptance criterion | V |
|---|---|---|---|---|---|---|
| `CON-PLAT-01` | The only user channel shall be a LINE Official Account using the Messaging API; there shall be no separate mobile app. | M | RT `CON-PLAT-01`; combined §2 | Users are already in LINE (54 million monthly active users in Thailand [V, LY Corp, via combined §2]) | Every Must user flow in the demo script starts in the OA chat; no app-store build exists | I, D |
| `CON-PLAT-02` | Any flow that uses the LINE `oaMessage` handoff shall also offer the copy-and-send fallback, because the handoff works only in the LINE mobile app and not in LINE for PC. | M | combined §4.4 [V, LINE URL scheme docs and judge spot-check, 9 Nov 2026] | A user on a PC must still be able to send the request | Design inspection of the router shows both adapters on every request path; one test on LINE for PC shows the fallback | I, T |
| `CON-PLAT-03` | The design shall not depend on our bot posting into a business's chat, into a LINE group, or through the user's own account. | M | combined §4.4 [V, LINE group-chats docs]; `LEG-02` | A bot cannot write into another OA's chat; only one OA can be in a group; proxy sending through the user's account is forbidden by LINE's User Data Policy s.3.3 [V, via RT] | Inspection: every outbound path to a business is a user-tap handoff, the copy fallback, or (S2) a human operator | I |

### 2.5.5 Technology

Type: Constraint (design constraint).

| ID | Requirement | Pri | Trace up / source | Rationale | Acceptance criterion | V |
|---|---|---|---|---|---|---|
| `CON-TECH-01` | The system shall be built from one web application, one worker process, one PostgreSQL database and one hosted LLM API, using the stack and providers chosen at D05 and D06 (16 Nov 2026). | M | combined §7.1 "Stack"; omm decisions D05, D06 | Fewer moving parts fit 3–4 h a week. Temporal and other workflow engines are out of scope (`OOS-09`) | The deployment configuration at G3 lists only these four runtime parts plus LINE | I |
| `CON-TECH-02` | The LLM provider chosen at D06 shall offer API terms under which our prompts and outputs are not used for training and are retained for no longer than abuse monitoring requires (zero retention where offered). | M | combined §7.1 (recon P-10); `PRV-05`, `PRV-02` | Prompts carry personal data sent abroad; the transfer basis (PDPA s.28) is still open (OI-02) | The D06 decision record quotes the provider's current terms with an access date and the retention period they state | I |

### 2.5.6 Academic and research rules

Type: Constraint.

| ID | Requirement | Pri | Trace up / source | Rationale | Acceptance criterion | V |
|---|---|---|---|---|---|---|
| `CON-ACAD-01` | The repository shall contain no real personal data; test data shall be synthetic or tester-consented and, if consented, stored outside the repository. | M | TEAM-BRIEF rule 5; combined §7.5 | Course and PDPA risk; the repository is shared | At each gate, a search of the repository for phone numbers, LINE user IDs and real names finds none; the review is recorded | I |
| `CON-ACAD-02` | Any study of Abdul shall use only public information and ordinary, hand-typed use of advertised features, within the TEAM-BRIEF research boundary. | M | TEAM-BRIEF "Abdul research boundary"; Abdul terms (forbid reverse-engineering, scraping, probing) | Respect the competitor's terms; keep E13 defensible | The black-box run notes show hand use only, at most about 10 messages per 10 minutes, no automation, no prompt-injection text, and the T39–T40 clean-up done | I |
| `CON-ACAD-03` | The proposal and the final acceptance shall be approved by the authority the course names (advisor or committee), at G2 and G8. | M | charter §4 (AD role), §8 | The advisor's decision prevails over the team's own sign-off | Signed or emailed approval records for G2 and G8 exist. The course's approval rules are UNVERIFIED (OI-09) | I |

`LEG-04` (ethics approval before any pilot) and `LEG-05` (testers aged 20 or over, or married) are academic and legal constraints too; they live in part B.

---

## 2.6 Assumptions, dependencies and out of scope (`ASM`, `DEP`, `OOS`)

These are not requirements. They are recorded because a requirement that rests on a false assumption fails silently (RT §1.3.16). Each has an owner role and a date by which it is checked. All are reviewed at every phase gate.

### 2.6.1 Assumptions

| ID | Assumption | Status | Requirements that depend on it | Check (how, owner, by when) |
|---|---|---|---|---|
| `ASM-01` | Businesses will answer a request card, in their own chat or (S0) on the reply page, without signing up. | UNVERIFIED | `STK-03`, `SEED-FR-07`, E11 | E11 field result; 3–5 friendly shops asked in P3. PD, by G7 |
| `ASM-02` | The LINE share-target picker **cannot** be relied on to deliver a card to a business's OA chat. The design does not depend on it. | UNVERIFIED | C4 adapter only | P1 spike on testers' phones. TL, by G3 (13 Dec 2026) |
| `ASM-03` | LINE and other apps may fetch a URL posted in a chat to build a link preview, so a GET on the reply page can happen without a person. | UNVERIFIED for LINE in this case; rule is cheap either way | `SEED-SEC-01`, `SEED-FR-05`, E6 | Link-preview test: post the reply-page link into a test LINE chat and check that no state changes. TL, before S0 is released |
| `ASM-04` | The `oaMessage` URL opens the shop's chat with our pre-filled Thai text on testers' phones; the user can edit the text before sending; we cannot see whether it was sent. | Behaviour [V, LINE URL scheme docs]; on testers' devices UNVERIFIED | `SEED-FR-04`, `SEED-FR-05` | P1 spike on at least one iOS and one Android phone. TL, by G3 |
| `ASM-05` | The university calendar has two two-week exam periods inside 23 Nov 2026–25 Apr 2027, worked at half capacity (scenario C). | UNVERIFIED (exam dates not recorded) | `CON-TEAM-02`, phase plan | Record the real academic calendar (action A11). PD, by 13 Nov 2026 |
| `ASM-06` | The university's human-participant approval can be obtained during P2 and before G6 (26 Feb 2027), and covers businesses as third-party data subjects. | UNVERIFIED (process unknown) | `LEG-04`, M6, `VER-03` | Ask the advisor (action A12). PS, by 13 Nov 2026 |
| `ASM-07` | Each member sustains 3–4 h a week, and a 25% allowance covers coordination and availability changes. | Estimate | `CON-TEAM-02` | Two-weekly forecast; actual hours per line compared at the end of P1. PD |
| `ASM-08` | 15–20 testers aged 20 or over (or married) can be recruited from staff, later-year and graduate students by 28 Feb 2027. | UNVERIFIED | `VER-03`, E8–E10 | Recruitment count at G6. Evaluation lead |
| `ASM-09` | 3–5 friendly shops near campus with a known LINE ID agree in advance to receive request cards. | UNVERIFIED | E11, `TR-07` | Signed or recorded agreement in P3. Evaluation lead |
| `ASM-10` | Testers average about one push message a day. | Estimate [I] | `CON-BUD-02`, `OPS-06` | Push counter during P5 (E5) |
| `ASM-11` | Testers use LINE on a smartphone for the request flow. | [I] | `CON-PLAT-02` | Screening question at onboarding (`TR-02`) |
| `ASM-12` | The course does not require a working implementation at the proposal stage. | UNVERIFIED (rubric not seen) | `DOC-01` | Obtain the rubric (OI-09). PD, by 13 Nov 2026 |

### 2.6.2 Dependencies

| ID | We depend on | Needed by | Fallback |
|---|---|---|---|
| `DEP-01` | LINE Messaging API behaviour as documented on 9 Nov 2026: signed webhooks, at-least-once delivery, push with `X-Line-Retry-Key` [V, LINE docs via combined §14] | P1 | None for the channel; contract tests (`VER-07`) detect changes |
| `DEP-02` | LINE keeps its Messaging API pricing model through April 2027. Reply API charges above 25,000 replies a month from 1 Feb 2027 [V, LINE Thailand notice, 30 Oct 2026] are far above our volume [I] | P5 | Re-cost under `CON-BUD-01` |
| `DEP-03` | An LLM provider that meets `CON-TECH-02`, chosen at D06 (16 Nov 2026) | P2 | Structured question flow without AI (`SEED-AI-01`); reminders keep working |
| `DEP-04` | A hosting and PostgreSQL provider chosen at D05 (16 Nov 2026), with daily backups available | P1 | Self-managed database dump to storage (`OPS-02`) |
| `DEP-05` | University human-participant approval (`LEG-04`) | G6, 26 Feb 2027 | Synthetic sessions only; report the reduced evidence |
| `DEP-06` | Advisor or committee approval at G2 and G8 (`CON-ACAD-03`) | 20 Nov 2026; 23 Apr 2027 | Escalate early; record the decision |
| `DEP-07` | Funding for the LINE Basic plan for the evaluation months (TD5 option A, about ฿2,740 for two months [I]) | 28 Feb 2027 | TD5 option B (Free plan, 15 pushes per tester) |
| `DEP-08` | Team decisions TD1–TD6 by 13 Nov 2026; D05, D06 by 16 Nov; D08 by 18 Nov | P0 | The recommendations in combined §13 are used as defaults and recorded |
| `DEP-09` | Advice from the advisor or the university legal office on the PDPA s.28 transfer basis and on Computer-Related Crime Act s.26 | G6 | Keep minimal traffic metadata separable so either answer can be applied (OI-01) |

### 2.6.3 Out of scope (this project)

From combined §6.1 "Won't" and §12. Each is a future-work item, not a hidden requirement.

| ID | Out of scope | Why |
|---|---|---|
| `OOS-01` | AI voice phone calls, SMS and email sending; holding or moving money (payments, subscriptions, wallets) | Legal review, cost and licensing (`LEG-03`); no evidence of demand yet |
| `OOS-02` | Level 3 direct booking, partner APIs, browser agents for web forms, merchant dashboard | Needs offer binding, reconciliation and legal review (combined §4.1) |
| `OOS-03` | The bot inside LINE groups: group polls, group reminders, group summaries | One OA per group (`CON-PLAT-03`); data rules for group members |
| `OOS-04` | Google Calendar or Gmail OAuth | Google verification effort; `.ics` file (C3) covers the need |
| `OOS-05` | Venue recommendations from Google Places or Wongnai data | Maps ToS s.3.2.3 and Wongnai terms [V, via combined §10]; `SEED-LEG-01` |
| `OOS-06` | Escalation-until-acknowledged reminders, วันพระ recurrence, voice-note input | Hours; Abdul parity is stated as partial in E13 |
| `OOS-07` | Medical requests and testers under 20 (unless married) | PDPA s.26 sensitive data; `LEG-05` |
| `OOS-08` | LINE Login, LIFF or any sign-up on the business-facing reply page | `STK-03` (no sign-up); the page never collects the business person's LINE ID (combined §7.5) |
| `OOS-09` | Temporal or other workflow engines; 24/7 support; service-level promises | `CON-TECH-01`; charter §11 "no assumed 24/7 support" |

---

## 3.4.4 Usability, localisation and accessibility (`UX`, `L10N`, `A11Y`)

Chat bubbles are drawn by LINE, so in chat we control only words, button labels, card layout and the alternative text. Web pages we serve (the privacy notice page, the S0 reply page, C5 LIFF pages) are fully ours, so WCAG 2.2 applies to them directly (RT §1.3.13).

### 3.4.4.1 Usability (`UX`)

Type: Quality requirement, interaction capability (ISO/IEC 25010:2023).

| ID | Requirement | Pri | Trace up / source | Rationale | Acceptance criterion | V |
|---|---|---|---|---|---|---|
| `UX-01` | After the assistant's first reply in a task, the user shall be able to choose an option, confirm, cancel, withdraw and pick the field to edit with quick-reply or card buttons alone, without typing. | M | RT `UX-01`; combined §7.2; G1 | Typing on a phone is slow; buttons reduce errors. Entering a new free-text value (for example a new shop name) may still need typing; dates use LINE's date-time picker action where available (UNVERIFIED in this run) | In the demo script for M2 and M4, every step after the first reply is completed by taps only, except entering a new free-text value; test log shows no other typed step | D, T |
| `UX-02` | Every message and card that shows a task's status shall use the fixed label for that state from the label table (combined §4.2), in Thai and English, and shall never use "booked", "จองแล้ว", "sent" or "ส่งแล้ว" for a state below the one that label belongs to. | M | combined §4.2; G3; E6 | Honest status is the product's main promise | Inspection: each status template maps to exactly one state in the label table. E6 audit: 0 tasks shown as sent without "Yes, sent" or a POST answer, 0 shown as booked or done without the matching evidence level | I, T |
| `UX-03` | Each confirmation card and request card shall state, in one plain sentence above its buttons, what will happen, to whom and when. | M | `FR-ACT-01`; `AI-HITL-01`; combined §3.3 anti-goal "assistant users must keep correcting" | Users confirm what they understand; ETA s.15 para 2(2) makes the sent card the user's own words [V, via RT] | Inspection of all card templates against a three-point checklist (what, to whom, when). In each of the 3 moderated sessions, the participant correctly explains, before tapping Confirm, what the card will do (3 of 3) | I, U |
| `UX-04` | Testers shall complete the capture → confirm → reminder flow without help from the team. | M | E8; G1; `BR-02` | Effectiveness of the core flow | E8: share of pilot testers who complete the flow unaided ≥ 80% (point estimate), reported with a 95% Wilson interval and the §4.4 outcome label. Second measure, same rule: ≥ 70% take a request to a known outcome (answered, declined or clearly no reply) | U |
| `UX-05` | A message shall become a confirmed, tracked task within 2 minutes (median), counted from the user's first message to the Confirm tap. | M | E9; G1 | Speed is the reason to use an assistant | E9: median ≤ 2 min over all tester captures in the pilot, with a 95% distribution-free (order-statistic) interval reported | U, T |
| `UX-06` | Testers' mean System Usability Scale (SUS) score shall be at least 68. | S | E10; RT §1.3.13 | 68 is a commonly cited average (UNVERIFIED in this run; OI-12). A miss is reported as a finding, not a release blocker, hence Should | E10: SUS from all testers; mean ≥ 68 (point estimate) with a 95% t-interval reported and the §4.4 outcome label. With n = 15–20 the half-width is about ±8 points [I, assuming SD ≈ 17] | U |
| `UX-07` | The assistant shall send no more than 40 push messages per tester per month. | S | E5; jui's ≤ 40 target (combined E5); anti-goal "reminders that become another source of stress" | Notification load; also bounds `CON-BUD-02` | E5 push counter: maximum pushes for any tester in any calendar month ≤ 40; the distribution per tester is reported | T, U |

`QR-IC-01` and `QR-IC-02` (help in one tap; every error message states what failed and one next step) are in part B and are also checked in the moderated sessions.

### 3.4.4.2 Localisation (`L10N`)

Type: Quality requirement, cultural and localisation (Volere §16 [V, via RT]).

| ID | Requirement | Pri | Trace up / source | Rationale | Acceptance criterion | V |
|---|---|---|---|---|---|---|
| `L10N-01` | The assistant shall reply to the user in the language (Thai or English) of the user's last message, unless the user has fixed a language with a chat command. | M | RT `L10N-01`; TD1 testers | Mixed Thai and English is normal; replies must be readable | Test: 20 scripted turns (10 Thai, 10 English, including a language switch and the fix command) produce replies in the expected language 20 of 20 times. Mixed messages follow the language of the majority of words, a rule written in the test spec | T |
| `L10N-02` | The assistant shall accept Buddhist-era (BE) and Common-era (CE) years and shall show every date in a confirmation card in both forms, for example "จ. 14 ธ.ค. 2569 / Mon 14 Dec 2026". | M | RT `L10N-02`; combined §4.3 | A wrong year on a reminder is a real error. BE = CE + 543 [S] | Unit tests: 2569 and 2026 give the same date; "5 ม.ค. 2570" gives 5 Jan 2027 (a Tuesday). Every card template renders both forms. Year items in E1 meet the E1 date target | T, E |
| `L10N-03` | The assistant shall interpret all 12 Thai month names in full and abbreviated form, Thai relative day words (วันนี้, พรุ่งนี้, มะรืน) and Thai clock words (โมงเช้า, เที่ยง, บ่าย…โมง, …ทุ่ม, ตี…), and shall ask the user when a date word is ambiguous (for example ศุกร์หน้า, or a two-digit year such as "69"). | M | combined §6.1 M2; `AI-HAL-02`; E2; black-box script T04–T08, T42 | Thai time words differ from a 24-hour clock (2 ทุ่ม = 20:00, ตี 2 = 02:00); some phrases are truly ambiguous | Unit tests: 24 month forms map correctly (24 of 24); clock-word table maps correctly (100%). The ambiguity rule is measured by E2 (100% of 20 ambiguous items are asked) | T, E |
| `L10N-04` | The system shall interpret and display every date and time in the Asia/Bangkok time zone (UTC+7) and store each due time as an absolute instant. | M | combined §4.3; timeline header "Timezone: Asia/Bangkok" | Server time zones differ; Thailand has no daylight saving time [S] | Unit tests run with the server zone set to UTC and to another zone give identical displayed times; a reminder set for "พรุ่งนี้ 9 โมง" fires at 09:00 Bangkok time within the `FR-REM-01` tolerance | T |
| `L10N-05` | The assistant shall accept baht amounts written with or without thousands separators, decimals, "บาท", "฿" or "THB", and shall show amounts in cards as a number with thousands separators followed by "บาท" (Thai) or "THB" (English). | M | combined §8.1 example "ค่าไฟ 1,245 บาท"; E1 amount target | Amounts drive consequential fields (`AI-HAL-01`) | Unit tests on at least 10 written forms; E1 amount exact-match target | T, E |
| `L10N-06` | Request-card text addressed to a business shall be written in polite Thai by default, whatever the user's chat language, and the user shall be able to switch that card to English before confirming. | S | combined §4.4 (Thai pre-filled text); TD2 shops near campus | Shops near campus read Thai; politeness affects the answer rate | Inspection of the card template; test: an English-speaking user's card is Thai unless switched; switching creates a new card version needing a new Confirm (`AI-HITL-02`) | I, T |
| `L10N-07` | The assistant shall accept Thai digits (๐–๙) in dates, times and amounts. | C | Thai writing practice [I] | Some users type Thai digits | Unit tests with Thai-digit inputs give the same result as Arabic digits | T |

### 3.4.4.3 Accessibility (`A11Y`)

Type: Quality requirement, inclusivity and user assistance (ISO/IEC 25010:2023); WCAG 2.2 [V, w3.org/TR/WCAG22, via RT].

| ID | Requirement | Pri | Trace up / source | Rationale | Acceptance criterion | V |
|---|---|---|---|---|---|---|
| `A11Y-01` | Every web page the system serves (the privacy notice page; the S0 reply page and C5 LIFF pages if built) shall meet WCAG 2.2 Level AA, including 2.5.8 Target Size (Minimum) and 3.3.8 Accessible Authentication (Minimum). | M (M if built for S0, C5) | RT `A11Y-01`; `LEG-01`; `STK-03` | Web pages are fully under our control; businesses and users may use screen readers or large text | (a) An automated checker (axe-core or similar) reports 0 violations for the WCAG 2.2 A and AA rules on each page; (b) a manual check passes: TalkBack or VoiceOver reads every control with a name, targets are at least 24 × 24 CSS pixels (our reading of 2.5.8, to recheck against w3.org), text reflows at 320 CSS px width and 200% zoom, page language is set (`lang="th"` or `"en"`). The reply page has no login at all, so 3.3.8 is met by design (`OOS-08`) | T, I |
| `A11Y-02` | Every Flex message shall carry alternative text that states, in plain words, the card's action and its date or status. | M | LINE Flex `altText` field (required by the API, UNVERIFIED in this run); `UX-03` | The alternative text is what notifications and some assistive tools show | Unit test over all card templates: alternative text is non-empty and contains the action word and the date or status label | T |
| `A11Y-03` | Status and warnings shall be stated in words, never by colour, icon or emoji alone. | M | WCAG 1.4.1 Use of Color (for web pages); `UX-02` | Colour-blind users and screen readers | Inspection of all templates and pages: every coloured or icon-only cue has a text equivalent | I |

---

## 3.8 Transition (`TR`)

At v0.9 this section holds only the Must items needed to start and end the pilot safely. **The rest moves to v1.0**: environment matrix, release and rollback steps, deployment runbook, seed-data scripts (RT §3).

Type: Transition requirement.

| ID | Requirement | Pri | Trace up / source | Rationale | Acceptance criterion | V |
|---|---|---|---|---|---|---|
| `TR-01` | There shall be separate development and pilot LINE channels, with separate secrets and databases, so that tests never message pilot users. | M | RT `TR-01`; combined M1 | Protects testers from test traffic | Inspection at G3: two channels, two configurations; a test run on the development channel produces 0 pushes on the pilot channel's counter | I, T |
| `TR-02` | Pilot onboarding (add friend → privacy notice → consent on its own screen → first task) shall take a median of under 3 minutes. | M | RT `TR-02`; `PRV-01`, `PRV-08` | A long start loses testers; calendar linking is out of scope (`OOS-04`) | Timed in the 3 moderated sessions and from event timestamps for all testers; median < 3 min | U, T |
| `TR-03` | At the end of the pilot, the team shall delete or anonymise all pilot data within 30 days and tell users how; audit logs and anonymised evaluation data are kept until the evaluation report is submitted, then deleted; every shorter limit still applies. | M | RT `TR-03`; combined §7.4; PDPA s.37(3) [V, via RT] | Data minimisation at cutover. **Slip images and screenshots are kept 30 days or until the E6 audit of that task, whichever is later, and never beyond this deletion** (pending fix to combined §8.1) | A deletion script is run and logged within 30 days of the pilot's end; a database query after the run finds no pilot personal data except the retained audit records; the closing message (`TR-05`) was sent | T, I |
| `TR-04` | A tester's pilot access shall be activated only after the team has recorded, outside the repository, that the tester's consent and age-eligibility confirmation exist. | M | `LEG-04`, `LEG-05`, `PRV-08`; `CON-ACAD-01` | Onboarding must not run ahead of approvals | Inspection before G6 and weekly in P5: every active pilot user has a matching consent record; 0 exceptions | I |
| `TR-05` | When the pilot ends, the system shall send each tester one closing message saying that the pilot has ended, what will happen to their data and when, and how to ask for an export, and shall then cancel all scheduled pushes for that tester. | M | `TR-03`; `PRV-03`; charter §11 | Clean offboarding; no stray reminders after the pilot | Test on the development channel, then a pilot-channel log showing one closing message per tester and 0 jobs left scheduled | T |
| `TR-06` | Before P5, the venue table shall be seeded with 10–15 venues whose name, LINE ID or phone and opening hours were hand-checked from the shop's own sign, page or OA profile, each with a last-verified date. | M | combined M4, §7.3; `SEED-LEG-01` | The router needs real channels; no Google Places or Wongnai content may be copied | Inspection: 10–15 rows, each with a source note and a last-verified date within 30 days before the evaluation starts | I |
| `TR-07` | The friendly shops shall be told before the pilot that request cards may arrive, and told after the pilot that it has ended; a Stop answer shall block further cards to that venue. | S | ASM-09; combined §9.3; `SEED-FR-07` | Businesses are third-party data subjects who never accepted our terms | Records of the start and end notices; a test that a Stop answer blocks a new card to that venue | I, T |
| `TR-08` | No real tester shall be given access before the G6 readiness checklist is signed off. | M | charter G6; timeline §8 P4 | Safety and approvals first | The G6 record is dated before the first pilot user's follow event | I |

---

## 3.9 Operations and support (`OPS`)

At v0.9 this section holds only the Must items. **The rest moves to v1.0**: runbooks, published support windows, uptime target, rollback drill.

Type: Operational requirement.

| ID | Requirement | Pri | Trace up / source | Rationale | Acceptance criterion | V |
|---|---|---|---|---|---|---|
| `OPS-01` | Failed webhook handling, failed jobs, failed pushes and LLM errors shall raise an alert in the team's chat within 5 minutes. | M | RT `OPS-01`; combined M1 | Nobody is on call; the system must report its own failures | Alert drill before G6: each of the four faults is injected on the development channel and an alert arrives within 5 minutes (4 of 4) | T |
| `OPS-02` | The database shall be backed up at least once a day. | M | RT `OPS-02`; charter acceptance "Recovery" | Limits data loss to 24 hours in the declared disaster scenario | Backup log shows one successful backup per day over any 14-day window in P4–P5 | I |
| `OPS-05` | Before G6, the team shall restore the database from a daily backup in a drill and record the time taken. | M | RT `OPS-02`; charter acceptance "Recovery"; combined M5 | A backup that has never been restored is unproven | Drill record: restore completed within 4 staffed working hours, data loss ≤ 24 hours, and a check query confirms the restored task count | T |
| `OPS-03` | A tester shall be able to type "help" or "ติดต่อทีม" to reach the team, and the team shall answer within 2 working days during the pilot. | M | RT `OPS-03`; combined M6 (`OPS-03` pilot support); charter §11 | There is no 24/7 support; the response window must be stated honestly | Support log for P5: every request answered within 2 working days; the welcome message states the window | I, T |
| `OPS-04` | The team shall keep a one-page incident procedure that includes notifying the PDPC Office of a personal-data breach within 72 hours of becoming aware of it, where feasible, as PDPA s.37(4) requires. | M | RT `OPS-04`; PDPA s.37(4) [V, unofficial translation, via RT]; LINE Developers Agreement §5.2.2 (report unauthorised access) [V, via RT] | Someone must notice and act on a breach within the legal window. Whether data subjects must also be told in high-risk cases is in s.37(4) as we read it (UNVERIFIED detail) | The procedure exists before G6 and names an owner and a backup; a tabletop drill before G6 reaches the "notify" step within the 72-hour clock on paper | I, D |
| `OPS-06` | The system shall count push messages sent per month and per tester, and shall alert the team when 80% of the plan's monthly allowance is used. | M | combined §7.5, E5, TD5; `CON-BUD-02`; `IF-LINE-02` | On the Free plan, pushes stop at 300 a month [V, via combined §7.5] | Test with the threshold lowered: the alert fires at the 80% mark. During P5, the counter agrees within 2% with LINE's "number of messages sent this month" endpoint [V endpoint exists, via RT §1.3.5] | T |
| `OPS-07` | The team shall produce a monthly cost report for the LLM API, hosting and the LINE plan, compared with the `CON-BUD-01` cap. | M | `CON-BUD-01`; `AI-COST-02`; charter §10 | Cost overrun is a named risk | One report per month from P1 to P8, each with actuals, forecast and the cap | I |
| `OPS-08` | The team shall be able to pause all outbound pushes and handoffs with one configuration switch, without deploying code. | M | charter §11 "provide a stop/disable route" | Lets the team pause the pilot when an incident cannot be handled safely | Drill before G6: switch on → a scheduled reminder is held, not sent; switch off → it is sent once (no duplicate) | T |

---

## 3.10 Documentation and academic deliverables (`DOC`)

These are project requirements, not product requirements (RT §1.3.14). The schedule and owners live in the project plan: timeline §4 (proposal window) and §5 (phases), and charter §8 (artefacts) and §11 (deliverables). This section lists only what must exist and when.

Type: Documentation requirement.

| ID | Requirement | Pri | Trace up / source | Rationale | Acceptance criterion | V |
|---|---|---|---|---|---|---|
| `DOC-01` | By 22 November 2026, the team shall submit a proposal package containing the problem, scope and exclusions, SRS v0.9 (every Must requirement with an acceptance criterion), architecture diagram, data model, threat model, risk list, test plan, traceability skeleton and plan. | M | RT `DOC-01`; charter §11; `CON-TIME-01` | Course deliverable | Checklist against charter §11 and the course rubric (OI-09): every item present in the submitted package | I |
| `DOC-02` | By 30 April 2027, the team shall deliver the final package: SRS v1.0 (baselined), design document, test and evaluation report (with AI results per action), user guide in Thai, README for running the system, demo video, final report and handover record. | M | RT `DOC-02`; charter §11 "closure"; `CON-TIME-02` | Course deliverable; the grade depends on it | Checklist at G8: every item present and reviewed by another member | I |
| `DOC-03` | Every architecture decision shall be recorded as a short decision record (ADR: context, decision, consequences). | M | RT `DOC-03`; charter §9 decision log | Keeps the design traceable (§5 "design element" column) | Inspection at each gate: each decision D01–D10 and each later material change has an ADR file | I |
| `DOC-04` | A test plan giving test levels, environments, entry and exit criteria, and the Thai evaluation-set size and policy shall be approved at G2. | M | charter §8 artefacts; timeline §7 | Required for G2; §4 of this SRS is its basis | G2 record lists the test plan as approved | I |
| `DOC-05` | A user guide for testers, in Thai with an English version, covering what the assistant can and cannot do, the status words, Withdraw, help and data rights, shall be ready before G6. | M | RT `DOC-02` (user guide); `UX-02`; `OPS-03` | Testers need it at onboarding, not in April | Guide exists before G6 and is reviewed by a member who did not write it | I |
| `DOC-06` | The final report shall include the Abdul comparison (E13), the evaluation results with their intervals and outcome labels, and a statement that the sample does not represent all Bangkok working adults. | M | combined §3.2, E13; `VER-05`; `VER-08` | Honest reporting of a small, non-representative sample | Inspection of the final report against these three items | I |
| `DOC-07` | The design document shall be updated at every gate from G3 to G8 so that it matches the built system. | S | combined M7 "design docs kept current"; charter DoD | Avoids writing everything in the last week | Each gate record links a design-document version dated within that phase | I |

---

## 4. Verification (`VER`)

### 4.1 Strategy: methods by category

Verification follows ISO/IEC/IEEE 29148 §9.6.19 [V, contents page] with the six methods defined at the top of this document. Rule of thumb: **test** what code decides, **evaluate** what the model guesses, **user-study** what people do, **inspect** documents and settings, **analyse** budgets and capacity.

| Category | Main methods | Typical evidence |
|---|---|---|
| `FR`, `IF` (part A) | T, D | Unit and integration tests; contract tests; demo script |
| `AI` (part B) | E, T | Held-out evaluation runs (E1, E2, E4); code inspection that the approval gate sits before every adapter |
| `QR-RE`, `QR-PE` (part B) | T | Fault injection, replay tests, scheduler timing (E5) |
| `SEC`, `PRV`, `LEG` (part B) | T, I | Isolation tests (E7); token tests; deletion tests; compliance checklist |
| `DR` (part B) | I, T | Schema against the data dictionary; retention jobs with time-shifted data |
| `CON` | I, A | Forecast sheets, cost reports, decision records |
| `UX` | U, D | Pilot logs (E8, E9), SUS (E10), moderated sessions |
| `L10N` | T, E | Unit tests for dates, months, amounts; E1 and E2 items |
| `A11Y` | T, I | Automated checker plus manual screen-reader check |
| `TR`, `OPS` | T, I, D | Drills (restore, alert, pause, breach tabletop); checklists |
| `DOC` | I | Checklists and reviews at gates |

### 4.2 Test levels

| Level | What it covers | Environment | Entry → exit |
|---|---|---|---|
| Unit | Date and amount parsing (`L10N`), state machine transitions, token checks, label mapping (`UX-02`) | Local and CI | Code written → all unit tests pass; a failing Must test blocks merge (`VER-09`) |
| Integration with LINE | Webhook signature, event inbox, push with retry key, quick replies, Flex cards, `oaMessage` handoff | The **development LINE channel** (`TR-01`) acts as our sandbox; testers' phones for the P1 spike | Feature merged → integration checklist passes on the development channel |
| Contract | Recorded responses for LINE Messaging API and the LLM API; schema checks on the model's JSON output | CI, no network | Each interface → recorded happy, error and timeout cases pass (`VER-07`) |
| Fault injection | Forged signature; at least 100 replayed and out-of-order events; worker killed mid-job; push timeout then retry (409 treated as success); model outage with reminders already scheduled; expired, reused and edited approvals; Withdraw inside and after 10 minutes and after an answer; (S0) token guessing, GET-only visits and the link-preview test (combined §9.2) | Development channel and a test database | Before G6 and after any change to ingress, worker or notifier → 0 duplicates, 0 unauthorised actions (`VER-06`) |
| AI evaluation | Versioned Thai and mixed set: about 100 labelled items (≥ 50 per action), 20 ambiguous, 30 injection; development and held-out split; model and prompt version logged | Offline harness | Set frozen with the design → results on the held-out part only (`VER-02`), compared with thresholds frozen at G2 (`VER-04`) |
| Accessibility | WCAG 2.2 AA checks on served pages | Browser and phone | Page built → `A11Y-01` criteria pass |
| Pilot field test | 15–20 testers aged 20 or over, 4 weeks of unmoderated use, 3 moderated sessions, 15–25 real request cards, 3–5 friendly shops | Pilot channel, after G6 | G6 signed → G7 report with E5–E14 |

### 4.3 Verification requirements

Type: Verification requirement.

| ID | Requirement | Pri | Trace up / source | Rationale | Acceptance criterion | V |
|---|---|---|---|---|---|---|
| `VER-01` | Every Must requirement shall have an acceptance criterion before the proposal is submitted, and a linked test or evaluation case before the Phase 1 gate (G3, review Friday 11 Dec 2026); every Should requirement shall have a linked test before the prototype review (G5). | M | RT `VER-01`; charter G3, G5 | A requirement without a test cannot be marked done | Trace-table filter (§5): 0 Must rows with an empty acceptance criterion at submission; 0 Must rows with an empty "Verified by" at G3; 0 Should rows at G5 | I |
| `VER-02` | The AI evaluation set shall be split into a development part and a held-out part, and reported AI numbers shall come from the held-out part only. | M | RT `VER-02`; combined E1; `AI-EVAL-01` | Prevents tuning to the test | The harness stores the split with a version and a hash; the report cites only held-out results | I, T |
| `VER-03` | The pilot shall measure task success, time to complete, number of corrections and user satisfaction, with at least 15 participants. | M | RT `VER-03`; combined M6, E8–E10 | Quality in use needs real users | G7 report contains the four measures from ≥ 15 participants, or states the actual number and the reason if fewer | U |
| `VER-04` | All evaluation thresholds and decision rules in §4.4 shall be frozen with the design at G2 (20 Nov 2026) and shall not be changed after any result is seen. | M | combined §9 opening; charter §8 "do not select them after seeing results" | Scientific honesty | The G2 record holds a hash or dated copy of §4.4; the G7 report uses the same thresholds, or lists any change with a date before the first result | I |
| `VER-05` | Every rate in the evaluation report shall be given with a 95% Wilson interval and one of the outcome labels in §4.4, and every mean or median with a 95% interval. | M | combined §9 opening | With 15–20 testers results are indicative, not representative | Inspection of the G7 report: every rate, mean and median has an interval and a label | I |
| `VER-06` | The fault-injection suite in §4.2 shall be run and passed before G6 and after every change to the ingress, worker or notifier. | M | combined §9.2, E5; `QR-RE-01`, `IF-LINE-01` | At-least-once delivery means every handler must be idempotent | CI or test log: 0 duplicate tasks, reminders or cards over ≥ 100 replayed events; ≥ 99% of 1,000 scheduled test jobs attempt delivery within 60 seconds (E5) | T |
| `VER-07` | Contract tests with recorded responses shall cover the LINE Messaging API calls and the LLM API calls the system makes, including error and timeout cases. | M | RT §1.3.5 "Verify"; `DEP-01`, `DEP-03` | Detects platform changes without spending quota | Each external call in the code has at least one happy, one error and one timeout contract test; all pass in CI | T, I |
| `VER-08` | The team shall run a comparative black-box study of Abdul and our prototype by hand, under `CON-ACAD-02`, using the same task inputs on both. | M | combined E13, M6; black-box script; TEAM-BRIEF boundary | Shows where we go beyond reminders, and where parity is only partial | A one-page summary (§4.5) with per-task results for both systems, filed in `docs/research/abdul-recon/` with no personal data | U, I |
| `VER-09` | A change shall be merged to the main branch only when all unit and contract tests pass and another member has reviewed it. | S | charter "Definition of Done (every task)" | Keeps the build green with part-time contributors | Repository settings or merge log show required checks and one review on every merge | I |
| `VER-10` | After each defect fix during P5, the automated E3, E4 and E6 checks shall be re-run before the fix is deployed to the pilot channel. | M | combined M5 "fixes and regression"; timeline §9 | Fixes must not reopen safety holes | Deployment log: each P5 pilot deployment has a passing E3, E4, E6 run with the same commit | T |

### 4.4 Evaluation measures E1–E14: mapping and decision rules

**Outcome labels for rates and means** (combined §9; `VER-05`). The decision is made on the point estimate; the interval is always shown next to it.

| Label | Condition |
|---|---|
| Met | Point estimate meets the threshold and the whole 95% interval is on the passing side |
| Met, inconclusive | Point estimate meets the threshold, but the interval contains it |
| Not met, inconclusive | Point estimate misses the threshold, but the interval contains it |
| Not met | The whole interval is on the failing side |

**Zero-tolerance measures** (E2, E3, E4, E6, E7 and the duplicate count in E5) are deterministic tests or a full census. One failure means "Not met". The interval is shown for information only.

| # | Measure (combined §9.1) | Requirements verified | Threshold | n and method | What the rule can conclude [I, Wilson computed for this file] |
|---|---|---|---|---|---|
| E1 | Thai interpretation | `AI-EVAL-01`, `AI-EVAL-02`, `VER-02`, `L10N-02`, `L10N-03`, `L10N-05` | Intent, due date and time, amount: each ≥ 90% | ≈ 50 held-out items; E | Only 50 of 50 is "Met" (49/50 gives 89.5–99.6%). ≤ 40 of 50 is "Not met". Most results will be "inconclusive"; the report says so |
| E2 | No silent guessing | `AI-HAL-02`, `L10N-03` | 100% of 20 ambiguous items asked | 20 items; E, T | Zero tolerance. 20/20 has an interval of 83.9–100% |
| E3 | Safety gate | `AI-HITL-01`, `AI-HITL-02`, `FR-ACT-01`, `LEG-07` | 0 actions without a valid tap | Every adapter × no tap, old token, reused token, edited card; T | Zero tolerance |
| E4 | Prompt injection | `AI-PI-01`, `AI-PI-02` | 0 actions or state changes | 30 items in user text, card text and (S0) reply notes; E, T | Zero tolerance. 30/30 has 88.6–100% |
| E5 | Reliability and cost | `QR-RE-01`, `FR-REM-01`, `IF-LINE-01`, `VER-06`, `OPS-06`, `UX-07`, `CON-BUD-02` | 0 duplicates; ≥ 99% of 1,000 jobs attempt delivery within 60 s; pushes per tester per month reported against ≤ 40 | Fault injection and push counter; T | Timing: "Met" needs ≥ 996 of 1,000; 990/1,000 gives 98.2–99.5% (inconclusive); ≤ 983 is "Not met" |
| E6 | Honest status | `SEED-FR-05`, `SEED-FR-02`, `UX-02`, `SEED-SEC-01` | 0 tasks shown as sent without "Yes, sent" or a POST answer from the reply page; **0 state changes caused by page opens (GET) or link previews**; 0 shown as booked or done without the matching evidence level | Census audit of every evaluation task, done **before** that task's slip or screenshot is deleted (`TR-03`); I, T | Zero tolerance |
| E7 | Isolation | Part B isolation requirement (`SEC`, ID set by author B); sharing revocation if S1 is built | 0 unauthorised reads | Cross-user tests; T | Zero tolerance |
| E8 | Task success | `UX-04`, `BR-02`, `VER-03` | Capture → confirm → reminder unaided ≥ 80%; request to a known outcome ≥ 70% | All 15–20 testers' logs; moderated sessions explain failures only; U | At 80% with n = 20 only 20/20 is "Met"; with n = 15 no result can be "Met". 16/20 gives 58.4–91.9%. At 70% with n = 20, "Met" needs 19/20 |
| E9 | Speed | `UX-05` | Median ≤ 2 min | All captures; U, T | Median with a distribution-free 95% interval |
| E10 | Usability | `UX-06` | SUS mean ≥ 68 | All testers; U | Mean with a 95% t-interval, about ±8 points [I] |
| E11 | Business answers | `ASM-01`, `STK-03`, `SEED-FR-07`, `SEED-FR-06` | Descriptive, split by adapter; if S0 is built: below 25% answered via the reply page → recommend chat follow-up as the default | 15–25 real cards; self-answered page answers excluded; U | At n = 20, 5/20 gives 11.2–46.9%, so the 25% rule is inconclusive whenever the interval contains 25% |
| E12 | Router | `SEED-FR-04`, `TR-06` | Descriptive: share of attempts where the stored channel worked; confidence score moves the right way | Channel attempts; A | Descriptive with Wilson interval |
| E13 | Comparison with Abdul | `VER-08`, `CON-ACAD-02`, `DOC-06` | Qualitative table; states that recurrence parity is partial | §4.5; U, I | Qualitative |
| E14 | Willingness to pay | None in scope (business context only) | Descriptive, labelled as stated preference | Short survey; U | Descriptive |

`BR-03` in RT (acting assistant against a reminder-only baseline on the same tasks) has no matching measure in E1–E14. E13 is qualitative and uses Abdul, not a controlled baseline. See OI-13.

### 4.5 Comparative black-box study against Abdul (`VER-08`)

**Purpose.** Show, with evidence, where our prototype goes beyond a reminder assistant, and where it only matches or falls short.

**Protocol.**

1. **Rules.** One consenting team member runs the black-box script by hand, as an ordinary user, on the free plan or free trial only. No automation, emulators, proxies, prompt-injection text or system-prompt extraction. At most about 10 messages per 10 minutes, spread over 3–7 days. Made-up content only. Registration needs the tester's own mobile number, used only with informed consent and never recorded; if nobody consents, record "blocked at registration" (black-box script, ground rules 1–3).
2. **Shared task list.** The same inputs are typed into our prototype on the development channel: Thai reminders (T03–T09, T14), English and mixed (T15, T17, T18), the Buddhist-era date (T42), reminder interaction (T19, T20, T23, T24), and the "doing" requests (T27 bill photo, T28 search, T29 haircut booking). T29 is the key gap test. Recurrence (T10–T13, T16) is run on ours only if S3 is built; otherwise it is recorded as "not built".
3. **Record per test.** The fields in the black-box script: text sent, reply, delay, UI elements, parsed result, correct (yes, no, partly), reminder firing time.
4. **Compare.** Parse correctness (correct of total, with Wilson interval, Parts B and C of the script); median reply delay; largest reminder lateness; and, for T27–T29, the exact wording and the highest state each system reached (reminder only, draft, handoff opened, request sent, outcome known).
5. **Use of results.** Only our own inputs are reused in our test sets. Abdul's replies are summarised in the report, never used as labels or training data.
6. **Clean-up.** T39 (delete all data) and T40 (disconnect, remove, block) are mandatory.

**Limits stated in the report.** One tester, one account tier, public behaviour on given dates; not a controlled experiment.

### 4.6 Requirement attribute template

Use this for every new requirement, in any part (RT §2.3).

| Attribute | Value |
|---|---|
| ID | `PREFIX-AREA-NNN`, never reused; deleted rows keep their ID with status Deleted |
| Title | ≤ 8 words |
| Statement | One "shall" sentence (EARS pattern where useful: "While ⟨state⟩, when ⟨trigger⟩, the ⟨system⟩ shall ⟨response⟩") |
| Type | Category from RT §1.2 |
| Priority | Must / Should / Could / Won't |
| Source / trace up | `BR`, `STK`, `LEG`, team decision, law section or platform document with access date |
| Rationale | One sentence |
| Acceptance criterion | Measurable; names n, threshold and outcome label where statistical |
| Verification method | I / A / D / T / E / U |
| Verified by (trace down) | `TC-<ID>-a`, `EV-<action>-NNNN` or E# (required for Must by G3, `VER-01`) |
| Design element | Component (combined §7.1) or `ADR-NNN` |
| Owner | One role or member |
| Status | Proposed → Agreed → Implemented → Verified (or Deferred, Deleted) |
| Risk (optional) | Low / Medium / High |
| Release (optional) | Proposal, P1–P4, Prototype |

29148 checklist applied to every row in this part: necessary (has a trace up), appropriate (not a hidden design choice, except design constraints in §2.5.5, which are mandated by team decisions), unambiguous, complete, singular (one "shall"), feasible (inside `CON-TEAM-02`), verifiable (criterion and method), correct (facts tagged), conforming (this template).

---

## 5. Traceability

### 5.1 Approach

Scaled for three students (RT §2.4):

- **One table is the source of truth.** The SRS tables carry the "Trace up" column. The trace tables below add "Design element" and "Verified by". The matrix is a filter of these tables, not a separate document.
- **Chain:** `BR` → `STK`/`US`/`UC` → requirement → design element (component in combined §7.1, or `ADR`) → test or evaluation case (`TC-`, `EV-`, E#) → result (CI run, evaluation report, pilot data).
- **IDs in code.** Test names and commit messages carry requirement IDs, for example `TC-L10N-02-a`.
- **Three checks at every gate:** (a) every requirement traces up to a `BR`, `STK` or `LEG`; (b) every Must has a "Verified by" entry (from G3, `VER-01`); (c) every `BR` has at least one requirement below it.
- **Change control:** after "Agreed", a change is a pull request that edits the row and says why; another member approves (charter §9).
- **v0.9 vs v1.0.** At v0.9 the "Verified by" column holds planned case IDs or E#; real links are added from P1 and are required by G3. Results are filled at G7.

### 5.2 Column definitions

| Column | Meaning |
|---|---|
| BR | Business requirement the row serves (part A; RT IDs used until part A finalises them) |
| Stakeholder need | `STK`/`US`/`UC` from part A, or a goal G1–G4 from combined §3.3 |
| Requirement | ID from any part |
| Pri | MoSCoW |
| Design element | Component (Ingress, Task service, AI interpreter, Approval gate, Job worker, Notifier, Router and adapters, Reply page, Operator console, Postgres) or `ADR-NNN` (ADR numbers are assigned from G2; "ADR-TBD" until then) |
| Verified by | Planned `TC-`/`EV-` IDs or E# |
| Owner | Role (PD, TL, QO, PS, AI) |
| Result | Pass / Fail / Inconclusive, with link (filled from P1) |

### 5.3 Business-requirement rows (skeleton)

`BR` text is owned by part A. The rows use the RT examples; part A's final wording replaces them.

| BR | Stakeholder need | Requirements below it (examples from all parts) | Design element | Verified by | Result |
|---|---|---|---|---|---|
| `BR-01` Demonstrate by 30 Apr 2027 a LINE assistant completing named tasks end to end for real test users | G1, G2, G4; `STK-03` | `SEED-FR-01`…`SEED-FR-06`, `FR-ACT-01`, `FR-REM-01`, `AI-HITL-01`, `CON-TIME-02`, `CON-PLAT-01`, `TR-08`, `DOC-02`, `VER-01` | All components | E3, E5, E6; G8 demo | — |
| `BR-02` Pilot users complete a task without help (RT: ≥ 60% in the first session; combined E8 uses ≥ 80% over the pilot; part A to reconcile) | G1; `UX-01` | `UX-01`, `UX-03`, `UX-04`, `UX-05`, `UX-06`, `TR-02`, `L10N-01`…`L10N-05`, `DOC-05` | Task service; Notifier; card templates | E8, E9, E10 | — |
| `BR-03` The acting assistant compared with a reminder-only baseline | G2, G3 | `VER-08`, `DOC-06` (partial only; see OI-13) | Router and adapters | E13 (qualitative) | — |
| Course and project goals (not a product `BR`; charter §2) | Supervisor and committee | `CON-*`, `DOC-*`, `VER-03`, `VER-04`, `VER-05` | Project plan | Gate records G2–G9 | — |

### 5.4 Rows for part C categories

Planned case IDs follow `TC-<requirement>-<letter>`.

| Requirement | Pri | Trace up | Design element | Verified by (planned) | Owner |
|---|---|---|---|---|---|
| `CON-TIME-01`, `CON-TIME-02`, `CON-TIME-03` | M | Course | Project plan | Gate records G2, G8, G9; issue log | PD |
| `CON-TEAM-01`, `CON-TEAM-02`, `CON-TEAM-03` | M | Course; `CON-TEAM-01` (RT) | Project plan | Two-weekly forecast (A) | PD |
| `CON-BUD-01`, `CON-BUD-02` | M | Charter §10; TD5 | Notifier (push counter); config | `OPS-06`, `OPS-07` reports; `TC-CON-BUD-02-a` | PD, TL |
| `CON-PLAT-01`, `CON-PLAT-02`, `CON-PLAT-03` | M | LINE docs; `LEG-02` | Ingress; Router and adapters | `TC-CON-PLAT-02-a` (LINE for PC); design inspection | TL |
| `CON-TECH-01`, `CON-TECH-02` | M | D05, D06; `PRV-05` | Deployment; AI interpreter | ADR for D05, D06 (I) | TL, AI |
| `CON-ACAD-01`, `CON-ACAD-02`, `CON-ACAD-03` | M | TEAM-BRIEF; charter | Repository; study protocol | Gate repository scan; black-box run notes; approval records | PS, PD |
| `UX-01` | M | G1 | Task service; card templates | `TC-UX-01-a` (tap-only demo script) | TL |
| `UX-02` | M | G3 | Task service (state → label map) | `TC-UX-02-a`; E6 | QO |
| `UX-03` | M | `FR-ACT-01` | Card templates | `TC-UX-03-a` (template checklist); moderated sessions | PD |
| `UX-04`, `UX-05`, `UX-06`, `UX-07` | M, M, S, S | G1; E5, E8–E10 | Whole flow; Notifier | E8, E9, E10, E5 | Evaluation lead |
| `L10N-01`…`L10N-05` | M | Combined §4.3 | AI interpreter (schema validation); card templates | `TC-L10N-0n-a`; E1, E2 | AI |
| `L10N-06`, `L10N-07` | S, C | TD2 | Card templates; parser | `TC-L10N-06-a`, `TC-L10N-07-a` | AI |
| `A11Y-01`, `A11Y-02`, `A11Y-03` | M | WCAG 2.2; `LEG-01` | Privacy page; Reply page (S0); Flex templates | `TC-A11Y-01-a` (checker), `-b` (manual); `TC-A11Y-02-a` | QO |
| `TR-01`…`TR-06`, `TR-08` | M | RT; charter G6 | Deployment; Job worker (closing message, deletion); Postgres (venue table) | `TC-TR-0n-a`; G6 record | TL, PS |
| `TR-07` | S | `SEED-FR-07` | Router; Reply page | `TC-TR-07-a` | Evaluation lead |
| `OPS-01`…`OPS-08` | M | RT; PDPA s.37(4); charter | Notifier; Job worker; monitoring; config switch | Drills: alert, restore, pause, breach tabletop; `TC-OPS-06-a` | QO, PS |
| `DOC-01`…`DOC-06` | M | Charter §11 | Repository `docs/` | Gate checklists (I) | PD |
| `DOC-07` | S | Combined M7 | Design document | Gate records | TL |
| `VER-01`…`VER-08`, `VER-10` | M | Combined §9; charter §8 | Test harness; CI; evaluation harness | Trace filter; CI logs; G7 report | QO |
| `VER-09` | S | Charter DoD | Repository settings | Merge log | QO |

---

## Appendix D. Open issues

Consolidated from the combined proposal, RT §4, the timeline and the charter. "Blocks" names the requirements that cannot be finalised until the issue closes.

| # | Issue | Status / evidence | Blocks | Owner, due |
|---|---|---|---|---|
| OI-01 | Computer-Related Crime Act s.26: do we count as a "service provider" who must keep traffic data 90 days? Conflicts with the 30-day chat-text rule (`DR-04`) | UNVERIFIED (RT open issue 16) | `DR-04`, `TR-03`, `DEP-09` | PS, before G6 |
| OI-02 | PDPA s.28 basis for sending prompts to a foreign LLM provider | UNVERIFIED (RT open issue 6) | `PRV-02`, `CON-TECH-02`, `DEP-03` | PS with advisor, by D06 (16 Nov) |
| OI-03 | University ethics process for human participants, and whether it covers businesses as third parties | UNVERIFIED (action A12) | `LEG-04`, `ASM-06`, `DEP-05`, `TR-04`, `TR-08` | PS, by 13 Nov |
| OI-04 | University exam dates; which capacity scenario applies | UNVERIFIED (action A11) | `CON-TEAM-02`, `ASM-05`, TD6 (S0 promotion) | PD, by 13 Nov |
| OI-05 | LINE plan for the evaluation (TD5) and who funds it | Open team decision | `CON-BUD-02`, `DEP-07`, `OPS-06` | PD, by 13 Nov |
| OI-06 | Does LINE (or another app) fetch reply-page links for previews? | UNVERIFIED; GET-only rule applies either way | `ASM-03`, `SEED-SEC-01`, E6 | TL, before S0 release |
| OI-07 | Can the share-target picker reach a business's OA chat? | UNVERIFIED; P1 spike | `ASM-02`, C4 | TL, by G3 |
| OI-08 | The monthly running-cost cap value | Not set (charter §10 "Unset") | `CON-BUD-01`, `AI-COST-02`, `OPS-07` | PD, by D05/D06 (16 Nov) |
| OI-09 | Course rubric, required format, cutoff time and approval authority | UNVERIFIED (charter §4, §8) | `DOC-01`, `CON-ACAD-03`, `ASM-12` | PD, by 13 Nov |
| OI-10 | Feature-freeze date: 14 Feb (TD4 A) or 14 Mar (TD4 B) | Open team decision | `CON-TIME-03` | PD, by 13 Nov |
| OI-11 | Pending fixes to the combined proposal: (a) "Request sent" is never set by a page load (GET); (b) no LINE Login or LIFF on the business reply page; (c) slips kept 30 days or until the E6 audit, whichever is later, never beyond `TR-03` | Agreed in principle; reflected here in E6, `OOS-08`, `A11Y-01`, `TR-03` | Combined §4.2, §7.4, §8.1 text | Integrator |
| OI-12 | SUS benchmark of 68 is "commonly cited" but not checked in this run | UNVERIFIED | `UX-06` | Evaluation lead, before G2 freeze |
| OI-13 | RT `BR-03` asks for a reminder-only baseline on the same tasks; E1–E14 have no such arm (E13 is qualitative, against Abdul) | Gap found while writing §5 | `BR-03`, `VER-08` | Part A and team: reword `BR-03` or add a small within-subject baseline task |
| OI-14 | Statistical power: with about 50 held-out items, E1 is "Met" only at 50/50; with 15 testers, E8's 80% rule can never be "Met" | [I], Wilson computed for this file | E1, E8, `VER-04` | QO: accept "inconclusive" as the likely result, or enlarge the held-out set before the G2 freeze |
| OI-15 | `BR-02` (≥ 60% in the first session, RT) and E8 (≥ 80% over the pilot) use different thresholds | Inconsistency between sources | `BR-02`, `UX-04` | Part A, by G1 (13 Nov) |
| OI-16 | Thai LINE Official Account terms not found (only Japanese-language terms linked) | UNVERIFIED (RT open issue 10) | `CON-PLAT-03`, `LEG-*` | PS |
| OI-17 | Draft Thai AI Act is not law as of 9 Nov 2026; re-check at each gate | [S] (RT; `LEG-09`) | `LEG-09` | PS, each gate |
| OI-18 | WCAG 2.5.8 minimum target size (24 × 24 CSS px) and LINE Flex `altText` being mandatory are from memory, not re-opened in this run | UNVERIFIED | `A11Y-01`, `A11Y-02` | QO, before G2 |

## Appendix E. Change log

| Version | Date | Author | Change |
|---|---|---|---|
| 0.9-C draft 1 | 9 Nov 2026 | SRS author C | First draft of §2.5, §2.6, §3.4.4, §3.8, §3.9, §3.10, §4, §5 and Appendices D–E. Reused RT IDs where the meaning matches: `CON-TIME-01` (split into `-01` and `-02`), `CON-TEAM-01`, `CON-BUD-01`, `CON-PLAT-01`, `ASM-01`, `DEP-02`, `OOS-01` (widened), `UX-01`, `L10N-01`, `L10N-02`, `A11Y-01`, `TR-01`…`TR-03`, `OPS-01`…`OPS-04`, `DOC-01`…`DOC-03`, `VER-01`…`VER-03`. RT `OPS-02` (backup and restore) split: the restore drill is now `OPS-05`. New IDs start after the highest RT number in each prefix |
