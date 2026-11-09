# Validation of omm's governance, planning, and meeting documents

**Date:** 9 November 2026 (Monday)
**Validator role:** plan and governance validator for the review run
**Files checked (branch `main` content in the local clone):**

- `docs/governance/project-charter.md` (v0.1, 7 Nov 2026)
- `docs/planning/timeline-and-development-phases.md` (v0.1, 7 Nov 2026)
- `docs/meetings/2026-11-07-kickoff.md` (prepared agenda, 7 Nov 2026)
- `README.md` and `docs/README.md`

**Revision (9 Nov, after judge review):** all nine items in `docs/review/judge/judge-omm-plan.md` are applied here and in the revised copies.

**Revised copies:** `docs/review/revised/omm/project-charter.md`, `timeline-and-development-phases.md`, `2026-11-07-kickoff.md`. Each starts with a "Revision notes (9 Nov 2026)" block.

**Evidence tags.** `[V]` = verified from a primary source I opened (for this report that mostly means the repository files themselves, or arithmetic I recomputed from them). `[S]` = secondary source. `UNVERIFIED` = not checked. Estimates are labelled as estimates.

---

## 1. Summary verdict

**The plan is sound in method and almost error-free in arithmetic, but its capacity is optimistic and its scope is now out of date.**

- **Arithmetic: correct.** Every sum, product, and reconciliation I recomputed matches the documents to the stated decimal places [V]. Every date range and duration is right, and the two stated weekdays (Saturday 7 Nov, Sunday 22 Nov 2026) are right [V]. The only calendar error is one Gantt bar end date.
- **Capacity: optimistic.** The plan subtracts nothing for Thai public holidays or university exam periods. It says so openly (timeline section 2), but then plans P1–P4 at 98–99% of the 3.5-hour midpoint. If you remove the New Year and Songkran weeks and allow for two exam periods, usable hours at the midpoint fall from 173.25 to about 141.75. That is below the 148-hour midpoint workload (estimate; exam dates UNVERIFIED).
- **Internal inconsistency on the commitment basis.** The timeline says commitments use the "confirmed 3-hour lower bound", but the phase allocation is built at 3.5 hours. At 3 hours, P1–P4 are 14.1 hours short [V, recomputed].
- **Governance gap.** There is no faculty advisor, supervisor, or course committee anywhere in the governance. A student "sponsor" approves the scope and accepts the project. In a university senior project the advisor or committee normally holds that authority (university rules UNVERIFIED).
- **Weight.** The method is a good small-company process scaled down. For 9–12 team-hours a week it is still heavy: 7 roles, 10 gates, a 14 × 7 responsibility matrix, 6 work statuses, a 5-signature approval table, and about 24.75 team-hours of scheduled meetings over the delivery period.
- **Missing pieces.** There is no named test plan, no traceability matrix artefact, no requirements-specification format, no ethics or consent step for the human participants (interviews start tomorrow, 10 Nov, under action A04), and no advisor sign-off points.
- **Scope drift.** vj's booking router and jui's request-card assistant (TumHai) make booking or business-facing actions central. omm's plan gives booking zero hours, and its evaluation boundary forbids real booking commitments. jui's plan also assumes 15–20 hours a week per member, about five times the confirmed 3–4 hours [V, `docs/research/ai-assistant-jui/2026-11-08-agent-proposal-v2.md` line 144]. The charter, timeline, and kickoff will all need a scope revision once the team chooses the combined direction.

**Recommendation:** keep omm's structure as the project's governance backbone. Apply the fixes below, lighten the process, and redo the estimate after the combined scope is chosen. The proposal window is the most urgent risk: from today only **13.5–18.0 usable team-hours** remain before 22 November, against a 15–21-hour lean estimate that was written before the idea widened.

---

## 2. Recomputed capacity and calendar

### 2.1 Calendar checks (all recomputed) [V]

| Stated fact | Recomputed | Result |
|---|---|---|
| 7 Nov 2026 – 30 Apr 2027 = 175 days inclusive = 25 weeks | Nov 24 + Dec 31 + Jan 31 + Feb 28 + Mar 31 + Apr 30 = 175; 175 / 7 = 25 | Correct |
| 7–22 Nov = 16 days | 22 − 7 + 1 = 16 | Correct |
| 23 Nov – 25 Apr = 22 complete weeks | Nov 8 + Dec 31 + Jan 31 + Feb 28 + Mar 31 + Apr 25 = 154 = 22 × 7; 23 Nov is a Monday, 25 Apr a Sunday | Correct |
| 26–30 Apr = 5 days | Mon 26 – Fri 30 | Correct |
| P1 23 Nov–13 Dec = 3 weeks | 21 days | Correct |
| P2 14 Dec–24 Jan = 6 weeks | 18 + 24 = 42 days | Correct |
| P3 25 Jan–14 Feb = 3 weeks | 7 + 14 = 21 days | Correct |
| P4 15–28 Feb = 2 weeks | 14 days | Correct |
| P5 1 Mar–11 Apr = 6 weeks | 31 + 11 = 42 days | Correct |
| P6 12–18 Apr, P7 19–25 Apr = 1 week each | 7 days each | Correct |
| P1–P7 total = 22 weeks | 3 + 6 + 3 + 2 + 6 + 1 + 1 = 22 | Correct |
| "Saturday, 7 November 2026" (kickoff) | Saturday | Correct |
| "Sunday, 22 November" (submission) | Sunday | Correct |

**Weekdays of the other stated dates** (not stated in the documents; shown so the team can plan): 9 Nov Mon (G0), 10 Nov Tue, 13 Nov Fri (G1), 16 Nov Mon, 18 Nov Wed, 20 Nov Fri (G2), 21 Nov **Sat**, 22 Nov **Sun**, 13 Dec **Sun** (G3), 24 Jan **Sun** (G4), 14 Feb **Sun** (G5), 28 Feb **Sun** (G6), 31 Mar Wed, 11 Apr **Sun** (G7), 25 Apr **Sun** (G8), 30 Apr Fri (G9). Gates G3–G8 all fall on Sundays because each phase runs Monday to Sunday. The charter allows for this ("schedule reviews on an available working day before each window ends"), so it is not an error. The revised copies add the Friday review date for clarity.

### 2.2 Gantt check [V]

The chart claims it uses "the following day as a bar's exclusive end". That holds for P1–P8 (for example, P1 ends `2026-12-14` for 13 Dec). **It does not hold for P0:** the bar ends `2026-11-21`, which shows 7–20 Nov, while the table says 7–22 Nov. The fix is to end P0 at `2026-11-23`, or to split it into "design to 20 Nov" and "submission checks 21–22 Nov". The milestones (22 Nov, 30 Apr) are correct.

### 2.3 Capacity arithmetic as written [V]

Formula used by omm: members × hours/week × weeks × 0.75 (a 25% allowance for coordination and availability variation).

| Scenario | Team-h/week | Gross, 22 weeks | Usable (× 0.75) | Stated | Check |
|---|---:|---:|---:|---:|---|
| 3.0 h/person | 9.0 | 198.0 | 148.5 | 148.5 | Correct |
| 3.5 h/person | 10.5 | 231.0 | 173.25 | 173.25 | Correct |
| 4.0 h/person | 12.0 | 264.0 | 198.0 | 198.0 | Correct |
| Proposal window, 16 days | 9–12 | 20.57–27.43 | 15.43–20.57 | 15.4–20.6 | Correct |
| Closure reserve, 5 days | 9–12 | 6.43–8.57 | 4.82–6.43 | 4.8–6.4 | Correct |
| Kickoff meeting, 75 min × 3 people | — | 3.75 | — | 3.75 | Correct |

**Workload:** package lows 20 + 26 + 12 + 18 + 24 + 12 + 12 = 124; highs 28 + 36 + 18 + 24 + 32 + 18 + 16 = 172; midpoint (124 + 172) / 2 = 148. Headroom at 3.5 h: 173.25 − 148 = 25.25. Headroom at 3 h: 148.5 − 148 = 0.5 ("almost no headroom" is fair). All correct [V].

**Lean proposal estimate:** 2 + 3 + 4 + 3 + 1 + 2 = 15 and 3 + 4 + 5 + 4 + 2 + 3 = 21. Correct [V].

**Phase allocation table:** every row sums correctly (for example P7: 2.5 + 2.5 + 2.6 = 7.6). Member totals are 49.3 / 49.3 / 49.4, and the team total is 148.0. Phase capacities at 3.5 h (23.625, 47.25, 23.625, 15.75, 47.25, 7.875, 7.875) sum to 173.25. All correct [V].

**Work-package placement:** W1 = 23.4 + 0.6 = 24 (midpoint of 20–28); W2 = 31; W3 = 15; W4 = 21; W5 = 1.8 + 12.2 + 9 + 5 = 28; W6 = 15; W7 = 0.2 + 3.4 + 2.8 + 7.6 = 14. Each equals its package midpoint. W5 split: 1.8 + 12.2 = 14 before evaluation and 9 + 5 = 14 after. All correct [V].

**Window statements:** "each three-week window ≈ 23.6 h" (10.5 × 3 × 0.75 = 23.625) and "six-week window ≈ 47.3 h" (47.25). Correct [V].

**Meeting cost** (charter section 7, not totalled in the documents): one 15-minute check-in (0.75 team-h) and one 30-minute session (1.5 team-h) per two weeks gives 11 × 2.25 = **24.75 team-hours over 22 weeks**. That is about 43% of the 57.75-hour allowance at the midpoint (231 × 0.25). It fits, but it shows that ceremonies are a large share of the allowance [V, recomputed].

### 2.4 Recomputed capacity with holidays and exams (estimate)

The documents subtract **no** holidays or exam periods. They state that the 25% allowance "is not a statement about Thai public holidays" (timeline section 2). Public holidays inside the delivery period:

| Holiday | Date | Weekday | Phase | Source |
|---|---|---|---|---|
| King Bhumibol's Birthday, substitute day (holiday falls on Sat 5 Dec) | 7 Dec 2026 | Mon | P1 | [S] OfficeHolidays Thailand 2026, accessed 9 Nov 2026 |
| Constitution Day | 10 Dec 2026 | Thu | P1 | [S] OfficeHolidays Thailand 2026, accessed 9 Nov 2026 |
| New Year's Eve / New Year | 31 Dec 2026 – 1 Jan 2027 (long weekend 1–3 Jan) | Thu–Fri | P2 | [S] Nation Thailand, Bank of Thailand 2027 holiday list, accessed 9 Nov 2026 |
| Makha Bucha + substitute day | 21 Feb (Sun) + 22 Feb (Mon) 2027 | Sun–Mon | P4 | [S] Time Out Bangkok, OfficeHolidays, accessed 9 Nov 2026 |
| Chakri Memorial Day | 6 Apr 2027 | Tue | P5 | [S] Nation Thailand / Bank of Thailand list, accessed 9 Nov 2026 |
| Songkran | 13–15 Apr 2027 | Tue–Thu | **P6** | [S] Time Out Bangkok, OfficeHolidays, accessed 9 Nov 2026 |

Sources: [Nation Thailand — Bank of Thailand sets 18 bank holidays in 2027](https://www.nationthailand.com/news/general/40070369); [Time Out Bangkok — Thailand 2027 public holidays](https://www.timeout.com/bangkok/news/the-thailand-2027-public-holidays-092326); [OfficeHolidays — Thailand 2027](https://www.officeholidays.com/countries/thailand/2027); [OfficeHolidays — Thailand 2026](https://www.officeholidays.com/countries/thailand/2026). All accessed 9 Nov 2026. The cabinet's final 2027 list may add special holidays (UNVERIFIED).

**University exam periods are UNVERIFIED.** The team has not recorded its university's academic calendar. Many Thai universities hold first-semester final exams around late November to December and second-semester midterms around March (UNVERIFIED, general pattern only). The team must replace these assumptions with real dates. Two clashes are possible (UNVERIFIED): first-semester finals may overlap the proposal window (now to 22 Nov), and second-semester finals at many Thai universities can fall in late April or May, during P7–P8 handover (19–30 Apr). The revised timeline states that its phase allocation is still scenario A and must be rebaselined at action A11 (13 Nov).

**Scenarios (estimates):**

- **A — as written:** 22 full weeks.
- **B — holiday weeks removed:** the New Year week (28 Dec–3 Jan) and the Songkran week (12–18 Apr) count as zero. Single holidays (7 Dec, 10 Dec, 22 Feb, 6 Apr) are left inside the 25% allowance. That leaves 20 effective weeks. Zeroing the whole New Year week assumes members are away over New Year; if they are free during the break, scenario B understates capacity.
- **C — holidays and exams:** B, plus two exam periods of two weeks each at half capacity (the equivalent of 2 lost weeks). That leaves 18 effective weeks.

| Scenario | Effective weeks | Usable at 3.0 h | Usable at 3.5 h | Usable at 4.0 h | Gap vs 148 h midpoint (3.5 h) | Gap vs 124 h low (3.0 h) |
|---|---:|---:|---:|---:|---:|---:|
| A — as written | 22 | 148.5 | 173.25 | 198.0 | +25.25 | +24.5 |
| B — holidays | 20 | 135.0 | 157.5 | 180.0 | +9.5 | +11.0 |
| C — holidays + exams | 18 | 121.5 | 141.75 | 162.0 | **−6.25** | **−2.5** |

**Reading:** in scenario C even the low workload (124 h) does not fit at 3 hours a week, and the midpoint (148 h) does not fit at 3.5 hours. Only the 4-hour case still covers the midpoint (162 − 148 = +14), and even there the high estimate (172 h) does not fit.

### 2.5 Phase-level check at the stated 3-hour commitment basis [V, recomputed]

The dependency table says "Use confirmed 3-hour lower bound for commitments", but the allocation was built at 3.5 hours.

| Phase | Allocated (midpoint plan) | Capacity at 3.5 h | Capacity at 3.0 h | Gap at 3.0 h | Capacity at 3.5 h with holiday weeks removed |
|---|---:|---:|---:|---:|---:|
| P1 | 23.4 | 23.625 | 20.25 | −3.15 | 23.625 |
| P2 | 46.8 | 47.25 | 40.5 | −6.30 | 39.375 (New Year week) → **−7.43** |
| P3 | 22.8 | 23.625 | 20.25 | −2.55 | 23.625 |
| P4 | 15.6 | 15.75 | 13.5 | −2.10 | 15.75 (Makha Bucha substitute absorbed) |
| P5 | 24.0 | 47.25 | 40.5 | +16.50 | 47.25 |
| P6 | 7.8 | 7.875 | 6.75 | −1.05 | **0** (Songkran 13–15 Apr; about two working days left) → **−7.8** |
| P7 | 7.6 | 7.875 | 6.75 | −0.85 | 7.875 |
| **P1–P4** | **108.6** | **110.25** | **94.5** | **−14.1** | **102.375 → −6.2** |

P1–P4 use 98.5% of capacity at 3.5 hours. Any slip before evaluation can only be absorbed by moving work into P5, and the timeline itself warns that P5 slack "cannot rescue an earlier missed prerequisite".

### 2.6 Proposal window from today [V, recomputed]

From 9 to 22 Nov there are 14 days (2 weeks). Usable team-hours: 9 × 2 × 0.75 = **13.5** to 12 × 2 × 0.75 = **18.0** (15.75 at the midpoint). The lean estimate is 15–21 hours. That estimate was written before vj's and jui's directions had to be merged, and the kickoff (3.75 person-hours on 7 Nov, if held) and the 8 Nov research have already used part of the original 16-day window (hours not recorded, UNVERIFIED). **The proposal window is over-committed unless members work above 3–4 hours this fortnight by explicit agreement, or the proposal package is cut.**

The revised copies add proposal-window work. The requirements specification fits inside the existing "Requirements and complete core flows" package (3–4 h), and the test plan and traceability skeleton fit inside "AI feasibility, threat model, and verification strategy" (3–4 h). Actions A11 (calendar and advisor checkpoints), A12 (consent and ethics), and A13 (merging the three directions) are new work, estimated at about 2–4 team-hours. The proposal estimate therefore rises to about **17–25 h against 13.5–18.0 h** available (estimate). The revised timeline §4 and kickoff Discussion 6 now say this.

---

## 3. Findings table

Severity: **high** = threatens the proposal or the April delivery or misstates authority; **medium** = should be fixed before the 20 Nov approval; **low** = clarity or consistency.

| # | File | Location | Issue | Severity | Fix (applied in revised copy?) |
|---|---|---|---|---|---|
| 1 | timeline | §2 capacity; also charter header, kickoff Discussion 6 | No holidays or exam periods are subtracted. With New Year and Songkran weeks removed plus about 4 half-capacity exam weeks, usable hours are 121.5 / 141.75 / 162. The 148 h midpoint and the 124 h low no longer fit at 3.5 h and 3 h respectively (estimate) | High | Added holiday table and scenario table; require the team to record real exam dates by 13 Nov (yes) |
| 2 | timeline | §6 balanced contribution vs §7 dependency "use 3-hour lower bound" | The phase plan is built at 3.5 h but commitments are meant to use 3 h. At 3 h, P1–P4 are 14.1 h short | High | Added a 3 h capacity column and a note that P1–P4 need rebaselining or a stated commitment basis (yes) |
| 3 | timeline | §6 P6 (12–18 Apr), P2, P4 | Songkran 13–15 Apr falls inside the only stabilization week; New Year falls in P2; Makha Bucha substitute day (22 Feb) in P4 [S] | High | Flagged with options (end evaluation observation earlier, or accept a shorter stabilization); dates left for team decision (partly) |
| 4 | timeline, kickoff | §4 lean estimate; Discussion 6 | From 9 Nov only 13.5–18.0 usable hours remain against a 15–21 h estimate, before merging three directions | High | Added the remaining-window calculation, costed the work this revision adds (A11–A13 about 2–4 h; total about 17–25 h), and a cut-or-agree-extra-hours rule (yes) |
| 5 | charter | §4 roles, §8 gates, §12 approvals | No faculty advisor / supervisor / course committee. A student SP approves scope and gives final acceptance. Advisor sign-off points are absent (university rules UNVERIFIED) | High | Added an external Advisor (AD) row, advisor checkpoints at G2, G6, G8, and an advisor line in approvals (yes) |
| 6 | all three | Scope, W1–W7, evaluation boundary | Widened idea (vj booking router, jui request cards): booking has 0 hours; the prototype boundary forbids real booking commitments; jui assumes 15–20 h/week and 200 pilot users; target user differs (working adults vs university students and staff vs diners) | High | Added "pending combined scope" notes only; no product redesign (noted) |
| 7 | charter, kickoff | A04 interviews (10 Nov); §11 operation boundary; P5 testers | No research-ethics or consent approval step for human participants (interviews, 5–8 testers; jui adds real businesses). Universities often require ethics review (UNVERIFIED) | Medium | Added a risk row, a deliverable, and action A12 (yes) |
| 8 | charter, timeline | §8, §11 | No named test plan, traceability matrix, or requirements-specification format, although acceptance criteria rely on "requirement-to-test traceability" | Medium | Added three lightweight artefacts with owners and due dates (yes) |
| 9 | charter | §7 cadence, §5, §8 | Process heavy for 9–12 team-h/week: 7 roles, 10 gates, 14 × 7 matrix, 6 statuses, separate weekly risk review, 5-signature approval, 7 budget categories. Meetings alone ≈ 24.75 team-h | Medium | Folded the risk review into the check-in; reduced statuses to 5; tiered the Definition of Done; marked the responsibility matrix and budget table as "fill only rows that apply" (yes) |
| 10 | charter | §10 risk register | No likelihood/impact rating, review date, or status. Missing risks: exam periods, member unavailability or drop-out, scope churn (happening now), ethics approval, LINE pricing and messaging limits, AI provider terms | Medium | Added a rating column and six rows (yes) |
| 11 | charter | §3 scope reduction order | No order among conditionals (F07, F08, F09, OCR) and no rule for the widened features | Medium | Made the ordered list an explicit D10 output (yes; order left to team) |
| 12 | charter | §8 Definition of Done | One heavy definition applied to every issue (security review, documentation, evidence, accountable acceptance) | Medium | Split into "every task" and "sensitive or release task" (yes) |
| 13 | charter | §8 gates G3–G5 | TL is accountable for accepting TL-led work (self-acceptance) | Low | Added "accepted after a demo reviewed by QO or another member" (yes) |
| 14 | timeline | §5 Gantt, P0 bar | Bar ends 2026-11-21 (shows 7–20 Nov), contradicting the exclusive-end convention and the 7–22 Nov table | Low | Split into a design bar (draws 7–20 Nov, matching G2) and a submission bar (draws 21–22 Nov). This fits both the bar label and the table better than extending one bar to 23 Nov. Added holiday markers (yes) |
| 15 | timeline | §4 windows 21–22 Nov; §5 gates | Submission checks on Saturday 21 Nov and submission on Sunday 22 Nov contradict "calendar windows do not require weekend work"; G3–G8 fall on Sundays | Low | Added weekdays and a "review by Friday" date; only the 21 Nov checks are optional, and the 22 Nov submission is mandatory unless done earlier (yes) |
| 16 | timeline | §6 W5 and W7 "main phases" | W5 is listed as P1–P4 but its hours are placed in P3–P6. W7 is listed as "all phases" but its hours are in P2, P4, P6, P7 | Low | Corrected the phase columns (yes) |
| 17 | charter | §2 last paragraph | Says business hypotheses (paid retention, willingness to pay) "must be measured", but a 5–8-person evaluation cannot measure them (timeline §8 P5 says so) | Low | Reworded: record as hypotheses; do not claim measurement (yes) |
| 18 | charter | §7 work tracking | "once the repository exists": it exists (`ommhungry/f-abdul`) [V, team brief] | Low | Updated (yes) |
| 19 | kickoff | Agenda leads, action register | Role names (Project lead, Product owner, Delivery lead, QA, Technical/integration lead) do not match charter codes (SP, PD, TL, QO, ...) | Low | Mapped to charter codes (yes) |
| 20 | kickoff | D06 vs charter §3 | OCR inclusion decided in D06 (16 Nov), but the charter selects conditionals by 18 Nov (D10) | Low | D06 = evidence by 16 Nov; inclusion decided in D10 on 18 Nov (yes) |
| 21 | kickoff | Live decision log | Two ID schemes (D01–D10 and DEC-001) without a link | Low | Added a "Resolves" column (yes) |
| 22 | kickoff | "Read before" paragraph | "The founder confirmed" — this is a three-student university project | Low | Changed to "The team confirmed" (yes) |
| 23 | kickoff | Status line | Today is 9 Nov; whether the 7 Nov meeting was held and what it decided is not recorded | Low | Status note added; team to fill outcomes (yes) |
| 24 | README.md | Description, structure | Target user and the structure tree predate the review (no `docs/review/`, `docs/research/ai-assistant-jui/`, `docs/requirements/`, `docs/combined/`) | Low | Not revised (outside my output paths); manager to update after integration |
| 25 | docs/README.md | Status table | Planning row says "confirmed three-person capacity", which is correct, but there is no row for the new review, requirements, and combined folders | Low | Not revised; manager to update |
| 26 | charter | §11 proposal deliverables | No related-work / background-literature section, which university proposal rubrics usually expect (rubric UNVERIFIED) | Low | Added "related work (Abdul and alternatives) if the rubric requires it" (yes) |

**Checked and found consistent across the five files [V]:** proposal date 22 Nov 2026; completion 30 Apr 2027; capacity 3 × 3–4 h; feature IDs F01–F11 and their classes (charter §3 = kickoff Discussion 3 = timeline §6 conditionals); decision dates D01–D10 match charter gates G0–G2 and timeline windows; 124–172 h and 148.5–198 h figures repeated identically; evaluation start 1 Mar and the six-week window; feature freeze 14 Feb; defect-only after 11 Apr; the member-to-role mapping (Member 1 PD/UX, Member 2 TL, Member 3 AI/QO/PS) matches the phase owners; every row of the responsibility matrix has exactly one accountable role; the 75-minute agenda sums to 75.

---

## 4. Method against good practice for a small student software project

**SDLC choice — good.** Iterative development with fixed gates suits fixed academic deadlines and part-time work. It sits between a plain waterfall (late integration) and open-ended Scrum (no protection for fixed dates). The two-week iteration with a demo is the right rhythm. Keep it.

**Where it is too heavy for 3–4 hours a week:**

- **Roles.** Seven role codes for three people produce a long roster table that will stay half empty. Keep the codes as labels of responsibility, but fill one line per person.
- **Ten gates.** G3–G5 can be ordinary iteration demos with a one-line acceptance note. Keep formal evidence for G1, G2, G6, G8, and G9.
- **Responsibility matrix.** 14 rows × 7 roles. With three people a simple owner/reviewer list per deliverable carries the same information.
- **Work statuses.** Six statuses plus Blocked. Four or five board columns are enough.
- **Weekly risk/capacity review as a separate activity.** Fold it into the 15-minute check-in; do the full review fortnightly.
- **Definition of Done.** The same security, privacy, and documentation checklist applies to every issue. Tier it.
- **Budget table with seven categories** and a five-signature approval table. These are useful only if the course requires them. A student prototype usually has one small cloud and model budget.
- **Change control.** Eight impact fields for every change is too much. Keep the full form for material changes; for small ones, use a one-line record in the decision log.

**Where it is missing something:**

- **Advisor or supervisor sign-off.** There are no checkpoints with the faculty advisor or committee (proposal approval, progress review, final defence). Dates UNVERIFIED; obtain them from the course.
- **Requirements baseline format.** G1 asks for "requirement IDs" but does not say where or how (for example a short software requirements specification (SRS) in `docs/requirements/` with ID, statement, priority, acceptance criterion, and source).
- **Traceability.** It is required as evidence but has no artefact. A single table (requirement ID → design element → test ID → result) is enough.
- **Test plan.** "Testing strategy" is listed, but there is no document with test levels (unit, integration, system, acceptance), environments, entry and exit criteria, and the Thai evaluation-set size.
- **Ethics and consent.** Interviews (A04, 10 Nov) and the evaluation (5–8 testers) involve people. University ethics review may be required (UNVERIFIED). Personal-data consent under Thailand's Personal Data Protection Act (PDPA) is needed in any case (the act itself is not opened here; see jui's legal file).
- **Risk register quality.** It has triggers and responses, which is good, but no likelihood × impact rating, owner names, or dates, and several student-specific risks are missing.
- **Academic calendar in the plan.** Exams and holidays are left to "reforecast later". For a student team they are the largest predictable capacity loss and should be in the baseline.
- **Velocity check.** The forecast compares planned and used hours. Adding "planned vs completed requirement IDs per iteration" would show earlier whether scope fits.

---

## 5. Impact of the widened idea (vj and jui) — what will need to change

I do not redesign the product. These are the parts of omm's plan that will need revision once the team chooses the combined direction.

| Area | omm's current plan | What vj / jui add | What must change in the plan |
|---|---|---|---|
| Target user | Working adults in Bangkok, plus family | jui: university students and staff first [V, jui README]; vj: diners and venue bookers in one city [V, vj scope] | Charter §1–2, kickoff Discussion 2, P5 cohort description, README description |
| Scope classes | Booking F08 conditional, 0 hours; F10 phone booking deferred; F11 universal booking excluded | vj: booking router (API → web form → LINE handoff → email → phone/voice) and a venue channel database as the core; jui: 10 actions, request cards (Level 2) as the "core differentiator", direct booking (Level 3) | Feature list F01–F11, scope reduction order (jui has its own drop order 10 → 9 → 5 → 6), D03, D07, D10 |
| Effort estimate | W1–W7 = 124–172 h, no booking | Request card and reply page, business directory, routing, follow-up | A new work package and re-estimate; something in F01–F06 must give way at 3–4 h/week |
| Capacity assumption | 3–4 h/person/week (confirmed) | jui: 15–20 h/person/week for 7 months [V, jui v2 line 144]; build plan of 7 months with 200 users | jui's targets (O2 300 requests, O5 200 users, O6 ≥10% paying) cannot be carried over unchanged |
| Evaluation boundary | 5–8 invited adults, synthetic or low-sensitivity tasks, no real booking commitments | vj: concierge bookings done by team members by hand; jui: 50 real cards sent to real businesses, a 200-user pilot | Charter §11 prototype boundary; P5 design; concierge work also consumes team hours |
| Stakeholders | Users and family members | Businesses who receive request cards or bookings | Ethics and consent for businesses; new risks (non-response, AI-disclosure wording) |
| Platform constraints | LINE identity and messaging | vj: the LINE Messaging API cannot message another business's official account, so a user-tap handoff is needed [as stated by vj, UNVERIFIED by me]; jui: free reply quota changes on 1 Feb 2027 [jui tags it V; UNVERIFIED by me] | Risk register; D07 integration strategy |
| Acceptance criteria | "No uncertain outcome reported as confirmed" | Booking or request outcomes are uncertain until the business replies | The existing criterion fits well; add booking-specific failure cases |
| Decisions due | D01 completion definition due today (9 Nov) | Direction not yet merged | D01 and D03 cannot be closed as written; the team should record the slip and a new date |

---

## 6. Strengths worth keeping

1. **Honest, reconciled arithmetic.** Every figure ties out to the decimal, and the documents state their formula. This is rare and valuable for an assessed proposal.
2. **"No hidden overtime" rule.** Equal contribution within agreed hours, with extra work volunteered and recorded.
3. **Conditional features carry zero committed hours** until a lifecycle estimate and approval exist. This protects the core.
4. **Evidence and status discipline.** "No uncertain outcome is reported as confirmed" and "user-reported versus externally verified status" fit any assistant that acts, including booking and request cards.
5. **Thresholds frozen before results.** Acceptance criteria and evaluation thresholds are fixed with the design, not after seeing data.
6. **Independent review for sensitive changes.** Two titles held by one person do not count as independent review.
7. **Scope reduction protects privacy and authorization** rather than trading them away for a demo.
8. **Decision register with alternatives, owners, and due dates** (D01–D10), and "TBD is not an architecture".
9. **"What each phase must demonstrate"** gives testable exit conditions per phase.
10. **Clear separation of the long-term vision from the April commitment**, and a realistic small evaluated prototype.
11. **Timezone and weekday awareness** (the Sunday submission warning).

---

## 7. What I could not verify

- The university's exam periods, academic calendar, advisor or committee milestones, proposal rubric, and ethics-review requirement (all UNVERIFIED).
- Whether the 7 Nov kickoff took place and what it decided; no minutes are filled in.
- Final 2027 Thai holiday dates (secondary sources only; the cabinet may add special holidays). The December 2026 dates (7 and 10 Dec) were checked against a secondary source only.
- Hours already spent on the proposal window (7–9 Nov).
- vj's LINE Messaging API limitation and jui's LINE quota change; I did not open LINE's documentation.
