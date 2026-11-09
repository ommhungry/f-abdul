# Project Governance and Charter

> **Revision notes (9 Nov 2026)** — validator's revised copy of omm's charter v0.1. Not approved by the team. Original: `docs/governance/project-charter.md` (unchanged). Full reasoning: `docs/review/validation-omm-plan.md`.
>
> 1. Header: capacity line now states that holidays and exams are not yet subtracted; added a status note on the widened idea (vj booking router, jui request cards).
> 2. Related-file links now point to the revised sibling copies and to the research brief from this folder.
> 3. §1: added a "pending combined scope" paragraph; D01 is due today and may slip; no product redesign.
> 4. §2: business hypotheses are recorded as hypotheses, not measured, in a 5–8-person evaluation (was "must be measured").
> 5. §3: scope reduction order now requires an explicit ordered list of conditional and widened features as a D10 output.
> 6. §4: added an external Advisor (AD) role (faculty advisor / course committee) and an advisor line in the roster; clarified that SP is the team's student owner and that external approval follows course rules (UNVERIFIED).
> 7. §5 and §10: the responsibility matrix and the budget table are marked "fill only the rows that apply"; added a note on how AD approval relates to SP.
> 8. §7: risk/capacity review folded into the weekly check-in (full review fortnightly); meeting cost totalled (≈24.75 team-hours over 22 weeks); the repository now exists; statuses reduced to five.
> 9. §8: gates G3–G8 fall on Sundays, so a "review by" Friday date is added; G3–G5 acceptance needs a demo reviewed by another member; advisor checkpoints added at G2, G6, G8; Definition of Done tiered into "every task" and "sensitive or release task"; Definition of Ready also tiered (permission/data impact and test approach only for sensitive tasks).
> 10. §8: added three lightweight artefacts: requirements specification, traceability table, test plan.
> 11. §9: a light path for small changes (one decision-log line).
> 12. §10: risk register gains a likelihood/impact column and six new risks (academic calendar, member unavailability, scope churn, ethics/consent, LINE platform limits and pricing, AI provider terms).
> 13. §11: proposal deliverables add related work (if the rubric requires it), the requirements/traceability/test-plan artefacts, ethics/consent, and advisor sign-off.
> 14. §12: advisor line in the approval table; version 0.2 row added.
> 15. Header version changed to 0.2 (review copy).
> 16. §10: the calendar risk row now lists Mon 7 Dec 2026 (substitute holiday, P1) and Thu 10 Dec 2026.


**Project:** Thailand LINE AI Personal Assistant  
**Version:** 0.2 (review copy, 9 Nov 2026) — based on omm's 0.1 of 7 November 2026; incorporates confirmed three-person capacity  
**Status:** Proposed charter for team discussion and approval  
**Timezone:** Asia/Bangkok  
**Proposal deadline:** 22 November 2026  
**Completion deadline:** 30 April 2027 — confirmed  
**Team capacity:** Three members, all familiar with app development, each contributing equally at 3–4 hours/week; 9–12 total team-hours/week. Public holidays and university exam periods are not yet subtracted; see the timeline, section 2  
**Scope status (9 Nov):** under review. The team has widened the idea (vj's booking router, jui's request-card assistant); this charter's scope applies until the combined direction is decided

Related files: [Today's meeting](2026-11-07-kickoff.md), [timeline and development phases](timeline-and-development-phases.md), [research and architecture brief](../../../research/business-and-architecture-brief.md).

## 1. Purpose and project authorization

Create a LINE-based personal assistant that helps working adults in Thailand capture, organize, and complete everyday obligations. Prioritize individual use and support explicit sharing of selected obligations with family members.

The project is new. Team size, development familiarity, weekly hours, and the final deadline are confirmed. Member names, role assignments, detailed scope, and design alternatives remain open. This charter provides a structure for those decisions; it does not claim that people have accepted roles or that the proposed scope is approved.

The proposal submitted on **22 November 2026** must contain the finalized architecture and overall design. Product implementation does not have to be finished at that point. The recommended delivery objective for the confirmed capacity is a **small evaluated prototype and handover by 30 April 2027**. A broader real-user pilot or commercial launch would require a separately funded expansion. The team must approve this completion definition by 9 November. Because the idea widened on 8–9 November, record either the decision or the slip and a new date in the decision log; do not leave D01 silently open.

The earlier research brief remains the long-term product vision. This charter proposes a smaller implementation baseline; it does not imply that the full vision fits the available hours.

**Pending combined scope.** vj's booking router (`docs/review/source-vj/`) and jui's request-card assistant (`docs/research/ai-assistant-jui/`) make booking or business-facing actions central, while this charter keeps booking conditional with no committed hours. When the team chooses the combined direction, revise §1–3, §10, and §11 together with the timeline's work packages. jui's plan assumes 15–20 hours per member per week; the confirmed capacity remains 3–4.

## 2. Objectives and success

| Objective | Evidence of success |
|---|---|
| Reduce individual administrative effort | A real message/document becomes a useful tracked obligation with minimal re-entry |
| Provide trustworthy follow-through | Source-backed dates/amounts, explicit uncertainty, and evidence-aware completion |
| Support family coordination safely | Accepted resource-specific sharing, permissions, and revocation without private-data leakage |
| Establish feasible external actions | Documented access and verified outcomes for any committed integration |
| Deliver a controlled, maintainable prototype | Proportionate threat model, tested access controls, recovery evidence, and handover documentation |
| Complete the proposal on time | Approved design package plus submission receipt by the actual cutoff |
| Complete the project on time | Accepted prototype, evaluation report, known limitations, and named maintenance/closure owner |

Business hypotheses—paid retention, willingness to pay, contribution margin—are recorded as hypotheses. A 5–8-person prototype evaluation cannot measure them; report only what it can show. A weak result is a reason to revise the business, not to conceal results or relabel incomplete software as complete.

## 3. Baseline scope proposal

Feature IDs are shared with the meeting and timeline documents.

### Required baseline, subject to team approval

- **F01:** LINE onboarding, verified identity, private user accounts, and defined account recovery.
- **F02:** Text capture with bounded AI-suggested dates/fields, source text, and mandatory confirmation of consequential uncertainty. Document-image/OCR extraction is conditional, not included in the baseline estimate.
- **F03:** Validated dates/amounts, recurring reminders, snooze, and completion recording.
- **F04:** Upcoming known obligations and distinct user-reported versus externally verified status.
- **F05:** Minimal selective family sharing, invitation acceptance, responsibility assignment, and revocation.
- **F06:** Minimum authorization, audit, privacy controls, export/deletion, observability, recovery, and cost monitoring appropriate to the prototype. Commercial operating scale is not included.

### Conditional scope

- **F07:** A calendar connection if utility, access, verification, and capacity support it.
- **F08:** One booking category with approved execution access or an explicitly designed hosted handoff.
- **F09:** Administrative tax-document checklist using reviewed sources.
- **F02 extension:** One tightly bounded image/OCR format only if its incremental estimate fits spare capacity. Text-first operation remains fully usable without it.

Conditional features are not silently included in the commitment. By **18 November**, select which are required for the April release, optional within spare capacity, or deferred. Every committed external dependency must have a chosen fallback that still satisfies the submitted design.

### Deferred or excluded baseline

- **F10:** Voice input, broad inbox ingestion, and telephone booking are deferred by default.
- **F11:** Autonomous banking/tax filing, unrestricted group reading, and universal booking are excluded.
- No stored-value wallet, independent transfer service, medical advice, or broad commercial availability is assumed.

### Scope reduction order

When resources are insufficient, remove all conditional features before reducing the required baseline. Then narrow supported recurrence rules, input formats, and interface polish while preserving a useful core journey, minimal family sharing, authorization, privacy, and evidence rules. Bound AI interpretation and keep user-confirmed structured input as a fallback. If these still cannot fit, the sponsor must revise resources or completion criteria explicitly rather than assume unpaid overtime.

The order **among** conditional features (F07, F08, F09, F02 image/OCR) and any features added from the widened idea is not yet set. D10 (18 November) must record it as a numbered list so that the first cut is not debated under pressure.

## 4. Governance structure and role descriptions

Roles describe accountability, not additional headcount. Allocate them across the **three confirmed members** by **9 November**. One person holds several roles; each decision or deliverable still has one accountable owner. Time spent in review, meetings, and documentation is part of each person's 3–4 hours.

| Code | Role | Responsibilities | Decision authority | Required outputs |
|---|---|---|---|---|
| SP | Sponsor / project owner | Own purpose, budget, scope priorities, deadline, and final acceptance | Approves scope baseline, material changes, spending, and project acceptance | Charter approval, scope decisions, acceptance record |
| PD | Product and delivery lead | Requirements, backlog, user research coordination, schedule, dependencies, meeting records, submission coordination | Prioritizes within approved scope; escalates material changes | Requirements, sprint goals, status reports, proposal package |
| TL | Technical lead / backend lead | Architecture, data model, workflow execution, interfaces, engineering standards, integration design | Approves technical design within agreed constraints | Architecture decisions, backend delivery, integration contracts |
| AI | AI and document-processing lead | Extraction, Thai-language evaluation, model/OCR integration, source grounding, cost control | Selects implementation within approved provider/data policy | Evaluation set, extraction pipeline, model reports |
| UX | UX and LINE/frontend lead | User flows, embedded web interface, Thai copy, consent/approval screens, usability | Owns interface decisions within approved journeys and privacy constraints | Wireframes, flow specifications, interface implementation |
| QO | Quality and operations lead | Test strategy, integration/system verification, deployment, monitoring, backup/restore, incident drills | Accepts verification evidence; can block unsafe release | Test reports, release checklist, runbooks, recovery evidence |
| PS | Privacy and security owner | Threat model, data inventory, permissions review, retention/deletion, external legal/security coordination | Can block release for unresolved material privacy/security issues | Privacy/security review, risk treatment, data-flow assessment |
| AD | Faculty advisor / course committee (external to the team) | Approves the proposal and final acceptance under the course's rules; reviews progress | As set by the course (UNVERIFIED; obtain the rules) | Proposal approval, progress feedback, final grade or acceptance |

SP is the team's own student owner of scope and priorities. Where the course gives approval or acceptance authority to the advisor or committee (AD), AD's decision prevails and SP prepares it. External legal or security advisers may be consulted where the team lacks competence. PS is an internal accountability role and does not imply a legal qualification or an automatic statutory Data Protection Officer appointment.

### Recommended three-person role mapping

Names and specialist strengths are unknown. Use this as the starting assignment proposal, then rebalance after the meeting:

| Planning seat | Primary responsibility | Additional responsibility | Key caution |
|---|---|---|---|
| Member 1 | Product, coordination, and LINE/interface delivery — PD/UX | User sessions and proposal assembly; SP only if explicitly assigned | Cap coordination; other members own their documentation and tests |
| Member 2 | Architecture, backend, workflow, and deployment — TL | Recovery implementation; reviews Member 3's AI and test changes | Member 3 reviews permission-sensitive backend changes |
| Member 3 | Bounded AI, evaluation, and quality/privacy coordination — AI/QO/PS | Family-permission test design and evaluation reports | Testing and security implementation remain shared responsibilities |

Each member contributes **3–4 hours per week**, not 3–4 hours per role. Review pairs: Member 1's interface work → Member 2; Member 2's backend work → Member 3; Member 3's AI/evaluation work → Member 2, with Member 1 validating user-facing outcomes. Rebalance roles if the quality/privacy workload exceeds Member 3's allocation.

Two titles held by one person do not create independent review. At least one different capable person reviews authorization, sharing, credential, and release-sensitive changes. If the team cannot competently review a material risk, use qualified advice or keep that capability out of the prototype.

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
| Research and requirements | C | A/R | C | C | R | C | C |
| Architecture and technical decisions | I | C | A/R | R | C | C | C |
| Data inventory and privacy/security design | I | C | R | C | C | R | A/R |
| Individual and family user experience | I | C | C | C | A/R | C | C |
| Backend, workflow, and permissions | I | I | A/R | C | C | R | C |
| AI/OCR extraction and evaluation | I | C | C | A/R | C | R | C |
| External integration feasibility and implementation | I | C | A | R | C | R | C |
| Test strategy and acceptance evidence | I | C | R | R | R | A/R | C |
| Deployment, monitoring, and recovery | I | I | R | C | C | A/R | C |
| Proposal assembly and submission | A | R | R | C | C | C | C |
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

## 7. Delivery method and working cadence

Use an iterative SDLC with formal gates: requirements and design are finalized for submission, implementation proceeds in small increments, and evaluation access depends on evidence. Testing, security, privacy, and documentation run throughout development. The proposed post-submission prototype effort is 124–172 hours; the timeline provides the capacity calculation and reduction triggers.

| Activity | Suggested cadence | Owner | Output |
|---|---|---|---|
| Proposal coordination, 9–20 Nov | Two brief asynchronous updates per week plus targeted decision comments | PD | Decisions, blockers, next actions |
| Development coordination | One 15-minute team check-in per week | PD/TL | Progress and dependency changes |
| Iteration planning and demonstration | Every two weeks; replace that week's check-in with a 30-minute session | PD/TL | Goal, selected work, demonstrated increment |
| Risk/capacity review | Five minutes inside the weekly check-in; full review every two weeks | PD | Updated risk register and forecast |
| Architecture/security review | At relevant changes and phase gates | TL/PS | Decision or findings record |
| Evaluation review | Weekly during evaluation, within the existing check-in | PD/QO | Usage, incidents, feedback, cost, fixes |
| Retrospective | One asynchronous observation per member per iteration | Whole team | One concrete process change |

These are proposed frequencies; agree actual times around availability. A 15-minute meeting consumes 0.75 team-hours. A 30-minute meeting consumes 1.5 team-hours. Over 22 delivery weeks this cadence costs 11 × (0.75 + 1.5) = 24.75 team-hours, about 43% of the 25% allowance at the midpoint. Avoid adding ceremonies that consume the limited development budget.

### Work tracking rules

Each issue records a feature/requirement ID, owner, acceptance criteria, estimate, dependencies, evidence link, and status. Suggested statuses: Backlog → Ready → In progress → Review (including verification) → Done, with Blocked recorded as a label.

Use GitHub issues/board and pull requests in the existing repository (`ommhungry/f-abdul`). Store approved documents and decision records in the repository. Do not create fictional issue links.

Limit active work to what each person can finish and review. A task is not Done merely because code has been written or a demo recorded.

## 8. Quality gates and acceptance

| Gate | Target | Review by (working day) | Required evidence | Accountable owner |
|---|---|---|---|---|
| G0 — project setup decisions | 9 Nov 2026 (Mon) | 9 Nov | Named roster, actual capacity, completion definition, submission instructions | SP |
| G1 — requirements baseline | 13 Nov (Fri) | 13 Nov | Defined users/journeys, feature priorities, requirement IDs, initial acceptance criteria | PD |
| G2 — design/proposal approval | 20 Nov (Fri) | 20 Nov | Final architecture, data/permission model, flows, threat model, integration approach, approved scope, estimate and timeline; advisor sign-off if the course requires it | SP (AD approves) |
| G3 — foundations accepted | 13 Dec (Sun) | Fri 11 Dec | Verified identity, repeatable deployment, persisted records, basic monitoring; demo reviewed by QO or another member | TL |
| G4 — core individual flow accepted | 24 Jan 2027 (Sun) | Fri 22 Jan | Text-first F01–F04 demonstrated end to end; F06 controls exercised; demo reviewed by QO or another member | TL |
| G5 — feature complete | 14 Feb (Sun) | Fri 12 Feb | Required scope and any committed conditional features integrated; no new feature work planned; demo reviewed by QO or another member | TL |
| G6 — controlled evaluation readiness | 28 Feb (Sun) | Fri 26 Feb | System/security evidence, recovery drill, low-sensitivity test-data policy, invited cohort and support boundaries, participant consent (and ethics approval if the university requires it); advisor informed | SP |
| G7 — evaluation completed | 11 Apr (Sun) | Fri 9 Apr | Six-week observation window, defect status, costs, user findings, evidence limitations | PD |
| G8 — acceptance and handover | 25 Apr (Sun) | Fri 23 Apr | Accepted prototype, documentation, access transfer, maintenance owner, known-issues record; advisor/committee acceptance per course rules | SP (AD accepts) |
| G9 — closure deadline | 30 Apr (Fri) | 30 Apr | Remaining closure items resolved and final evidence archived | SP |

Calendar windows describe target completion; they do not require weekend work. G3–G8 fall on Sundays because phases run Monday to Sunday; hold the review on the Friday shown. Add the course's own milestones (progress report, defence, final submission) here once known.

### Lightweight engineering artefacts

| Artefact | Contents | Owner | First due |
|---|---|---|---|
| Requirements specification (`docs/requirements/`) | Requirement ID, statement, priority, acceptance criterion, source | PD | G1, 13 Nov |
| Traceability table | Requirement ID → design element → test ID → result | QO | Skeleton at G2; filled from P1 |
| Test plan | Test levels (unit, integration, system, acceptance), environments, entry/exit criteria, Thai evaluation-set size and policy | QO | G2, 20 Nov |

Keep each to a few pages; one table per artefact is acceptable.

### Proposed acceptance criteria to finalize by 18 November

| Area | Draft criterion | Evidence |
|---|---|---|
| Core journeys | Every required journey and its agreed failure cases pass | Requirement-to-test traceability and demonstrations |
| Identity and sharing | All defined unauthorized-access and revoked-permission cases are denied | Automated and manual isolation tests |
| Consequential actions | No action occurs outside valid authority; no uncertain outcome is reported as confirmed | Adversarial and failure-injection scenarios |
| Thai interpretation | Approved evaluation set covers supported text inputs, mixed language, dates, and ambiguity; numeric targets selected before evaluation | Versioned dataset, scoring rules, error analysis |
| Reminder execution | Proposed: at least 99% of 1,000 scheduled test jobs attempt delivery within 60 seconds under the agreed normal test load | Scheduler measurements; provider delivery and user attention measured separately |
| Recovery | Proposed prototype target: restore within 4 staffed working hours from a daily backup, with up to 24 hours' data loss in the declared disaster scenario | Measured restore exercise; clearly disclosed prototype limitation |
| Security/privacy | No unresolved critical/high findings under the agreed severity rubric; export/deletion and audit behavior verified | Review report and regression evidence |
| Usability | Representative users can complete the agreed primary flow; capture time/corrections | Usability results and fixes |
| Operations | Deployment, rollback, monitoring, incident, and cost procedures demonstrated | Runbooks and exercise results |

These targets are proposals and require feasibility review. Freeze the selected load profile, evaluation thresholds, dataset policy, and acceptance criteria with the design; do not select them after seeing results.

### Definition of Ready and Done

**Ready:** owner and reviewer named; requirement ID and acceptance criteria written; dependencies known. For permission- or data-sensitive tasks, also note the permission/data impact and test approach.

**Done (every task):** reviewed by another member, relevant tests passed, requirement ID linked, and short notes updated.

**Done (permission-, data-, or release-sensitive task):** all of the above, plus security/privacy implications addressed, evidence attached, and the accountable owner accepts the deliverable. Real-user deployment also requires the applicable release gate.

## 9. Change control

After 20 November, an approved design baseline remains the source of truth. Small changes that do not alter approved behavior, scope, dates, or data handling need only one line in the decision log and the normal review. Material changes follow a recorded process:

1. Submit a change request with reason, alternatives, affected requirements/design, effort, timeline, cost, security/privacy impact, and rollback/fallback.
2. TL and relevant specialists assess it; PD updates the delivery forecast.
3. SP approves material scope/budget/deadline changes. TL may approve internal implementation changes that preserve approved behavior and constraints.
4. Update the decision record, requirements, tests, timeline, and proposal addendum if required by the recipient.

Urgent incident containment may precede normal review. Record the action immediately and complete retrospective review on the next working day. Containment authority does not authorize unrelated feature changes.

## 10. Risks, resources, and dependencies

| Risk / dependency | Accountable role | Likelihood / impact (team to rate L/M/H) | Trigger | Response |
|---|---|---|---|---|
| Actual team capacity below estimate | PD | ___ | Available hours or completed work cannot support forecast | Re-estimate, reduce conditionals, rebaseline with SP |
| External integration access unproven | TL | ___ | Required evidence absent by 16 Nov | Select supported handoff or remove execution from baseline |
| Proposal requirements unknown | PD | ___ | No verified instructions by 9 Nov | Escalate immediately; obtain recipient requirements |
| Privacy/security expertise gap | PS | ___ | Team cannot assess a material risk | Seek qualified advice or reduce data/action scope |
| Single-person dependency | TL | ___ | No reviewer or backup for a critical subsystem | Cross-train, document, redistribute ownership |
| Model/provider cost overrun | AI | ___ | Measured usage exceeds approved budget | Usage limits, model routing, narrower feature scope |
| Evaluation cannot start safely on 1 Mar | QO | ___ | Readiness gate not met | Delay external access; use synthetic-data evaluation while fixing issues and disclose reduced user evidence |
| Holiday, leave, exams, or other availability | PD | ___ | Roster calendar shows reduced capacity | Reschedule work explicitly; preserve final contingency |
| Scope exceeds 9–12 team-hours/week | SP | ___ | Forecast exceeds remaining usable hours | Remove optional scope, narrow baseline deliberately, or add agreed capacity |
| University exam periods and public holidays | PD | ___ | Exam dates not recorded by 13 Nov; holidays Mon 7 Dec and Thu 10 Dec (P1), New Year (P2), Makha Bucha substitute 22 Feb (P4), Songkran 13–15 Apr (P6) | Record dates; rebaseline phases; protect P1–P4 |
| Member unavailable or leaves the course | SP | ___ | Any member below 3 h/week for two weeks | Redistribute ownership; cut conditionals; inform advisor |
| Scope churn from new ideas | SP | ___ | Directions not merged by the D03 date (happening now: vj, jui) | Time-box the merge; record one direction; park the rest as future scope |
| Human-participant ethics and consent | PS | ___ | Interviews or evaluation planned without consent (and ethics approval if the university requires it) | Pause recruitment until approved; use synthetic sessions |
| LINE platform limits and pricing | TL | ___ | Message quota, pricing, or API limits block a required flow | Design within limits; budget; fallback flow |
| AI provider terms and data handling | AI | ___ | Provider terms conflict with the data policy | Change provider or restrict data sent |

### Budget categories to assign

Fill only the categories that apply; a student prototype usually needs one small cloud and model budget.

| Category | Owner | Approved cap | Actual / forecast |
|---|---|---:|---:|
| Hosting, storage, backups, and monitoring | QO | Unset | Unset |
| Models, OCR, and evaluation | AI | Unset | Unset |
| LINE and external service fees | TL | Unset | Unset |
| Security/privacy/legal review | PS | Unset | Unset |
| User research and controlled evaluation support | PD | Unset | Unset |
| Payment/merchant costs if in scope | PD/TL | Unset | Unset |
| Contingency | SP | Unset | Unset |

Obtain approval before incurring expenses. No spending authorization or supplier contract is implied by this document. Do not treat the prior brief's illustrative ฿300,000 fixed-cost scenario as this project's approved budget.

## 11. Proposal and project deliverables

### Required for 22 November submission

- Problem definition, customer segment, evidence, objectives, and non-goals.
- Related work (Abdul and alternatives) if the course rubric requires it.
- Requirements with IDs, priority, acceptance criteria, and scope exclusions.
- Final architecture diagrams, data flows, identity/sharing model, interfaces, deployment/recovery design, and rationale.
- Complete core user flows, including errors, uncertainty, approvals, sharing, and revocation.
- Selected AI/OCR and integration approach, with evidence and approved contingencies.
- Threat model, data inventory, privacy approach, retention decisions, and test plan.
- Requirements specification and traceability table skeleton (see §8).
- Participant consent approach (and ethics approval request if the university requires it).
- Approved charter, named roles, capacity-based estimate, phases, dependencies, costs, and risk register.
- Submission artifact in the required format, approval record (including advisor sign-off if required), and receipt.

No working implementation is required unless the recipient's rubric explicitly adds it. Any demonstration or feasibility spike should be agreed and labeled according to what it proves.

### Required for project closure

- Source, configuration documentation, and a reproducible deployment process.
- Accepted evaluated prototype with required features and declared limitations.
- Requirement/test traceability, security review, and operational recovery evidence.
- Controlled evaluation covering utility, errors, and cost, with clear limits on conclusions about market demand or paid retention.
- User/operator guides, incident procedures, known issues, and maintenance ownership.
- Secure transfer of necessary access; no secrets committed into the repository.
- Final report, demonstration, and acceptance record required by the project recipient.

### Prototype operation boundary

Use synthetic records and low-sensitivity tasks with a small invited adult cohort. Do not depend on the prototype for consequential bill payments, medical tasks, tax filing, or real booking commitments. A hosted booking handoff may be demonstrated if approved, but it must not be described as assistant-executed booking.

The team has no assumed 24/7 support capacity. Publish the actual staffed response windows for evaluation, provide a stop/disable route, and pause evaluation when a material incident cannot be handled safely. Live commercial payment collection and handling of sensitive personal documents require a separately approved expansion and review.

The architecture should retain strong authentication, least privilege, explicit sharing, source/evidence rules, and action controls. Prototype recovery and availability targets are deliberately limited and disclosed; they are not service-level promises for a commercial product.

## 12. Approval and document maintenance

| Item | Name | Decision / signature | Date | Version |
|---|---|---|---|---|
| Sponsor approval | ______ | ______ | ______ | ______ |
| Product/scope readiness | ______ | ______ | ______ | ______ |
| Technical/design readiness | ______ | ______ | ______ | ______ |
| Quality/operations readiness | ______ | ______ | ______ | ______ |
| Privacy/security review | ______ | ______ | ______ | ______ |
| Advisor / committee (if required by the course) | ______ | ______ | ______ | ______ |

PD maintains this charter and the timeline. Update both when dates, capacity, scope, or responsibilities change. Record what changed and why; do not overwrite the history of approved commitments.

| Version | Date | Change | Author / approver |
|---|---|---|---|
| 0.1 | 7 Nov 2026 | Initial proposed governance and charter | Prepared for team review; not yet approved |
| 0.2 (review copy) | 9 Nov 2026 | Validator corrections; see revision notes | Not yet approved |
