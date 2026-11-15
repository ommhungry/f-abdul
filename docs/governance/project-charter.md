# Project Governance and Charter

> **Revision notes (v0.3, 10 Nov 2026).** This version aligns omm's charter with the combined direction in [`../combined/combined-proposal.md`](../combined/combined-proposal.md) (v0.2) and [`../requirements/SRS-v0.9.md`](../requirements/SRS-v0.9.md). It starts from the 9 Nov review copy (`../review/revised/omm/project-charter.md`, v0.2), which already carries the validator's fixes, so those fixes are kept.
>
> **Nothing here is approved.** The combined proposal's recommended options for team decisions TD1–TD6 (due Fri 13 Nov) are used as the working baseline and are marked **[TDn, pending]**. If the team chooses a different option, change the marked text and the dependent figures together.
>
> What changed from v0.2:
>
> 1. **Product** (§1): from "obligations assistant with family sharing" to "a LINE assistant that acts, not only reminds": one engine for obligations and requests to a business.
> 2. **Users** (§1, TD1): from Bangkok working adults to university students and staff aged 20 or over (or married).
> 3. **Objectives** (§2): rewritten around combined goals G1–G4 and the evaluation measures E1–E14.
> 4. **Scope** (§3): the F01–F11 list is replaced by the combined MoSCoW list (M1–M7, S0–S4, C1–C6, Won't). A mapping from the old F-IDs is given so the kickoff and timeline stay traceable until they are updated. Family sharing moves from Required to Should (S1). Calendar, booking and voice are re-decided.
> 5. **Effort** (§7): Must scope is 111–157 h (midpoint 134 h), sized to scenario C at 3.5 h with a 7.75 h margin. The old 124–172 h figure is retired.
> 6. **Decisions** (§6): a register of TD1–TD6 is added.
> 7. **Gates and acceptance** (§8): evidence and criteria now use the combined proposal's measures; gate dates are unchanged. A TD4 option can move G5–G7.
> 8. **Risks, budget, deliverables, operation boundary** (§10–§11): updated to the combined proposal's risk table, the LINE plan decision (TD5), the SRS, and the "user taps, code decides" rule.
> 9. Roles, responsibility matrix, cadence, change control: kept, with wording changes where family sharing or OCR had been assumed.

**Project:** Thailand LINE AI Personal Assistant (working name to be chosen; "TumHai" is a placeholder)  
**Version:** 0.3 — 10 November 2026; draft aligned to the combined proposal v0.2  
**Status:** Proposed charter for team discussion and approval. Not approved  
**Timezone:** Asia/Bangkok  
**Proposal deadline:** 22 November 2026  
**Completion deadline:** 30 April 2027 — confirmed  
**Team capacity:** Three members, all familiar with app development, each contributing equally at 3–4 hours/week; 9–12 total team-hours/week. Public holidays and university exam periods are not yet subtracted; the combined proposal plans to the worst case (scenario C) at 3.5 h/week  
**Scope status:** Combined direction adopted as the working baseline; scope decision D03 / TD6 due 13 November

Related files: [Kickoff meeting](../meetings/2026-11-07-kickoff.md), [timeline and development phases](../planning/timeline-and-development-phases.md) (v0.3, aligned to this charter), [combined proposal](../combined/combined-proposal.md), [advantages matrix](../combined/advantages-matrix.md), [SRS v0.9](../requirements/SRS-v0.9.md), [requirements register](../requirements/requirements-register.csv), [research brief](../research/business-and-architecture-brief.md).

## 1. Purpose and project authorization

Create a LINE-based assistant for Thailand that **turns a message into a task with an outcome, prepares the next step so that one tap from the user sends it, and keeps following up until the outcome is backed by evidence**. The baseline competitor, Abdul (abdul.chat), reminds; this project acts. The assistant handles two task types on one lifecycle:

- an **obligation** (a bill, rent, a renewal, an appointment), and
- a **request to a business** (a haircut, a table, a quote).

It works at three levels: Level 1 does the task itself; Level 2 prepares a request the user sends with one tap and then tracks the answer; Level 3 is direct booking with businesses that opt in. **Only Levels 1 and 2 are built.** The governing rule is that the AI interprets and code decides: nothing leaves the system without the user's confirmed tap.

**Prototype users [TD1, pending].** Students and staff of the team's university who are aged 20 or over, or married, and who manage some of their own bills, rent, renewals or appointments. This replaces the earlier Bangkok working-adult segment. The final report must state that the sample does not represent all working adults.

The project is new. Team size, development familiarity, weekly hours, and the final deadline are confirmed. Member names, role assignments, and several design alternatives remain open. This charter provides a structure for those decisions; it does not claim that people have accepted roles or that the proposed scope is approved.

The proposal submitted on **22 November 2026** must contain the finalized architecture and overall design. Product implementation does not have to be finished at that point. The recommended delivery objective for the confirmed capacity is a **small evaluated prototype and handover by 30 April 2027**, tested with 15–20 testers. A broader pilot, a business model, or a commercial launch would require separately funded expansion. Decision D01 (completion definition) was due 9 November. Record the decision, or the slip and a new date, in the decision log; do not leave it open.

**Business material is context, not a deliverable.** The research brief and members' business ideas (user-pays pricing, venue-database moat, merchant tiers) remain useful motivation. No money is collected in the prototype.

## 2. Objectives and success

Goals G1–G4 come from the combined proposal §3.3. Targets are the evaluation measures in §9.1 of that proposal, to be frozen by 20 November.

| Objective | Evidence of success |
|---|---|
| **G1** A message becomes a confirmed, tracked task quickly | Median ≤ 2 minutes from first message to confirmed task (E9); at least 80% of testers complete capture → confirm → reminder unaided (E8) |
| **G2** A request to a business is prepared for one-tap send and followed up to a known outcome | At least 70% of request tasks reach a known outcome: answered, declined, or clearly no reply (E8); share answered, declined, no answer, and time to answer reported (E11) |
| **G3** Status is honest | 0 tasks shown as sent, booked or done without the matching evidence (E6); a handoff is never "sent" just because a page was opened |
| **G4** No action leaves the system without the user's confirmed tap | 0 actions without a valid tap across no-tap, old-token, reused-token and edited-card tests (E3) |
| Interpret Thai reliably and refuse to guess | Intent, date/time and amount accuracy ≥ 90% on a held-out set (E1); all 20 ambiguous items asked, none filled silently (E2); 0 effects from 30 prompt-injection items (E4) |
| Deliver a controlled, maintainable prototype | Cross-user isolation (E7), reliability and cost (E5), tested recovery, handover documentation |
| Complete the proposal on time | Approved design package plus submission receipt by the actual cutoff |
| Complete the project on time | Accepted prototype, evaluation report, known limitations, and a named maintenance/closure owner |

With 15–20 testers, every rate is reported with a 95% Wilson confidence interval and treated as indicative, not representative. A decision rule is **inconclusive** when its interval contains the threshold.

Business hypotheses — willingness to pay, retention, margin — are recorded as hypotheses. The evaluation can show only stated preference (E14, a short survey) and a descriptive repeat-use figure (E11). A weak result is a reason to revise the idea, not to conceal results or relabel incomplete software as complete.

## 3. Baseline scope proposal

Scope follows the combined proposal §6.1. The earlier feature IDs F01–F11 are kept only as a mapping (below). **Do not confuse them with the SRS product functions F1–F14**, which are numbered separately (SRS §2.2).

### Must — the April deliverable [TD6, pending]

| ID | Item | Contents |
|---|---|---|
| M1 | Foundations | LINE webhook with signature check and event inbox; database schema; job worker; deployment; separate development and pilot channels; alerts; logs without personal content; secrets outside the repository; early spike on LINE's `oaMessage` handoff and share picker on testers' phones |
| M2 | Obligations and reminders | Text capture → confirmation card with Confirm, Edit and Cancel; Thai and Buddhist-era dates; **one-off** reminders with snooze, done and quiet hours; completion with an evidence level |
| M3 | Bounded AI and Thai test set | Classifier and schema extraction; structured fallback; about 100 labelled Thai and mixed items, 20 ambiguous, 30 injection items, with a held-out split |
| M4 | Request to a business | Card composer with Confirm, Edit and Cancel; approval bound to the content; LINE handoff and copy-and-send; "Did you send it?"; follow-up, expiry and 10-minute Withdraw; outcome returned plus a reminder for the agreed time; router and venue table of 10–15 hand-checked venues |
| M5 | Assurance | Isolation tests; fault injection; approval and Withdraw tests; privacy notice, retention jobs, export and delete; backup restore drill |
| M6 | Evaluation | Ethics and consent pack covering testers and businesses; 15–20 testers and 3–5 friendly shops; 3 moderated sessions; four weeks of unmoderated use with 15–25 real request cards; analysis; Abdul comparison |
| M7 | Documentation | Design docs kept current, final report, demo, handover |

Must effort is **111–157 h, midpoint 134 h** (estimate; line items in the combined proposal §6.2).

### Should — started only when the two-weekly forecast shows the hours, in this order [TD3, pending]

- **S0** Reply page for businesses (one-tap answer on a web page with no sign-up). **Promoted to Must automatically** if the calendar recorded at action A11 shows scenario B or better at 3.5 h (Must + S0 = 144.5 h) [TD6].
- **S3** Recurrence: monthly first, then weekly and daily.
- **S4** "Upcoming in the next N days" list as a chat message.
- **S1** Share one item with one invited person: read status, mark done, revoke.
- **S2** Operator console: a team member handles a phone-only shop in a published time window; every human step is logged and timed.

### Could

C1 photo of one bill format read by a vision model; C2 bill split with a PromptPay QR to the payer's own account; C3 `.ics` calendar file for a confirmed booking; C4 share-target-picker adapter (only if the spike shows it works); C5 LIFF pages; C6 a second language model compared on the held-out set.

### Won't — this project

Level 3 direct booking; partner APIs; browser agents for web forms; email; AI phone calls; bots inside LINE groups; Google Calendar or Gmail OAuth; payments or subscriptions; venue recommendations built from Google or Wongnai data; escalation-until-acknowledged reminders; voice-note input; วันพระ recurrence; medical requests; testers under 20; Temporal; a merchant dashboard.

### Mapping from the earlier feature IDs

| Earlier ID (kickoff, timeline) | Now |
|---|---|
| F01 LINE onboarding, identity, private account | M1 (with isolation tests in M5) |
| F02 Text capture with bounded AI | M2 and M3. Image/OCR extension → **C1 (Could)** |
| F03 Validated dates, recurring reminders, snooze, completion | M2 for one-off reminders. **Recurrence → S3 (Should)** |
| F04 Upcoming obligations; user-reported vs verified status | Evidence levels in M2; the "upcoming" list → S4 (Should) |
| F05 Selective family sharing | **S1 (Should)**, reduced to one item and one invited person |
| F06 Authorization, audit, privacy, recovery, cost monitoring | M5, with alerts in M1 |
| F07 Calendar connection (conditional) | **Won't** for Google Calendar; `.ics` file → C3 |
| F08 One booking category (conditional) | **Replaced by M4**, requests to a business; first category hair and beauty shops [TD2, pending]. Level 3 direct booking is Won't |
| F09 Tax-document checklist (conditional) | **Not carried forward.** The combined proposal and SRS do not mention it. Confirm it is dropped (see D10 below) |
| F10 Voice input, inbox ingestion, telephone booking (deferred) | Won't (voice notes, Gmail, AI phone calls: future work) |
| F11 Autonomous banking/tax filing, group reading, universal booking (excluded) | Still excluded |

### Scope reduction order

When resources are insufficient, apply the combined proposal §6.5 in this order, never weakening authorization, privacy, retention or evidence rules:

1. Start no Should or Could item.
2. Venue table drops to 10 venues; request cards to about 15 (the minimum of 15 testers stays).
3. Moderated sessions drop from 3 to 2, plus a written remote task script.
4. The copy fallback shows text only (no phone path).
5. If more is needed, the sponsor revises scope explicitly through change control. Nobody assumes unpaid overtime.

Decision D10 (18 November) must confirm this order, the order of Should items (TD3), and whether F09 is dropped.

## 4. Governance structure and role descriptions

Roles describe accountability, not additional headcount. Allocate them across the **three confirmed members** now; the roster below is still unassigned. One person holds several roles; each decision or deliverable still has one accountable owner. Time spent in review, meetings, and documentation is part of each person's 3–4 hours.

| Code | Role | Responsibilities | Decision authority | Required outputs |
|---|---|---|---|---|
| SP | Sponsor / project owner | Own purpose, budget, scope priorities, deadline, and final acceptance | Approves scope baseline, material changes, spending, and project acceptance | Charter approval, scope decisions, acceptance record |
| PD | Product and delivery lead | Requirements, backlog, user research and **evaluation recruitment**, schedule, dependencies, meeting records, submission coordination | Prioritizes within approved scope; escalates material changes | Requirements, sprint goals, status reports, proposal package, two-weekly hours forecast |
| TL | Technical lead / backend lead | Architecture, data model, job execution, LINE interfaces and handoff adapters, engineering standards | Approves technical design within agreed constraints | Architecture decisions, backend delivery, integration contracts |
| AI | AI and Thai-language lead | Extraction, Thai evaluation set, model integration, injection tests, cost control | Selects implementation within approved provider/data policy | Evaluation set, extraction pipeline, model reports |
| UX | UX and LINE/frontend lead | User flows, confirmation and request cards, Thai copy, consent screens, usability | Owns interface decisions within approved journeys and privacy constraints | Wireframes, flow specifications, interface implementation |
| QO | Quality and operations lead | Test strategy, fault injection, deployment, monitoring, backup/restore, incident drills | Accepts verification evidence; can block unsafe release | Test reports, release checklist, runbooks, recovery evidence |
| PS | Privacy and security owner | Threat model, data inventory, retention/deletion, ethics and consent application, reply-page security, external legal coordination | Can block release for unresolved material privacy/security issues | Privacy/security review, ethics pack, risk treatment, data-flow assessment |
| AD | Faculty advisor / course committee (external to the team) | Approves the proposal and final acceptance under the course's rules; reviews progress | As set by the course (UNVERIFIED; obtain the rules) | Proposal approval, progress feedback, final grade or acceptance |

SP is the team's own student owner of scope and priorities. Where the course gives approval or acceptance authority to the advisor or committee (AD), AD's decision prevails and SP prepares it. External legal or security advisers may be consulted where the team lacks competence. PS is an internal accountability role and does not imply a legal qualification or an automatic statutory Data Protection Officer appointment.

**Evaluation lead and operator.** The combined proposal and the SRS name an *evaluation lead* and, if S2 is built, an *operator*. This charter treats the evaluation lead as part of PD (recruitment and feedback) with ethics paperwork under PS; the operator is any team member rostered for S2. If the team wants a separate evaluation-lead seat, add a row here and in §5.

### Recommended three-person role mapping

Names and specialist strengths are unknown. Use this as the starting assignment proposal, then rebalance:

| Planning seat | Primary responsibility | Additional responsibility | Key caution |
|---|---|---|---|
| Member 1 | Product, coordination, and LINE/interface delivery — PD/UX | Tester and shop recruitment, sessions and proposal assembly; SP only if explicitly assigned | Cap coordination; other members own their documentation and tests |
| Member 2 | Architecture, backend, job system, handoff adapters, deployment — TL | Recovery implementation; reviews Member 3's AI and test changes | Member 3 reviews approval-gate and permission-sensitive backend changes |
| Member 3 | Bounded AI, evaluation set, and quality/privacy coordination — AI/QO/PS | Approval-gate, isolation and injection test design; ethics pack; evaluation reports | Testing and security implementation remain shared responsibilities |

Each member contributes **3–4 hours per week**, not 3–4 hours per role. Review pairs: Member 1's interface work → Member 2; Member 2's backend work → Member 3; Member 3's AI/evaluation work → Member 2, with Member 1 validating user-facing outcomes. Rebalance roles if the quality/privacy workload exceeds Member 3's allocation.

Two titles held by one person do not create independent review. At least one different capable person reviews approval-gate, authorization, credential, reply-page and release-sensitive changes. If the team cannot competently review a material risk, use qualified advice or keep that capability out of the prototype.

### Named responsibility roster

| Role | Primary person | Backup / reviewer | Hours/week attributable to this role | Accepted on |
|---|---|---|---:|---|
| SP | Unassigned | Unassigned | ___ | ___ |
| PD | Unassigned | Unassigned | ___ | ___ |
| TL | Unassigned | Unassigned | ___ | ___ |
| AI | Unassigned | Unassigned | ___ | ___ |
| UX | Unassigned | Unassigned | ___ | ___ |
| QO | Unassigned | Unassigned | ___ | ___ |
| PS | Unassigned | Unassigned | ___ | ___ |
| AD (external) | Advisor name: ___ | — | not counted in team hours | ___ |

The sum of a person's role allocations must not exceed that person's available hours. Identify a named proposal submitter and backup separately, even if they also hold PD/SP roles.

## 5. Responsibility matrix

Fill only the rows that apply; with three people, an owner and reviewer per deliverable carries the same information. Where AD approves an item (proposal, final acceptance), SP prepares it and remains accountable inside the team.

**R:** performs the work. **A:** accountable for the result. **C:** consulted. **I:** informed. **A/R:** both accountable and performing. Each row has exactly one accountable role. Specialist sign-offs listed later remain required checks and do not add a second accountable owner to the row.

| Deliverable / activity | SP | PD | TL | AI | UX | QO | PS |
|---|---|---|---|---|---|---|---|
| Charter, scope, deadline, and budget | A | R | C | I | C | C | C |
| Research and requirements (SRS) | C | A/R | C | C | R | C | C |
| Architecture and technical decisions | I | C | A/R | R | C | C | C |
| Data inventory and privacy/security design | I | C | R | C | C | R | A/R |
| User experience: confirmation and request cards, Thai copy | I | C | C | C | A/R | C | C |
| Backend, job system, approval gate, and permissions | I | I | A/R | C | C | R | C |
| AI extraction and Thai evaluation set | I | C | C | A/R | C | R | C |
| LINE handoff adapters, venue table, reply page (if S0) | I | C | A | R | C | R | C |
| Test strategy and acceptance evidence | I | C | R | R | R | A/R | C |
| Deployment, monitoring, and recovery | I | I | R | C | C | A/R | C |
| Proposal assembly and submission | A | R | R | C | C | C | C |
| Ethics and consent application (testers and businesses) | I | R | I | I | C | I | A |
| Evaluation recruitment and feedback | I | A/R | C | C | R | R | C |
| Material change approval | A | R | C | C | C | C | C |
| Release and final acceptance | A | R | R | C | C | R | C |

## 6. Decision rights and escalation

- The accountable owner records a decision after consulting the relevant contributors.
- Product priorities within the approved scope are managed by PD. Architecture changes are managed by TL.
- SP decides material scope, budget, deadline, or completion-definition changes.
- QO and PS may block a release when required quality or safety evidence is absent. SP may reduce scope or delay release but cannot simply label failed mandatory criteria as passed.
- Any team member may raise a privacy/security concern or stop their own unsafe action immediately.
- Raise a blocked task on the same working day. Escalate unresolved cross-role dependencies to PD/TL by the next working day; material trade-offs go to SP.
- Before the proposal freeze, resolve conflicting choices through a short evidence review and an architecture/decision record. After freeze, use change control.

The proposal owner tracks decisions D01–D10 from the meeting file. Missing critical decisions on 18 November require an explicit supported fallback or removal from scope before approval on 20 November.

### Team decisions TD1–TD6, due Friday 13 November 2026

From the combined proposal §13. "Recommended" is that document's recommendation, not a team decision. Record the outcome and date in the last column.

| # | Decision | Recommended option | Related D-item | Decided (option / date) |
|---|---|---|---|---|
| TD1 | Who the prototype is for | A: university students and staff aged 20 or over (or married). Fallback C: staff and graduate students only | D03 | Open |
| TD2 | First category for requests to businesses | A: hair and beauty shops near campus, 10–15 hand-checked venues; B (any business with a LINE ID) allowed in the field test | D03, D07 | Open |
| TD3 | Order of Should and Could items | A: S0 → S3 → S4 → S1 → S2 → Could | D10 | Open |
| TD4 | Evaluation window and Songkran | B: freeze 14 Mar, evaluate 15 Mar–11 Apr (four weeks), 1–14 Mar for overflow, stabilization before Songkran | D10 | Open |
| TD5 | LINE plan for the evaluation | A: Basic plan for the evaluation months (฿1,280 + 7% VAT a month; about ฿2,740 for two months, estimate) if funded; B: Free plan with about 15 pushes per tester a month as fallback | D10 | Open |
| TD6 | Adopt the scope baseline and handle the proposal-window overrun | A: adopt Must 111–157 h (midpoint 134 h); S0 promoted to Must if the real calendar is scenario B or better; take the S3 sub-option if in scenario C; cut the proposal package to fit 13.5–18 h | D03 | Open |

Decisions that stay on omm's later schedule: stack and deployment (D05) and AI provider (D06) on 16 Nov; privacy and threat model (D08) on 18 Nov; the working name.

## 7. Delivery method and working cadence

Use an iterative SDLC with formal gates: requirements and design are finalized for submission, implementation proceeds in small increments, and evaluation access depends on evidence. Testing, security, privacy, and documentation run throughout development.

**Effort and capacity.** Must scope is 111–157 h (midpoint 134 h). Usable delivery hours from 23 Nov to 25 Apr after the 25% coordination allowance are 141.75 h in scenario C (holidays removed and two exam fortnights at half capacity) at 3.5 h per person per week. That leaves a **7.75 h margin at the midpoint**, and no margin at the high estimate. At 3.0 h in scenario C only the low estimate fits. These are estimates; the combined proposal §6.3 shows all scenarios. The older 124–172 h figure in the timeline is retired.

**Forecast rule.** PD publishes an hours forecast every two weeks. Should items start only when the forecast shows the hours, in the TD3 order. Actual hours per line item are compared with the estimate at the end of P1; if they run over, apply the cut order in §3 straight away.

| Activity | Suggested cadence | Owner | Output |
|---|---|---|---|
| Proposal coordination, 9–20 Nov | Two brief asynchronous updates per week plus targeted decision comments | PD | Decisions, blockers, next actions |
| Development coordination | One 15-minute team check-in per week | PD/TL | Progress and dependency changes |
| Iteration planning and demonstration | Every two weeks; replace that week's check-in with a 30-minute session | PD/TL | Goal, selected work, demonstrated increment, **hours forecast** |
| Risk/capacity review | Five minutes inside the weekly check-in; full review every two weeks | PD | Updated risk register and forecast |
| Architecture/security review | At relevant changes and phase gates | TL/PS | Decision or findings record |
| Evaluation review | Weekly during evaluation, within the existing check-in | PD/QO | Usage, incidents, push counts, feedback, fixes |
| Retrospective | One asynchronous observation per member per iteration | Whole team | One concrete process change |

These are proposed frequencies; agree actual times around availability. A 15-minute meeting consumes 0.75 team-hours. A 30-minute meeting consumes 1.5 team-hours. Over 22 delivery weeks this cadence costs 11 × (0.75 + 1.5) = 24.75 team-hours, about 43% of the 25% allowance at the midpoint. Avoid adding ceremonies that consume the limited development budget.

### Work tracking rules

Each issue records a requirement ID (from the SRS register), owner, acceptance criteria, estimate, dependencies, evidence link, and status. Suggested statuses: Backlog → Ready → In progress → Review (including verification) → Done, with Blocked recorded as a label.

Use GitHub issues/board and pull requests in the existing repository (`ommhungry/f-abdul`). Store approved documents and decision records in the repository. Do not create fictional issue links.

Limit active work to what each person can finish and review. A task is not Done merely because code has been written or a demo recorded.

## 8. Quality gates and acceptance

Gate dates are unchanged from v0.2 and match the SRS. TD4 option B would move the freeze and the evaluation window (see the note under the table).

| Gate | Target | Review by (working day) | Required evidence | Accountable owner |
|---|---|---|---|---|
| G0 — project setup decisions | 9 Nov 2026 (Mon) | 9 Nov | Named roster, actual capacity, completion definition, submission instructions | SP |
| G1 — requirements baseline | 13 Nov (Fri) | 13 Nov | TD1–TD6 decided; SRS v0.9 reviewed by the team; Must/Should/Could list accepted | PD |
| G2 — design/proposal approval | 20 Nov (Fri) | 20 Nov | Final architecture, data and approval-gate model, flows, threat model, evaluation thresholds E1–E14 frozen, test plan, approved scope, estimate and timeline; advisor sign-off if the course requires it | SP (AD approves) |
| G3 — foundations accepted | 13 Dec (Sun) | Fri 11 Dec | M1 done: signature-checked webhook, repeatable deployment, persisted records, alerts, handoff spike result; demo reviewed by QO or another member | TL |
| G4 — core obligation flow accepted | 24 Jan 2027 (Sun) | Fri 22 Jan | M2 and M3 demonstrated end to end; Thai test-set run against the thresholds recorded in advance; demo reviewed by QO or another member | TL |
| G5 — feature complete | 14 Feb (Sun) | Fri 12 Feb | M4 request flow with handoff, follow-up, Withdraw and honest states integrated; any promoted Should item integrated; no new feature work planned; demo reviewed by QO or another member | TL |
| G6 — controlled evaluation readiness | 28 Feb (Sun) | Fri 26 Feb | M5 evidence (isolation, fault injection, approval tests, restore drill), privacy notice and consent screens, ethics approval and consent (testers and businesses), 15–20 testers and 3–5 shops recruited, LINE plan decided, support boundaries; advisor informed | SP |
| G7 — evaluation completed | 11 Apr (Sun) | Fri 9 Apr | Observation window complete, defect status, push counts and costs, E1–E14 results with confidence intervals, evidence limitations | PD |
| G8 — acceptance and handover | 25 Apr (Sun) | Fri 23 Apr | Accepted prototype, documentation, access transfer, maintenance owner, known-issues record; advisor/committee acceptance per course rules | SP (AD accepts) |
| G9 — closure deadline | 30 Apr (Fri) | 30 Apr | Remaining closure items resolved, SRS v1.0 baselined, final evidence archived | SP |

**If TD4 option B is chosen** (recommended), the 1–14 March overflow period moves G6 to 14 Mar (Sun, review Fri 12 Mar), and the evaluation window is 15 Mar–11 Apr (four weeks) instead of six. The timeline v0.3 (§5) already shows P5 as 1–14 Mar overflow plus the 15 Mar–11 Apr evaluation; the combined proposal's phase table (§6.4) still shows P5 as a single block starting 1 March. If the team picks another TD4 option, change this table and the timeline together.

Calendar windows describe target completion; they do not require weekend work. G3–G8 fall on Sundays because phases run Monday to Sunday; hold the review on the Friday shown. Add the course's own milestones (progress report, defence, final submission) here once known.

### Lightweight engineering artefacts

| Artefact | Contents | Owner | First due |
|---|---|---|---|
| Requirements specification | [SRS v0.9](../requirements/SRS-v0.9.md) and [register](../requirements/requirements-register.csv): 16 requirement categories, one row per requirement with acceptance criterion and verification method | PD | v0.9 with the proposal on 22 Nov; v1.0 on 30 Apr |
| Traceability table | Register columns trace each requirement up to its source and down to design and tests (`traces_down` is empty at v0.9) | QO | Skeleton at G2; filled from P1 |
| Test plan | Technical tests (combined proposal §9.2), evaluation measures E1–E14 (§9.1), Thai test-set size and policy | QO | G2, 20 Nov |

### Acceptance criteria — thresholds to freeze by 20 November

Targets below are from the combined proposal §9.1. They still need a feasibility review by the team and are frozen with the design; do not change them after seeing results.

| Area | Criterion | Evidence |
|---|---|---|
| Core journeys | At least 80% of testers complete capture → confirm → reminder unaided; at least 70% take a request to a known outcome; median ≤ 2 minutes to a confirmed task (E8, E9) | Evaluation logs; 3 moderated sessions explain failures but are not scored |
| Approval gate | 0 actions without a valid tap; 0 state changes from page opens or link previews (E3, E6) | Adversarial tests with no tap, old, reused and edited tokens |
| Isolation | 0 unauthorized reads across users (and across revoked shares if S1 is built) (E7) | Automated and manual isolation tests |
| Thai interpretation | Intent, date/time and amount ≥ 90% on the held-out set; 100% of ambiguous items asked (E1, E2) | Versioned dataset, scoring rules, confidence intervals, error analysis |
| Prompt injection | 0 effects from 30 injection items in user text, card text and reply notes (E4) | Test results |
| Reminder execution | 0 duplicate reminders or cards; at least 99% of 1,000 scheduled test jobs attempt delivery within 60 seconds under the agreed test load (E5) | Scheduler measurements; provider delivery and user attention measured separately |
| Recovery | Proposed prototype target: restore within 4 staffed working hours from a daily backup, with up to 24 hours' data loss in the declared disaster scenario | Measured restore exercise; clearly disclosed prototype limitation |
| Security/privacy | No unresolved critical/high findings under the agreed severity rubric; export and delete within 30 days, retention jobs, and audit behavior verified | Review report and regression evidence |
| Usability | Mean System Usability Scale score ≥ 68 (a commonly cited average, UNVERIFIED here) (E10) | Questionnaire and fixes |
| Operations | Deployment, rollback, monitoring, incident, push-count and cost procedures demonstrated | Runbooks and exercise results |
| Reported, not pass/fail | Business answers and repeat use (E11); router accuracy (E12); Abdul comparison from public information only (E13); willingness to pay as stated preference (E14) | Descriptive tables with intervals |

With 15–20 testers, rates are reported with 95% Wilson intervals. A rule is called inconclusive when its interval contains the threshold.

### Definition of Ready and Done

**Ready:** owner and reviewer named; requirement ID and acceptance criteria written; dependencies known. For permission- or data-sensitive tasks, also note the permission/data impact and test approach.

**Done (every task):** reviewed by another member, relevant tests passed, requirement ID linked, and short notes updated.

**Done (permission-, data-, approval-gate-, or release-sensitive task):** all of the above, plus security/privacy implications addressed, evidence attached, and the accountable owner accepts the deliverable. Real-user deployment also requires the applicable release gate.

## 9. Change control

After 20 November, an approved design baseline remains the source of truth. Small changes that do not alter approved behavior, scope, dates, or data handling need only one line in the decision log and the normal review. Material changes follow a recorded process:

1. Submit a change request with reason, alternatives, affected requirements/design, effort, timeline, cost, security/privacy impact, and rollback/fallback.
2. TL and relevant specialists assess it; PD updates the delivery forecast.
3. SP approves material scope/budget/deadline changes. TL may approve internal implementation changes that preserve approved behavior and constraints.
4. Update the decision record, requirements register, tests, timeline, and proposal addendum if required by the recipient.

**Pre-approved changes.** Starting a Should item when the forecast shows the hours, and the automatic promotion of S0 to Must under TD6, do not need a new change request. Record them in the decision log with the forecast that triggered them. Any other move of a Should or Could item into Must is a material change.

Urgent incident containment may precede normal review. Record the action immediately and complete retrospective review on the next working day. Containment authority does not authorize unrelated feature changes.

## 10. Risks, resources, and dependencies

Likelihood/impact are from the combined proposal §11 where it rates the risk; the team should rate the others.

| Risk / dependency | Accountable role | Likelihood / impact | Trigger | Response |
|---|---|---|---|---|
| Actual team capacity below estimate; student exams cut hours | PD | High / high | Forecast cannot support the Must list | Plan is sized to scenario C at 3.5 h; cut order in §3; forecast every two weeks; record real exam dates by 13 Nov |
| Estimates still too low | PD | Medium / high | Actual hours per line item exceed the estimate at the end of P1 | Compare actuals at end of P1; Should items wait for the forecast; apply the cut order |
| Scope exceeds 9–12 team-hours/week | SP | ___ | Forecast exceeds remaining usable hours | Remove optional scope, narrow baseline deliberately, or add agreed capacity |
| Proposal requirements unknown | PD | ___ | No verified instructions (recipient, format, rubric, cutoff) | Escalate immediately; obtain recipient requirements |
| Proposal-window overrun | PD | ___ | 17–25 h of proposal work against 13.5–18 usable team-hours from 9 Nov | TD6: cut the proposal package to fit; do not trade away G2 evidence |
| Age rule shrinks the tester pool | Evaluation lead (PD) | Medium / medium | Fewer than 15 eligible testers by P4 | Recruit staff, later-year and graduate students; start recruiting in P3–P4; fall back to option C of TD1 |
| Ethics approval late | PS | Medium / high | Application not filed in P2, or not approved before G6 | File in P2 covering businesses as third-party data subjects; synthetic sessions until approved; pause recruitment |
| Businesses ignore request cards | PD | Medium-high / medium | Few cards answered in the field test | Recruit 3–5 friendly shops in advance; chat follow-up path; report E11 honestly either way |
| `oaMessage` needs the shop's LINE ID and works on mobile only; share picker may not reach OA chats | TL | Certain / medium | P1 spike shows the handoff fails on testers' phones | Venue table stores checked IDs; copy-and-send fallback; picker adapter is Could |
| LINE push quota exceeded during evaluation | TL | High on the Free plan / medium | Push counter near 300 a month on the Free plan | TD5: Basic plan if funded, or daily digest and a per-tester cap; push counter per tester |
| Thai extraction below target | AI | Medium / medium | Held-out accuracy under 90% at G4 | Schema plus confidence plus Edit; structured fallback; narrower input forms |
| Prompt injection through business or user text | AI | Medium / medium | Any injection item changes state | Text is data; the model has no acting tools; code enforces the approval gate |
| Reply page abused or triggered by link previews (S0) | PS | Low / medium | Link-preview test changes state | Reply-page security rules; GET never changes state; link-preview test |
| Songkran inside P6 | PD | Certain / medium | Stabilization planned 12–18 Apr | TD4: stabilization in late P5, before Songkran |
| University exam periods and public holidays | PD | ___ | Exam dates not recorded by 13 Nov; holidays Mon 7 Dec and Thu 10 Dec (P1), New Year (P2), Makha Bucha substitute 22 Feb (P4), Songkran 13–15 Apr (P6) | Record dates; rebaseline phases; protect P1–P4 |
| Member unavailable or leaves the course | SP | ___ | Any member below 3 h/week for two weeks | Redistribute ownership; cut conditionals; inform advisor |
| Single-person dependency | TL | ___ | No reviewer or backup for a critical subsystem | Cross-train, document, redistribute ownership |
| Privacy/security expertise gap | PS | ___ | Team cannot assess a material risk | Seek qualified advice or reduce data/action scope |
| Model/provider cost overrun and provider terms | AI | ___ | Measured usage exceeds approved budget, or provider terms conflict with the data policy | Usage limits, model routing, narrower scope; change provider or restrict data sent |
| Evaluation cannot start safely on the planned date | QO | ___ | G6 not met by 28 Feb (14 Mar if TD4 option B) | Delay external access; use synthetic-data evaluation while fixing issues and disclose reduced user evidence |
| Scope churn from new ideas | SP | ___ | New directions added after TD6 is decided | Record one direction; park the rest in the combined proposal §12 (future work) |
| LINE or another platform ships its own agent | SP | Medium / low for the project | Platform announces a comparable agent API | Replaceable adapters; core stays portable |

### Budget categories to assign

Fill only the categories that apply; a student prototype usually needs one small cloud and model budget.

| Category | Owner | Approved cap | Actual / forecast |
|---|---|---:|---:|
| Hosting, storage, backups, and monitoring | QO | Unset | Unset |
| Models and evaluation (OCR only if C1 is built) | AI | Unset | Unset |
| LINE and external service fees | TL | Unset; TD5 option A is about ฿2,740 for two months (estimate) | Unset |
| Security/privacy/legal review | PS | Unset | Unset |
| User research and controlled evaluation support | PD | Unset | Unset |
| Payment/merchant costs | PD/TL | None: the prototype handles no money | None |
| Contingency | SP | Unset | Unset |

Obtain approval before incurring expenses. No spending authorization or supplier contract is implied by this document. Do not treat the earlier brief's illustrative ฿300,000 fixed-cost scenario as this project's approved budget. Identify who funds the LINE plan (team or department) when TD5 is decided.

## 11. Proposal and project deliverables

### Required for 22 November submission

- Problem definition, target users (TD1), evidence, objectives, and non-goals.
- Related work (Abdul and alternatives) if the course rubric requires it.
- The combined proposal, with the level-of-action model, lifecycle and evidence levels, and the approval-gate rule.
- SRS v0.9 and the requirements register: IDs, priority, acceptance criteria, verification methods, and scope exclusions.
- Final architecture diagrams, data flows, data model, interfaces, deployment/recovery design, and rationale.
- Complete core flows (obligation; request to a business with handoff and follow-up), including errors, uncertainty, approvals, Withdraw and expiry.
- Selected AI and integration approach with evidence and approved contingencies; the LINE handoff constraints.
- Threat model, data inventory, privacy approach, retention rules, and test plan, with evaluation thresholds E1–E14 frozen.
- Traceability table skeleton.
- Participant consent approach and the ethics approval request, covering businesses as third-party data subjects.
- Approved charter, named roles, capacity-based estimate and forecast, phases, dependencies, costs, and risk register.
- Submission artifact in the required format, approval record (including advisor sign-off if required), and receipt.

No working implementation is required unless the recipient's rubric explicitly adds it. Any demonstration or feasibility spike should be agreed and labeled according to what it proves.

### Required for project closure

- Source, configuration documentation, and a reproducible deployment process.
- Accepted evaluated prototype with required features and declared limitations.
- SRS v1.0 baselined, requirement/test traceability, security review, and operational recovery evidence.
- Controlled evaluation with 15–20 testers covering E1–E14, with confidence intervals and clear limits on conclusions about market demand or paid retention.
- Abdul comparison from public information only (E13).
- User/operator guides, incident procedures, known issues, and maintenance ownership.
- Secure transfer of necessary access; no secrets committed into the repository.
- Final report, demonstration, and acceptance record required by the project recipient.

### Prototype operation boundary

Use synthetic records and low-sensitivity tasks with a small invited cohort of testers aged 20 or over (or married). No medical requests, no real money movement, and no reliance on the prototype for consequential bill payments, tax filing, or real booking commitments.

**The assistant never sends a message to a business by itself.** The user's own tap sends it (LINE handoff or copy-and-send). Do not describe the prototype as assistant-executed booking. Every card states that an AI assistant prepared it for a named user and offers a way to stop further requests. Status shows "sent", "answered" or "done" only when the matching evidence exists.

The team has no assumed 24/7 support capacity. Publish the actual staffed response windows for evaluation (including the S2 operator window if built), provide a stop/disable route, and pause evaluation when a material incident cannot be handled safely. Live commercial payment collection and handling of sensitive personal documents require a separately approved expansion and review.

The architecture should retain strong authentication, least privilege, the approval gate, source/evidence rules, and action controls. Prototype recovery and availability targets are deliberately limited and disclosed; they are not service-level promises for a commercial product.

## 12. Approval and document maintenance

| Item | Name | Decision / signature | Date | Version |
|---|---|---|---|---|
| Sponsor approval | ______ | ______ | ______ | ______ |
| Product/scope readiness | ______ | ______ | ______ | ______ |
| Technical/design readiness | ______ | ______ | ______ | ______ |
| Quality/operations readiness | ______ | ______ | ______ | ______ |
| Privacy/security review | ______ | ______ | ______ | ______ |
| Advisor / committee (if required by the course) | ______ | ______ | ______ | ______ |

PD maintains this charter and the timeline. Update both when dates, capacity, scope, or responsibilities change. Record what changed and why; do not overwrite the history of approved commitments. **The timeline (v0.3) is aligned to this charter. The kickoff file still describes the obligations-only scope and the F01–F11 IDs; update it after TD1–TD6 are decided.** Both documents use the TD recommendations as a working baseline until then.

| Version | Date | Change | Author / approver |
|---|---|---|---|
| 0.1 | 7 Nov 2026 | Initial proposed governance and charter | Prepared for team review; not yet approved |
| 0.2 (review copy) | 9 Nov 2026 | Validator corrections; see `../review/revised/omm/project-charter.md` | Not yet approved |
| 0.3 | 10 Nov 2026 | Aligned to the combined proposal v0.2 and SRS v0.9; TD1–TD6 register added; scope, effort, gates, acceptance criteria, risks and boundary updated | Draft; not yet approved |
