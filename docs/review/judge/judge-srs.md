# Judge verdict: SRS v0.9 and requirements register

| Field | Value |
|---|---|
| Card | srs |
| Files judged | `docs/requirements/SRS-v0.9.md` (1,772 lines); `docs/requirements/requirements-register.csv` (346 data rows) |
| Checked against | `docs/requirements/requirement-types-research.md` (RTR), `docs/combined/combined-proposal.md` v0.2 (CP), `docs/research/abdul-recon/abdul-recon-2026-11-09.md` §7 (via SRS Appendix C), `docs/review/TEAM-BRIEF.md` |
| Date | 9 November 2026 |
| Judge position | After the manager; independent |

## Verdict: REVISE (targeted)

Most of the SRS is very good. All 16 categories are present and have real content. The reader guide answers the user's question correctly. Every Abdul input is mapped. The legal facts match RTR. The Wilson intervals are correct. The register agrees with the SRS row for row.

It cannot pass yet, for three reasons:

1. **The Must scope has grown past the costed plan, and nothing shows it.** The user asked whether the SRS still fits about 134 hours. It does not show that it does. It adds roughly 30 Must requirements that are not lines in the CP Must packages. A rough estimate puts them at 25–40 hours `[I, judge estimate]`, against a margin of 7.75 hours.
2. **The lifecycle has two gaps that code would hit.** One flow (FR-FUP-05) needs a state change that FR-LCY-03 forbids. One requirement (FR-RP-03) sets two states at once, which the single-state rule does not allow.
3. **The register's priority column mixes priority with scope.** You cannot filter Must rows by package without reading free text.

These are bounded fixes, not a rewrite: one reconciliation table, a few changes of priority, a scope column, and four or five edits to rows. After they are made, the expected verdict is PASS.

---

## 1. Coverage

| Check | Result |
|---|---|
| All 16 RTR categories present with real content | **Pass.** The reader-guide table matches RTR §1.2 one to one. Every category has requirement rows (BR 9, STK 11, US 15, UC 15, IF 19, FR 79, AI 25, QR 23, DR 10, SEC 15, PRV 12, LEG 17, CON 16, UX/L10N/A11Y 17, TR 8, OPS 8, DOC 7, VER 10, ASM/DEP/OOS 30) |
| Reader guide answers "are FR and NFR enough?" | **Pass.** It says FR and NFR are the right top split but are not enough. It gives the right reasons: law, platform limits, team budget and uptime fail in different ways; AI needs its own test sets and pass marks; course deliverables are not features. This matches RTR §0 |
| CP Must items M1–M7 covered | **Pass with one mismatch.** Every content line of M1–M7 has a home (for example M1 secrets → SEC-01, logs → PRV-10, spike → QR-CO-01 and ASM-04; M5 isolation → SEC-12, fault injection → VER-06, restore → OPS-05, regression → VER-10; M6 → LEG-04, VER-03, BR-09, OPS-03, VER-08). **Mismatch:** CP M7 "design docs kept current" is Must, but `DOC-07` is S (defect D-09) |
| CP Should S0–S4 and Could C1–C6 covered | **Pass.** S0 → FR-RP, IF-RP, SEC-05..10, PRV-09; S1 → FR-SHR; S2 → FR-OPC, SEC-14; S3 → FR-REC; S4 → FR-UPC; C1–C6 → FR-PHO-01, FR-BILL-03, IF-ICS-01, FR-SPK-01, IF-LIFF-01, QR-MA-02. Small inconsistency: QR-MA-02 (model switch by config) is S, while its parent CP C6 is Could |
| CP E11 measures | **Partial.** BR-09 leaves out two E11 measures: "team minutes per handled request (S2)" and "repeat use within 30 days" (defect D-12) |
| Recon P-1..P-11 and D-1..D-8 mapped | **Pass.** Appendix C maps all 19 inputs (P-6 and P-7 share one row). Each has a priority, and Won't items are named |

## 2. Quality: sample of 40 requirements against the 29148 checklist

The sample covered every prefix: BR-01, BR-05, BR-08, STK-02, STK-03, US-01, US-11, IF-LINE-02, IF-LINE-06, IF-OAM-01, FR-CAP-01, FR-CAP-03, FR-ACT-03, FR-ACT-06, FR-REM-01, FR-REM-02, FR-EVD-02, FR-HND-01, FR-HND-03, FR-FUP-01, FR-FUP-06, FR-LCY-03, FR-LCY-06, FR-RTE-03, FR-RP-03, AI-HITL-01, AI-EVAL-03, AI-SAFE-02, AI-COST-01, QR-PE-01, QR-SE-01, QR-RE-04, DR-04, SEC-03, PRV-03, CON-TEAM-01, UX-03, L10N-02, A11Y-01, OPS-04.

Placeholders such as "TBD at freeze" and "TBD after P2 run" were not counted as failures. RTR §3 allows them at v0.9, and each has a date and an open issue.

**Failures: 9 of 40 (22.5%).** Seven are minor (wording or singularity). Two are major (lifecycle consistency).

| ID | Characteristic failed | Problem |
|---|---|---|
| FR-RP-03 | Consistent | "Set Request sent **and** Answered via reply page in one transaction". FR-LCY-01 allows exactly one state value, and the diagram goes HandoffOpened → AnsweredViaPage directly. `answered_via_page` is also an evidence level, so the meaning is unclear (D-02) |
| FR-FUP-05 (with FR-LCY-03) | Consistent, feasible | "Choosing another venue creates a new card version". The task is in RequestSent at that point, but the diagram allows only Expired → HandoffOpened. FR-LCY-03 would reject the transition (D-03) |
| FR-REM-01 | Unambiguous, consistent | The statement says every reminder is sent "within ±60 s". The criterion accepts ≥ 99%. "−60 s" also allows sending early |
| FR-REM-02 / QR-RE-04 | Singular, necessary | Each row has two "shall" clauses, and the two rows state the same D5 rule twice. A future change to one will make them disagree |
| IF-LINE-02 | Singular | Two obligations in one row: reply within the token window, and push only for listed cases |
| QR-SE-01 | Singular, unambiguous | Two "shall" clauses and a split priority "M (S0 parts: S)". The criterion allows "a logged first-open observation", which is a stored write on a GET, against "shall not change any stored state" |
| AI-SAFE-02 | Unambiguous | "Policy-violating content" does not say whose policy. Name the rubric (for example the five listed harm classes plus a written rating guide) |
| FR-RTE-03 | Singular | Records the attempt **and** updates a score whose formula is TBD. Split it, or move the score to E12 |
| A11Y-01 | Unambiguous (priority) | "M (M if built for S0, C5)". The privacy-notice page is Must, but the S0 and C5 pages are not. Split it into a Must row (privacy page) and an "M if built" note |

The worst offenders are FR-RP-03 and FR-FUP-05: a developer who follows FR-LCY-03 literally would build a system that breaks a Must flow. The rest are wording.

Positive points: every sampled row has a trace up, a measurable criterion and a method. The acceptance criteria are concrete (inputs, n, and an expected result). Duplicates were merged with their history kept (Appendix F).

Redundancy that is not a defect but should be noted: BR-02 = UX-04 and BR-08 = UX-05 are the same measure at two levels. That is acceptable if UX-04 and UX-05 are marked as derived from the BR rows.

## 3. Consistency

| Topic | Result |
|---|---|
| Retention vs CP §7.4 | **Consistent.** R1, R2, R4–R8, R13 and R15–R16 match CP §7.4 word for word in substance. R4 "whichever is later, capped by TR-03" is applied the same way in §1.1 fix 3, FR-EVD-04, TR-03 and E6. New rows R9–R11 are labelled as proposals (OI-25). R8 widens CP's "audit records of confirmed actions" to include consent and rights-request records. That is reasonable, but it should be listed in OI-25 too |
| AI-HITL-01 confirm-before-acting | **Consistent** with FR-ACT-02/03, BR-06, E3, LEG-02/06 and UC-06/07. One gap: AI-HITL-01 covers "shared item" (S1), but FR-SHR-01 has no confirmation step (D-10) |
| AI-EXP-01 disclosure | **Consistent.** Welcome (FR-ONB-01), notice (PRV-01), every card (FR-REQ-01, STK-05), reply page (FR-RP-01), LEG-08 |
| Quiet hours (D5) | **Consistent** across FR-REM-02, QR-RE-04, US-03, UC-04, UC-08, glossary and §1.3. Open points are properly logged in OI-03 (snooze repeat; outcome reminder before an appointment) |
| "Request sent" / "Handoff opened" | **Consistent** in the text (fix 1, fix 4, FR-HND-01/03, Table 3.2.9-A, UC-07, glossary). It is broken in the state model (FR-RP-03, see above) |
| Reply-page GET never changes state; no LINE login | **Consistent.** QR-SE-01, IF-RP-01/02, PRV-09, OOS-08, SEC-10. QR-SE-01 still has the "first-open observation" wording problem |
| Tester age 20+ or married | **Consistent** with CP §3.1, §3.2, §9.3, §10 (LEG-05, FR-ONB-03, TR-04, OOS-07). RTR's parental-consent route was dropped, as CP decided |
| ETA s.15 para 2(2), s.16, s.17, s.17/1 | **Consistent** with RTR §1.3.9 and open issue 9, including the "design basis, not a claim of coverage" caveat for s.17/1 |
| Other legal facts | **Consistent** with RTR: PDPA s.19, s.20, s.23, s.26, s.28, s.30, s.31, s.33, s.37(4) 72 h; CCA s.26 90 days (open); PDPC AI guideline advisory. **One misstatement:** PRV-03 and §3.6.4 say the PDPC AI guideline sets "90 days" for erasure and that we are "stricter". RTR says the 90 days applies only to data held *inside a model* (D-11) |
| Dates and weekdays | Correct (checked: 22 Nov 2026 Sun, 20 Nov Fri, 13 Dec Sun, 11 Dec Fri, 14 Dec 2026 Mon, 5 Jan 2027 Tue). **One inconsistency:** the G6 date appears as 26 Feb (ASM-06, DEP-05, the review day) and as 28 Feb (OI-14, the gate). Use "G6 28 Feb 2027 (review Fri 26 Feb)" everywhere |

## 4. Feasibility: is the Must set consistent with the 134-hour plan?

**Count.** The register has 232 active rows with priority exactly "M", plus 2 rows with mixed values (A11Y-01, QR-SE-01), for 234 in total. Appendix G's "234" counts both. Many of these are not build work: BR, STK, US, UC, LEG, CON, DOC and VER account for about 90 rows. The raw count is therefore not the problem.

**The problem.** The SRS gives no link from a Must row to a CP package line (M1–M7). Nowhere does it show that the build-relevant Musts add up to the CP §6.2 plan. Appendix A states "feasible (inside `CON-TEAM-02`)" as a check that was applied to every row, but no evidence supports it.

The following Must requirements are **not lines in the CP Must packages** (CP §6.1 contents and §6.2 hour lines). Some may fit inside an existing line; none is costed:

| Requirement(s) | Added work | Rough hours `[I, judge estimate]` |
|---|---|---:|
| AI-SAFE-02 (≥ 20 harmful items, two raters), AI-BEH-02 (10 out-of-scope items), AI-SAFE-03 (≥ 10 medical items) | Evaluation material grows from CP M3's 150 items to about 190. §3.3.3 still says "about 150 items (CP M3)", which is now wrong | 2–4 |
| AI-COST-01 | Daily per-user model-call limit | 1 |
| AI-EVAL-04 | A full held-out run after *every* prompt or schema change | 1–3 (recurring) |
| L10N-01 | Reply-language tracking plus a "fix language" command | 1–2 |
| IF-LINE-06, OPS-06 | Per-tester push cap; counter must agree with LINE's figure within 2% | 1–2 |
| IF-LINE-08 | Leave groups on `join` | 0.5 |
| QR-IC-01 | Rich menu with help | 0.5–1 |
| FR-LCY-06, QR-SA-03 | Uncertain state and conflict handling | 1–2 |
| FR-FUP-05 | Offer another venue after a second "No reply yet" | 1–2 |
| PRV-07 | Consent-withdrawal command (CP M5 names only export and delete) | 1 |
| OPS-04 | Incident procedure plus a tabletop drill | 1–2 |
| OPS-07 | Monthly cost report, P1–P8 (8 reports) | 1–2 |
| OPS-08 | Global pause switch plus a drill | 1–2 |
| A11Y-01, A11Y-03 | WCAG 2.2 AA audit with an automated checker and a screen reader for the privacy page | 1–2 |
| VER-07 | Contract tests (happy, error, timeout) for every external call | 2–3 |
| VER-08, §4.5 | Full black-box study over 3–7 days on both systems (CP M6 has "Abdul table 1–2 h") | 2–4 |
| DOC-03, DOC-05 | An ADR for every decision; a Thai and English user guide before G6 | 2–4 |
| DR-01, DR-05 | Full data dictionary; re-applying deletions after a restore | 2–3 |
| TR-02, TR-05 | Timed onboarding; closing message and job cancellation | 1–2 |
| QR-PE-01 | p95 first response ≤ 5 s (loading indicator, timing log) | 0.5–1 |
| **Total** | | **about 25–40 h** |

Even if half of this fits inside existing CP lines, the rest (about 12–20 h) is more than the 7.75-hour margin in scenario C at 3.5 h. **So yes, the SRS has grown the scope quietly.** The CP judge passed 134 h for a smaller requirement set. This is the main defect (D-01).

## 5. Register

- **CSV vs SRS agreement.** A script compared every ID in the register with the SRS tables. All 337 requirement IDs that appear in a table match on statement and priority. The 9 dressed use cases UC-01, UC-03..UC-10 are prose sections, so they were not compared. The only differences were the brief use cases (UC-02, UC-11..15), where the CSV joins the name and summary. That is expected. The 20-ID sample asked for is therefore exceeded: there were **0 mismatches** in statement or priority.
- **Priority column: it should be normalised.** It currently holds 18 distinct values ("M", "S (S0)", "S (S1)"…"C (C5)", "n/a (assumption)", "M (M if built for S0, C5)", "M (S0 parts: S)"). A filter on "M" quietly drops A11Y-01 and QR-SE-01. A filter on "S" misses 39 rows. Recommended columns:
  - `priority` ∈ {M, S, C, W, n/a}
  - `scope_item` ∈ {M1…M7, S0…S4, C1…C6, NEW, project}, where "NEW" marks a Must that is not in a CP package, which makes D-01 visible
  - `conditional` (for example "M if S0 built"; "promote to M if TD6 = B or better")
- **Status column.** It is also mixed ("Recorded (UNVERIFIED (exam dates not recorded))"). Split it into `status` and `evidence`.
- **Owner and trace-down.** Owner is "TBD" on all 346 rows (OI-33). `traces_down` is filled on only 96 rows. Both are acceptable at v0.9 because they are required by G3 (`VER-01`), but they need a named owner.

## 6. Verification section

| Check | Result |
|---|---|
| Decision rule | **Correct and clear.** Pass or fail on the point estimate; the 95% interval is always reported; "Inconclusive" is used only for E11's 25% rule (D2). It narrows CP §9's general "inconclusive" bullet; that deviation is logged as OI-05 and must be closed at G2 |
| Zero-tolerance and descriptive measures | Correctly separated. **Small conflict:** VER-05 asks for "one of the outcome labels" on *every* rate, but §4.4 says descriptive measures have no pass mark. Change it to "every rate that has a threshold" |
| Wilson numbers | **All 9 recomputed. All correct:** 45/50 → 78.6–95.7; 49/50 → 89.5–99.6; 20/20 → 83.9–100; 30/30 → 88.6–100; 990/1000 → 98.2–99.5; 16/20 → 58.4–91.9; 12/15 → 54.8–93.0; 14/20 → 48.1–85.5; 5/20 → 11.2–46.9 |
| SUS interval | "About ±8 points" is right at n = 20 (t × 17/√20 = 8.0). At n = 15 it is ±9.4. State "±8 to ±9.4" |
| E-measure → requirement mapping | **Complete.** E1–E13 each list their requirements. E14 is honestly "none in scope". BR-03 is deferred with OI-04 |
| Power limitation | Stated plainly (OI-06, DOC-06). Good |

---

## 7. Defects

| # | Section / ID | What | Why it matters | Exact fix | Severity |
|---|---|---|---|---|---|
| D-01 | §1.2, §2.5.2 `CON-TEAM-02`, Appendix A checklist, register | About 30 Must requirements (table in §4 of this verdict) are not lines in the CP M1–M7 packages and are not costed. The "feasible" check is claimed without evidence | The user's limit is ~134 h with a 7.75 h margin; the SRS may need 25–40 h more `[I]` | Add Appendix H "Must scope reconciliation": map every build-relevant Must to a CP §6.2 line, or to "NEW" with low–high hours. Show the new Must total against 141.75 h. Then demote "NEW" rows to Should, or to "v1.0", until the total fits. Suggested candidates: L10N-01 fix command, FR-FUP-05, OPS-07 (per phase instead of monthly), VER-08 (cut to the CP Abdul table, 1–2 h), DOC-03 (ADRs for D01–D10 only), AI-EVAL-04 (pilot releases only), VER-07 (happy and error only), A11Y-01 manual screen-reader check. Update §3.3.3 "about 150 items" to the real total. If anything stays Must, raise a CP v0.3 change | **Major** |
| D-02 | §3.2.11 `FR-RP-03`; Table 3.2.9-A; diagram | Sets two states at once, which conflicts with FR-LCY-01's single state value. `answered_via_page` is both a state and an evidence level | Ambiguous for build and for the E6 audit | Reword: "…the system shall record the sent-confirmed time, set the state to Answered via reply page and the evidence level to `answered_via_page`, in one transaction, and notify the user." In FR-HND-03 and the label table, say that a task in Answered via reply page counts as sent for E6 | **Major** |
| D-03 | §3.2.8 `FR-FUP-05`; §3.2.9 diagram; `FR-LCY-03` | The "choose another venue" path runs from RequestSent, but the diagram has no RequestSent → (new card) transition, so FR-LCY-03 rejects it | A Must flow (BR-04, UC-08 2e) would be blocked | Add the transition `RequestSent --> HandoffOpened: other venue, after a new Confirm` (or `RequestSent --> Captured: new card version`). Add a row to the FR-LCY-03 table-driven test | **Major** |
| D-04 | Register `priority`, `status` | Free-text mixed values; scope is mixed into priority | Filtering and counting are unreliable (A11Y-01 and QR-SE-01 drop out of "M") | Split into `priority` {M,S,C,W,n/a}, `scope_item`, `conditional`; split `status` into status and evidence. Regenerate Appendix G from the new columns | **Medium** |
| D-05 | `QR-SE-01` | Two "shall" clauses; mixed priority; "logged first-open observation" is a write on GET | Not singular; contradicts its own rule | Split it: QR-SE-01 (M): "An HTTP GET or HEAD to any URL the system serves shall not change task state, approvals, answers or evidence; access logging is allowed." Add a new row for "state changes only via verified postbacks or POST" (M), with the reply-page part covered by IF-RP-01 (S0) | Medium |
| D-06 | `FR-REM-02`, `QR-RE-04` | The same D5 rule appears twice, each with two "shall" clauses | Risk of drift; not singular | Keep QR-RE-04 as the master; reduce FR-REM-02 to "shall apply the quiet-hours rule of QR-RE-04 to every system-scheduled push", or merge it with a note in Appendix F | Minor |
| D-07 | `FR-REM-01` | The statement says all reminders within ±60 s; the criterion says ≥ 99%; early sending is allowed | Ambiguous pass rule | "…shall submit the reminder push no earlier than the stored time and no later than 60 s after it." Keep the E5 ≥ 99% criterion | Minor |
| D-08 | `IF-LINE-02`, `FR-RTE-03`, `A11Y-01`, `AI-SAFE-02` | Non-singular statements (IF-LINE-02, FR-RTE-03); mixed priority (A11Y-01); undefined "policy" (AI-SAFE-02) | 29148 singular and unambiguous | Split IF-LINE-02 and FR-RTE-03 into two rows each. Split A11Y-01 into M (privacy page) and "M if built" (S0, C5). In AI-SAFE-02, replace "policy-violating" with "content in any of the five listed harm classes, as judged by the written rating guide in the evaluation set" | Minor |
| D-09 | `DOC-07` | Priority S, but CP M7 "design docs kept current" is Must | Disagrees with the judge-passed CP | Make it M, or record the deviation with a reason in Appendix F | Minor |
| D-10 | `AI-HITL-01` vs `FR-SHR-01` | The master rule covers shared items, but the share flow has no confirmation step | An S1 build would breach the master rule | Add to FR-SHR-01: "…after the owner confirms a card naming the item and the invited person" | Minor |
| D-11 | `PRV-03` rationale; §3.6.4 erasure row | "PDPC AI guideline cites 90 days, we choose 30 (stricter)". Per RTR, the 90 days concerns erasure of data inside a model, not a general deadline | Misstates a legal source | "PDPA s.33 sets no fixed period in our reading; we commit to 30 days. (The PDPC AI guideline's 90 days concerns data inside a trained model, which we do not have.)" | Minor |
| D-12 | `BR-09` | Leaves out two CP E11 measures: team minutes per handled request (S2) and repeat use within 30 days | Coverage of a judge-passed measure | Add both to the BR-09 statement and criterion ("team minutes if S2 is built") | Minor |
| D-13 | `VER-05` vs §4.4 | VER-05 gives every rate an outcome label; §4.4 says descriptive measures have none | Self-contradiction | "Every rate that has a threshold shall carry an outcome label; every rate shall carry a 95% Wilson interval" | Minor |
| D-14 | §4.4 E10 row, `UX-06` | "About ±8 points" holds only at n = 20 | Small numeric overstatement | "About ±8 (n = 20) to ±9.4 (n = 15) points" | Minor |
| D-15 | `ASM-06`, `DEP-05` vs OI-14 | G6 is given as 26 Feb in some rows and 28 Feb in others | Date inconsistency | Use "G6 28 Feb 2027 (review Fri 26 Feb)" everywhere | Minor |
| D-16 | OI-28 | No Must way exists to cancel or delete a tracked obligation after Confirm. FR-REM-04 assumes such cancellation exists | Usability gap in the core Must flow; cheap to fix | Close OI-28 now: add FR-ACT-07 (M) "a Cancel task button on the Tracked message sets Discarded after a confirm tap", and add Scheduled → Discarded to the diagram | Minor |

**Counts:** 3 major, 2 medium, 11 minor.

## 8. What to keep unchanged

The 16-category structure and reader guide, Appendix C (recon mapping), Table R and its precedence rules, the LEG checklist, the §4.4 decision rules and power table, Appendix F (merge log with original text kept), and the open-issue list. These are at the quality the user asked for.

## Could not verify

- The hour figures in §4 of this verdict are the judge's own estimates `[I]`, not measured.
- Legal sources were checked against RTR (judge-passed), not reopened.
- Parts A, B and C in `docs/requirements/srs/` were not compared line by line with the integrated SRS. The integrated text was judged on its own.

---

## Re-check (9 November 2026, after integrator fixes J-01..J-16)

Scope: only the parts that changed (Appendix F.4, Appendix G, Appendix H, the lifecycle, the register, and the rows that changed).

### Appendix H hours: recomputed, all correct

- **NEW Must sum.** The low estimates are 0.25 + 0.25 + 0.5 + 0.5 + 0.5 + 0.5 + 0.25 + 0 + 0.25 = **3.0 h**. The high estimates are 0.5 + 0.5 + 0.5 + 1 + 1 + 1 + 0.5 + 0 + 0.25 = **5.25 h**. The midpoint is **4.125 h**.
- **Reconciled Must.** 134 + 4.125 = **138.125 h** (range 114–162.25 h).
- **Scenario table at 3.5 h a week:**
  - A: 173.25 − 138.125 = +35.125 h
  - B: 157.5 − 138.125 = +19.375 h
  - **C: 141.75 − 138.125 = +3.625 h**
- **With S0 added (148.625 h):** A +24.625 h, B +8.875 h, C −6.875 h.
- **At 3.0 h a week:** A +10.375 h, B −3.125 h, C −16.625 h. The low estimate in C gives 121.5 − 114 = +7.5 h.
- **TD5 option B (Free plan).** It adds 0.5–1 h, which gives a margin of about +2.875 h.

All of these figures match the SRS. `CON-TEAM-02` now cites the same numbers.

### Every active Must is mapped

The register has 234 active Must rows. Each one has a `scope_item`, and every row in M1–M7 also has a `cp_plan_line`. The split is:

| Scope item | Must rows |
|---|---:|
| M1 | 29 |
| M2 | 43 |
| M3 | 17 |
| M4 | 43 |
| M5 | 24 |
| M6 | 10 |
| M7 | 4 |
| NEW | 8 |
| project | 56 |

This matches Appendix G. No row is unmapped.

The valid values are respected:

| Column | Values found |
|---|---|
| `priority` | {M, S, C, W, n/a} only |
| `scope_item` | {M1–M7, S0–S4, C1–C6, NEW, project} only |
| `status` | {Proposed, Recorded, Deleted, Deferred} |

Evidence notes now sit in their own `evidence` column.

### The 56 "project" Must rows

I checked each one. They are genuinely outside the build-and-evaluation hours, or they are covered elsewhere:

- **BR and STK rows** are met by the rows they trace to.
- **CON rows** are limits, not work. The forecasting work falls inside the CP's 25% coordination allowance.
- **LEG rows** are met by their "Satisfied by" rows, plus a checklist review inside the M6 ethics line.
- **Proposal-window work in P0:** `DOC-01`, `DOC-04`, the D01–D10 ADRs, and the D06 records behind `PRV-02`, `PRV-05`, `AI-SUP-02` and `CON-TECH-02`.
- **Gate rules:** `VER-01`, `VER-04`, `TR-08`.

One caveat carries over: CP §6.3 already shows that the P0 window is short (13.5–18 h available against 17–25 h needed). This is a known CP risk (TD6), not something the SRS revision introduced.

### Lifecycle

The diagram, Table 3.2.9-A, `FR-LCY-01`/`03`, `FR-RP-03`, `FR-HND-03`, `FR-FUP-05` and `FR-ACT-07` now agree:

- `FR-RP-03` sets one state (Answered via reply page) and the evidence level `answered_via_page`. That state counts as sent for E6.
- RequestSent → HandoffOpened (another venue, after a new Confirm) is in the diagram, and the FR-LCY-03 test names it.
- Scheduled → Discarded (a confirmed Cancel task) is in the diagram, the Discarded row of the table, and `FR-ACT-07`.

Every transition that a requirement names appears in the diagram.

### Spot-check of changed rows against the 29148 checklist

I checked 16 rows: `FR-RP-03`, `FR-HND-03`, `FR-FUP-05`, `FR-ACT-07`, `FR-REM-01`, `FR-REM-02`, `QR-SE-01`, `QR-SE-02`, `IF-LINE-02`, `IF-LINE-09`, `FR-RTE-03`, `FR-RTE-05`, `AI-SAFE-02`, `A11Y-04`, `FR-SHR-01`, `VER-05`. I also read `PRV-03`, `BR-09`, `L10N-08` and `CON-TEAM-02`.

None fails. Earlier defects D-01..D-16 are all closed.

### Residual notes (minor, not blocking; fix when convenient)

1. **`IF-LINE-09` push list is incomplete.** It allows push only for "reminders, follow-ups, expiry notices and asynchronous results". The `TR-05` closing message and the nudges and digests named in `QR-RE-04` are not on that list. Fix: add "the end-of-pilot closing message and the system-scheduled pushes listed in `QR-RE-04`".
2. **Rich menu is still Must work.** `QR-IC-01` was demoted to S, but a rich menu stays Must through `LEG-01` (privacy link in the rich menu) and the §3.1.1 "rich menu (Help; My tasks)" row. "My tasks" has no FR. Fix: either drop "My tasks" from §3.1.1, or map the rich menu to the M2 line. The saving from demoting `QR-IC-01` is close to zero either way.
3. **`AI-COST-03` is labelled two ways.** H.3 lists it as NEW, but the register and H.1 put it in M1. This is conservative (its 0.25 h is counted), but the label should be one or the other.
4. **`FR-REM-01` statement vs criterion.** The statement is absolute ("no later than 60 s"), while the criterion accepts ≥ 99%. This is acceptable as a sampling acceptance rule; saying so in the criterion would remove the doubt.
5. **Absorbed work may be optimistic.** Several rows are absorbed into existing CP lines without extra hours (for example M2 "capture, card, in-chat edit, cancel" now carries 23 rows, and M1 "deploy" carries `OPS-06` and `QR-PE-01`). With a margin of only +3.625 h, the P1 actual-hours check and the two-weekly forecast (OI-34, CP §6.5 cut order) are essential. The SRS states this honestly.

### Final verdict: **PASS**

The SRS v0.9 and the register can go forward to G1/G2. The remaining notes are minor.
