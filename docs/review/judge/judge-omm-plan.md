# Judge verdict: omm-plan card

**Date:** 9 November 2026
**Card:** omm-plan (validate omm's charter, timeline, kickoff, README.md and docs/README.md; write a validation report and three revised copies)
**Files judged:**

- `docs/review/validation-omm-plan.md`
- `docs/review/revised/omm/project-charter.md`
- `docs/review/revised/omm/timeline-and-development-phases.md`
- `docs/review/revised/omm/2026-11-07-kickoff.md`

The judge read these against the originals: `docs/governance/project-charter.md`, `docs/planning/timeline-and-development-phases.md`, `docs/meetings/2026-11-07-kickoff.md`, `README.md` and `docs/README.md`. The judge also ran a line-by-line diff of each original against its revised copy.

## Verdict: PASS WITH MINOR FIXES

The work does what the assignment asked. All four parts are covered: arithmetic and calendar, consistency across the files, method against good practice, and the effect of the widened idea. Every figure the judge recomputed is right. The holiday dates are right. The revised copies keep omm's structure and voice, and nothing of substance was removed without being recorded. The weaknesses are small. The revisions add new work to the proposal window, which the report itself says is already over-committed, but they do not cost that work. There are also a few wording errors, two small changes that are missing from the revision notes, and one missed public holiday. Each needs only a short text edit. None needs a redesign.

## 1. Independent re-checks

### 1.1 Arithmetic and calendar (recomputed by the judge) [V]

| Item | Judge's result | Agrees with the worker? |
|---|---|---|
| 7 Nov 2026 to 30 Apr 2027 | 175 days inclusive = 25 weeks | Yes |
| 23 Nov to 25 Apr | 154 days = 22 weeks; Monday 23 Nov to Sunday 25 Apr | Yes |
| Phase lengths P1 to P7 | 3 + 6 + 3 + 2 + 6 + 1 + 1 = 22 weeks | Yes |
| Capacity for 22 weeks at 3, 3.5 and 4 h per person | 148.5 / 173.25 / 198.0 usable hours | Yes |
| Proposal window (16 days) and closure reserve (5 days) | 15.43–20.57 h and 4.82–6.43 h | Yes |
| Proposal window remaining from 9 Nov (14 days) | 13.5–18.0 h (15.75 h at the midpoint) | Yes |
| Package totals | 124–172 h, midpoint 148 h; headroom 25.25 h at 3.5 h and 0.5 h at 3 h | Yes |
| Phase allocation and package placement | Rows and columns reconcile; W5 split is 14 + 14 | Yes |
| P1–P4 at 3 h per person | 94.5 h against 108.6 h allocated, a shortfall of 14.1 h | Yes |
| Holiday-adjusted column | 23.625 + 39.375 + 23.625 + 15.75 + 47.25 + 0 + 7.875 = 157.5 h, which matches scenario B | Yes |
| Scenarios B and C | 20 or 18 weeks × 9, 10.5 or 12 h × 0.75 = 135 / 157.5 / 180 h and 121.5 / 141.75 / 162 h | Yes |
| Meeting cost | 11 × (0.75 + 1.5) = 24.75 h, which is 42.9% of the 57.75 h allowance | Yes |
| Weekdays | Sat 7 Nov, Mon 9 Nov, Fri 13 Nov, Mon 16 Nov, Wed 18 Nov, Fri 20 Nov, Sat 21 Nov, Sun 22 Nov, Sun 13 Dec, Sun 24 Jan, Sun 14 Feb, Sun 28 Feb, Wed 31 Mar, Sun 11 Apr, Sun 25 Apr, Fri 30 Apr. The "review by" Fridays are 11 Dec, 22 Jan, 12 Feb, 26 Feb, 9 Apr and 23 Apr | Yes |

### 1.2 The claimed P0 Gantt bar error

In a Mermaid Gantt chart, an explicit end date is **exclusive** by default. A chart must add the `inclusiveEndDates` keyword to change this ([Mermaid Gantt demos, v11.0.0](https://cdn.jsdelivr.net/gh/knsv/mermaid@11.0.0/demos/gantt.html), [S], accessed 9 Nov 2026). The original bar `2026-11-07, 2026-11-21` therefore draws 7 to 20 November. That contradicts both the table (7–22 Nov) and omm's own sentence that the chart "uses the following day as a bar's exclusive end so it includes the final day listed in the table". The finding is correct, and "low" is the right severity.

There is one caveat. The bar is labelled "Requirements and finalized design", and design approval (G2) is on 20 November. The original end date may therefore have been deliberate. The fix to `2026-11-23` is acceptable. A split into "design to 20 Nov" and "submission checks 21–22 Nov", which the report also offers, would match both the label and the table better. This is advice, not a required fix. The new holiday bars use the exclusive-end rule correctly (for example, `2027-04-13, 2027-04-16` draws 13–15 Apr).

### 1.3 External facts spot-checked (five checks)

| Fact claimed | Judge's check | Result |
|---|---|---|
| Makha Bucha falls on Sun 21 Feb 2027, with a substitute holiday on Mon 22 Feb | [OfficeHolidays Thailand 2027](https://www.officeholidays.com/countries/thailand/2027), accessed 9 Nov 2026 [S] | Correct |
| Chakri Day is Tue 6 Apr 2027 | Same source [S] | Correct |
| Songkran is Tue 13 to Thu 15 Apr 2027 | Same source [S] | Correct |
| Constitution Day is Thu 10 Dec 2026 (the worker marked this UNVERIFIED) | [OfficeHolidays Thailand 2026](https://www.officeholidays.com/countries/thailand/2026), accessed 9 Nov 2026 [S] | Correct; it could have been verified |
| jui's plan assumes "about 15-20 hours a week each"; jui's objectives O2, O5 and O6, and its drop order 10 → 9 → 5 → 6 | `docs/research/ai-assistant-jui/2026-11-08-agent-proposal-v2.md`, lines 99–103, 129 and 144 [V] | Correct |
| vj's notes say LINE cannot message another business's official account | `docs/review/source-vj/docs/specs/scope-and-objectives.md`, line 49 [V] | Quoted correctly; the worker rightly labels the underlying LINE claim UNVERIFIED |

**A holiday the worker missed.** Monday 7 Dec 2026 is a national holiday in P1. It is the substitute day for King Bhumibol's Birthday, which falls on Saturday 5 Dec (OfficeHolidays 2026 [S]). See defect 6.

### 1.4 Completeness against the assignment

| Assignment part | Covered? |
|---|---|
| (1) Arithmetic and calendar: capacity, the 148 h midpoint, phase hours, milestones, weekdays, Gantt | Yes, in full, with the report's own recomputations |
| (2) Consistency across the five files | Yes. Findings 19–25, plus a list of the items that were checked and found consistent. README.md and docs/README.md are flagged for the manager rather than revised, which is correct because they are outside the worker's output paths |
| (3) Method against good practice; too-heavy parts; missing requirements baseline, traceability, test plan and advisor sign-off | Yes. Report §4 covers this; the charter's §8 artefacts, the AD role and the advisor checkpoints apply it |
| (4) What must change because of the widened idea, without a redesign | Yes. Report §5 has a nine-row table, and only "pending combined scope" notes were added to the revised files |
| Revised copies with "Revision notes (9 Nov 2026)" | Yes, all three |

### 1.5 Structure, voice and dropped content

The diffs show that every changed line belongs to a change the worker meant to make. All sections, tables, the F01–F11 and D01–D10 IDs, the acceptance criteria and the operation boundary are kept. The tone and wording still read as omm's. The changes that were not listed in the revision notes are recorded as defects 4 and 5.

## 2. Defects

| # | File and section | What is wrong | Why it matters | Exact fix | Severity |
|---|---|---|---|---|---|
| 1 | Revised charter §8 "Lightweight engineering artefacts" and §11; revised timeline §7 (new dependency rows); revised kickoff A11–A13 | The revisions add proposal-window work: a requirements specification, a test plan, a traceability skeleton, a consent and ethics approach, advisor sign-off, the calendar task A11 and the merge task A13. Neither the lean estimate (15–21 h) nor the "Remaining from 9 November" paragraph is updated. The report's most urgent finding is that only 13.5–18.0 h remain | The revision makes the over-commitment it warns about worse, without saying so. A reader will think the added items are free | In timeline §4, under the lean estimate, add a short note. It should map the requirements specification to "Requirements and complete core flows" and the test plan and traceability skeleton to "AI feasibility, threat model, and verification strategy". It should state that A11, A12 and A13 are new work estimated at about 2–4 team-hours (label this an estimate). That raises the estimate to about 17–25 h against 13.5–18.0 h available. Repeat the one-line consequence in kickoff Discussion 6 | Major |
| 2 | Revised timeline §4, paragraph after the window table | "The 21–22 November steps fall on a weekend, so they are optional checks, not planned work." This also covers the 22 Nov submission, which is not optional | It misstates the one hard deadline | Reword: "The 21 November checks are optional weekend work. Submission on 22 November is mandatory unless it was done earlier; aim to submit on Friday 20 November if the portal allows." | Minor |
| 3 | Revised timeline §5, Songkran paragraph, and §6 phase table | The P6 holiday-adjusted capacity is shown as `0 (Songkran)`, but the text says Songkran leaves "about two working days" (Mon 12 and Fri 16 Apr) | The table and the text disagree | Add a footnote to the table: "Scenario B conservatively counts the whole Songkran week as zero; about 2 working days (Mon 12 and Fri 16 Apr) remain in practice." | Minor |
| 4 | Revised charter, revision notes | The Definition of Ready was narrowed, so permission/data impact and test approach are now needed only for sensitive tasks. The notes do not list this; note 9 mentions only the Definition of Done | The assignment requires every change to be listed | In note 9, add "Definition of Ready also tiered (permission/data impact and test approach only for sensitive tasks)" | Minor |
| 5 | Revised kickoff, revision notes, and Discussion 6 | "Sat 21 Nov (optional)" changes the status of a checkpoint, but note 5 says only "weekdays added" | An unlisted change | In note 5, add "21 Nov checks marked optional because they fall on a Saturday" | Minor |
| 6 | Report §2.4 holiday table; revised timeline §2 holiday table; revised charter §10 risk row | The table leaves out Mon 7 Dec 2026, the substitute day for King Bhumibol's Birthday, which falls in P1. It also marks 10 Dec as UNVERIFIED when the date can be checked | The holiday list is incomplete | Add the row "King Bhumibol's Birthday (substitute) – Mon 7 Dec 2026 – P1 – [S] OfficeHolidays Thailand 2026, accessed 9 Nov 2026" and mark 10 Dec [S] from the same source. Scenarios A–C do not change, because single holidays sit inside the 25% allowance | Minor |
| 7 | Revised charter header; revised timeline header | Both headers still say "Version: 0.1", but both history tables add a "0.2 (review copy)" row | The version is inconsistent within each file | Change each header to "Version: 0.2 (review copy, 9 Nov 2026) – based on omm's 0.1 of 7 November 2026" | Minor |
| 8 | Revised timeline §2 ("Treat scenario C as the planning case") and §6 (allocation still at 148 h and 3.5 h) | The file now holds two planning bases. Scenario C is declared the planning case, but the phase allocation still assumes scenario A. Report §2.4 also assumes first-semester finals in Nov–Dec and second-semester midterms in March. It does not say that the first-semester finals would fall in the proposal window, or that second-semester finals at many Thai universities can fall in late April or May, during P7–P8 handover (UNVERIFIED) | The reader cannot tell which basis to plan with. A likely exam clash with the proposal window and with handover is not flagged | Under the §6 phase table, add: "This allocation is scenario A. Rebaseline to the real calendar at A11 (13 Nov). Until then, check especially whether exams overlap the proposal window (now to 22 Nov) or P7–P8 (19–30 Apr)." | Minor |
| 9 | Report §2.4 scenario B | Scenario B removes the whole New Year week (28 Dec–3 Jan), but that week has only two public holidays (Thu 31 Dec and Fri 1 Jan). Students may also have more free time in a term break, not less. The assumption is not explained | The judge accepts it as a labelled estimate, but the reasoning should be visible | Add one sentence: "Assumes members are away over New Year; if members are free during the break, scenario B understates capacity." | Minor |

**Not counted as defects (noted for the manager):**

- The decision register in kickoff Discussion 4 still uses role names ("Technical lead", "AI lead") rather than codes. The worker's note 3 claims a mapping only for the agenda and the action register, so the claim is accurate.
- Updating README.md and docs/README.md is correctly left to the manager (findings 24–25).

## 3. Strengths confirmed

- Every figure in the report is reproducible. The report recomputes omm's figures rather than restating them.
- It finds a real inconsistency in the commitment basis: commitments are said to use 3 h a week, but the phases are planned at 3.5 h, leaving P1–P4 14.1 h short.
- It finds that Songkran falls in the only stabilization week. This is a real and important schedule risk.
- It adds the advisor (AD) role and sign-off points without removing the student sponsor's responsibilities.
- It lightens the process (tiered Definition of Done, folded risk review, light change path, "fill only rows that apply") without dropping omm's safeguards.
- It handles the widened idea well: impacts are listed, no redesign is made, and jui's 15–20 h a week assumption is caught and cited.

## 4. Required before integration

Fix defect 1 (major). Fixes 2–9 are short wording or table edits and can be made at the same time. No re-validation is needed after these edits; a spot-check of the changed lines is enough.

## Re-check (9 Nov 2026)

**Scope of this re-check:** only the parts that changed after the first verdict. These are in `docs/review/validation-omm-plan.md` and in the three revised copies in `docs/review/revised/omm/`.

| # | Defect | Status | Evidence |
|---|---|---|---|
| 1 (major) | Added proposal work not costed | **Fixed** | Timeline §2 (paragraph "Remaining from 9 November") and the new §4 paragraph "Added proposal work". The requirements specification is placed in "Requirements and complete core flows". The test plan and traceability skeleton are placed in "verification strategy". A11–A13 are costed at about 2–4 h, labelled as an estimate. Kickoff Discussion 6 and validation report §2.6 say the same. Check: 15 + 2 = 17 and 21 + 4 = 25, against 13.5–18.0 h available. The figures match in all three files. A deadline of 13 Nov to cut the package or agree extra hours is added |
| 2 | Submission called optional | **Fixed** | Timeline §4: "The 21 November checks are optional weekend work. Submission on 22 November is mandatory unless it was done earlier" |
| 3 | P6 "0" vs about two working days | **Fixed** | Footnote under the timeline §6 table: Mon 12 and Fri 16 Apr remain |
| 4 | Change to the Definition of Ready not listed | **Fixed** | Charter revision note 9 |
| 5 | "Sat 21 Nov (optional)" not listed | **Fixed** | Kickoff revision note 5, which also says the 22 Nov submission stays mandatory |
| 6 | 7 Dec missing; 10 Dec marked UNVERIFIED | **Fixed** | Both dates added with [S] OfficeHolidays tags in the validation report §2.4, the timeline holiday table and the charter calendar risk row. The weekdays are correct: 7 Dec is a Monday and 10 Dec a Thursday |
| 7 | Header still said version 0.1 | **Fixed** | The charter and timeline headers now read "0.2 (review copy, 9 Nov 2026) — based on omm's 0.1"; revision notes 15 (charter) and 11 (timeline) record this |
| 8 | Two planning bases; exam clashes not flagged | **Fixed** | A "Planning basis" note under timeline §6 states that the allocation is scenario A and must be rebaselined at A11 on 13 Nov. Possible exam clashes with the proposal window and with P7–P8 are flagged as UNVERIFIED, and the validation report §2.4 says the same |
| 9 | Scenario B assumption not explained | **Fixed** | The scenario B row in the timeline and the validation report §2.4 explain the assumption |
| Optional | Split the P0 Gantt bar | **Done correctly** | `p0, 2026-11-07, 2026-11-21` draws 7–20 Nov, which matches G2. `p0b, 2026-11-21, 2026-11-23` draws 21–22 Nov. Mermaid end dates are exclusive by default, so the two bars together cover the 7–22 Nov table. The milestone stays on 22 Nov. Timeline revision note 5 and validation finding 14 describe the split accurately |

**New errors:** none found. One redundancy is not counted as a defect. Timeline §4 now gives two similar "aim for Friday 20 November" sentences, one about being submission-ready and one about submitting. They do not contradict each other.

**Revision notes:** complete for every change checked above.

### Final verdict: PASS

No defects remain. The card is ready for integration.
