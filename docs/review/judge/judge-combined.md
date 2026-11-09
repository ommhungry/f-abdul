# Judge verdict: combined card (9 November 2026)

- Files judged: `docs/combined/advantages-matrix.md` (103 lines), `docs/combined/combined-proposal.md` (551 lines).
- Inputs checked against: `docs/review/TEAM-BRIEF.md`; `docs/review/revised/omm/` (brief §10.1, charter §3, timeline v0.2 §2, §6, §9); `docs/review/revised/vj/specs/scope-and-objectives.md`; `docs/research/ai-assistant-jui/2026-11-08-agent-proposal-v2.md` (§6, §13, §15); `docs/requirements/requirement-types-research.md` (LEG, DR, PRV, AI-HITL, CON-TEAM); `docs/research/abdul-recon/abdul-recon-2026-11-09.md` (§6 gap analysis, §7 P-x/D-x); the five earlier judge verdicts.
- Method: every hour figure recomputed by hand; IDs and rules compared line by line with the requirements file; member strengths spot-checked in the members' own files. No web sources were reopened; external `[V]` tags were taken from the earlier validators and judges.

## Verdict: REVISE (targeted)

The integration itself is good work. The matrix is faithful, the 11 conflicts are resolved with sound reasons, the arithmetic is correct to the decimal, and the architecture and flows are coherent. It is not ready for the 13 November decisions because of six major defects. Two of them change what the team would decide: the tester age rule (TD1) contradicts `LEG-05`, and the "Must is 10 h lighter than omm's plan" claim, which underpins TD6, rests on under-costed packages. No rewrite is needed. All fixes are local.

## 1. Faithfulness and completeness

**Passes, with small gaps.**

- omm: O1–O14 match the revised brief and timeline. The photo-capture conflict (brief §10.1 Must vs charter F02 "conditional") is real and correctly resolved (C7). One misplacement: omm's brief §10.1 lists **voice-note input as Could**; the proposal puts it in Won't without listing it as a conflict (defect m6).
- vj: V1–V10 match `scope-and-objectives.md` (router, venue table, Wizard-of-Oz, anti-goals, adapter, ToS discipline). Missing vj metrics: **team time per successful booking**, **share of requests needing a phone call**, and **repeat use within 30 days** (vj scope "Success metrics"). Team time per booking is the measure that keeps a Wizard-of-Oz result honest (defect m7).
- jui: J1–J14 match v2 §6.1, §8, §13–§15. The corrections (3–4 h, 54M MAU, ETA s.15 para 2(2) and s.17) are applied correctly in both files. jui's cost KPI "LINE pushes per active user per month ≤ 40" is dropped without comment, although push quota is a live risk (TD5) (folded into m7).
- Abdul recon: used. §6 gap analysis drives proposal §1–§2, and the matrix cites P-1/P-2. But parity is overstated: recon P-2 requires one-off, daily, weekly, monthly and วันพระ recurrence, while M2 builds only one-off and monthly and the cut order removes monthly. P-5 (escalation until acknowledged) and P-11 (per-user metering) are not mentioned. (defect m5)
- Conflicts C1–C11: all sound except C1, whose "18+" restriction is wrong (M1 below).
- Internal inconsistency: matrix §1 last row says to use the requirements file "categories only, not its ETA section numbers (that file is still under review)". That file has a final PASS (`judge-requirement-types.md` line 173), and the proposal itself relies on its `AI-HITL-01`, `LEG-06`, `LEG-07` and `CON-TEAM-01` as judge-passed (defect m1).

## 2. Hours check

**Arithmetic: correct. Credibility of the "lighter" claim: not established.**

Recomputed:

| Item | File says | Recomputed |
|---|---|---|
| Must low / high / mid | 116 / 160 / 138 | 18+22+12+20+18+14+12 = 116; 24+30+16+28+26+20+16 = 160; 21+26+14+24+22+17+14 = 138 ✔ |
| Must + Should | 134 / 188 / 161 | ✔ |
| Scenario table (A/B/C at 3.0, 3.5, 4.0) | +10.5, +35.25, +12.25, +37.0; −3.0, +19.5, −3.5, +19.0; −16.5, +3.75, −19.25, +1.0 | All ✔ against timeline v0.2 §2 (148.5/173.25/198.0; 135.0/157.5/180.0; 121.5/141.75/162.0) |
| M1–M5 vs `CON-TEAM-01` | 90–124, mid 107 | ✔ |
| Phase placement | 138 total; P1–P4 = 100 vs 94.5 / 110.25; P5 spare 15–22 | ✔ (40.5−25 = 15.5; 47.25−25 = 22.25) |

Where it is under-costed (all estimates, judge's):

1. **M4 (20–28 h) carries more than any omm package of the same size.** It contains a card composer with a LIFF edit form, content-hash approvals, a *public* web page with tokens, rate limits, Stop and "withdrawn" states, a 10-minute Withdraw, the handoff and copy adapters, router scoring, follow-up and expiry jobs, outcome-to-reminder, and **hand verification of 20–40 venues**. Venue verification alone (finding and checking a LINE ID or phone and opening hours, about 10–20 minutes each) is roughly 4–13 h of field work that is not visible anywhere. A bottom-up split gives about 25–40 h.
2. **M6 grew only 12–18 → 14–20 h while the sample grew 5–8 → 15–30 testers**, plus 20–40 real cards, 6–8 moderated sessions, recruiting 5–10 friendly shops, an ethics/consent pack (`LEG-04`), and support during the pilot (`OPS-03`, answer within 2 working days). Moderated sessions with a facilitator and note-taker cost about 2–2.5 person-hours each, so 6–8 sessions alone are about 12–20 h. A realistic range is about 22–35 h.
3. **M1 and M5 were cut below omm's W1 and W5** although each gained scope: M1 now hosts the reply page and LIFF token checks; M5 adds reply-page abuse tests and Withdraw tests. "Fewer permission surfaces" offsets part of that, not all.

With mid-range corrections (M4 about +8 h, M6 about +10 to +15 h), the Must midpoint becomes about **155–160 h**. That is about equal to or above omm's 148 h, not 10 h below it. It then **does not fit scenario C at 3.5 h (141.75)** and only just fits B at 3.5 h (157.5). So the summary line "The Must list fits at the 3.5-hour planning midpoint in every scenario" and TD6's reason "lighter than omm's plan in build hours" are not supported. The conclusion may still hold if the team accepts smaller numbers (for example 10–15 venues, 4 moderated sessions), but then those must be the planned numbers, not cut-order items (defect M2).

Also note: recruitment, consent and venue verification must happen **before P5**, in P1–P4, which are already 5.5 h short at 3.0 h.

## 3. Consistency with `requirement-types-research.md`

- **TD1 age rule: confirmed defect (manager's concern is correct).** Matrix C1 says "restricted to adults (18+)"; proposal §3.1 says "Adult (18+)"; §9.3 says "adults only". `LEG-05` says testers must be **20 or over** (or married, sui juris) unless written parental consent is also obtained, because Thai majority is 20 (CCC s.19) and PDPA s.20(1) then needs parental consent; it recommends recruiting 20+. Under Thai law "adult" means 20+, so "18+" is wrong and the recruitment pool claim must shrink (most first- and second-year students are under 20). (M1)
- **Retention: missing.** The proposal never states retention. `DR-04` (chat text ≤ 30 days; audit ≤ 180 days or until report), `PRV-03` (export/delete within 30 days), `PRV-06`, `TR-03` (delete pilot data within 30 days of the end) and open issue 16 (CRCA s.26 90-day traffic data) all apply. The combined design adds new personal data the requirements file did not foresee: payment slip images and screenshots of the shop's chat (Evidence), business reply free text, and the user's display name shown on a public page. None has a retention rule. (M3)
- **Requirement IDs collide.** Proposal §5 says IDs "follow" the requirements file, but reuses its IDs with different meanings: `SEC-01` (file: encrypted secrets; proposal: webhook signature), `LEG-01` (file: published privacy policy; proposal: no personal-account automation / no Places copying), `DR-01` (file: data dictionary; proposal: claims stored apart), `FR-01` etc. The file's rule is "number, never reused". The SRS author will copy these. (M4)
- **`AI-HITL-01`: consistent.** §4.3 and §7.2 implement it in code, with content hash, version, expiry and single use. It also covers QR codes (C2). One gap: `AI-EXP-01` requires AI disclosure in the welcome message and privacy notice too, not only on cards (m8).
- **`LEG-07` Withdraw: present (FR seed, M4, §10), but its meaning for the handoff path is unstated.** With `oaMessage` the text is sent by the user from their own LINE; our system cannot retract it. Withdraw can only mark the reply page "withdrawn by sender". `LEG-07` requires telling the user what could not be undone (for example: "unsend the chat message yourself"). The state diagram has no Withdrawn state (m3, m4).
- **`CON-TEAM-01`: used correctly** (M1–M5 = 90–124 h within ~120–160), subject to the under-costing in §2.

## 4. Technical soundness

- **Architecture (Mermaid §7.1):** coherent. The approval gate is the only path to adapters; the AI has no acting tools; the job table and outbox follow omm §8.6. Good.
- **"Request sent" is not observable — major.** `oaMessage` only opens the shop's chat with pre-filled text. Our server never learns whether the user tapped Send, and the user can edit the text before sending. Yet the evidence table (§4.2), the state diagram (`Confirmed --> RequestSent: user tapped Send`) and the sequence §8.2 treat it as a known event. For a product whose central claim is honest status (G3, E6), this is a status we cannot know. (M5)
- **Request card and reply page flows (§8.2, §8.3):** sound in shape; the follow-up for chat answers is correctly Must.
- **Reply page security: incomplete — major.** Good: signed, card-bound, expiring tokens; rate limits; free text as data; Stop. Missing:
  - **Link previews.** LINE fetches URLs posted in chats to build previews. If an answer can be triggered by a GET, a crawler or a preview can "Accept" or consume a single-use token. Answers must be POST only, after a page load, with a confirm step.
  - Concrete expiry (for example the end of the last offered time slot, capped at 7 days) and what the page shows after it.
  - Token leakage: `Referrer-Policy: no-referrer`, no third-party scripts or analytics, `noindex`, tokens never logged.
  - Minimal personal data on the page (first name or nickname only; no LINE ID).
  - Anyone holding the link (including the user) can answer or press Stop; the file admits identity is not proven, but E11 needs a rule (for example flag answers made from the requester's own LIFF session). (M6)
- **LINE limits:** correctly stated. No OA-to-OA messaging (`[I]`, from validation-vj #1, honestly tagged), one OA per group, `oaMessage` mobile only, Free plan 300 pushes, 840 pushes for 30 testers × 28 days (✔), Basic plan about ฿2,740 for two months (1,280 × 1.07 × 2 = 2,739.2 ✔). TD5 option B (10 pushes per tester per month) is very tight for reminders plus follow-ups plus outcome messages; say so, or count pushes per flow (m7).
- **State diagram (§8.4):** missing Withdrawn and Expired states, though both are Must behaviours (m3).
- **Evaluation plan:** sized correctly to 15–30 testers and 20–40 cards; Wilson intervals required; E1 ±5 points at n≈150 is correct (1.96 × √(0.09/150) ≈ 0.048). Two weak points: E8's "≥ 80% unaided" rests on 6–8 moderated sessions, so its interval is about ±25 points; and E11's 25% decision rule at n = 20 has an interval of roughly 11–47%. State that both are applied to the point estimate and reported with the interval, and that E11 is inconclusive if the interval straddles 25% (m9). SUS 68 "UNVERIFIED" is fine as tagged.

## 5. Legal and boundary

- **No payments held:** ✔ (§10 Money row, C2 only shows a QR to the payer's own account; matches `LEG-03`).
- **PDPA cross-border for the LLM:** mentioned, but not to the level of the requirements file. `PRV-02` requires recording the s.28 basis (UNVERIFIED which applies) and `P-10` (recon) asks for zero-retention, no-training routing. Add both as inputs to D06 (m2).
- **Ethics and consent:** reasonable (§9.3), but it treats ethics review as "whether needed"; `LEG-04` makes obtaining required approvals a precondition of the pilot. Businesses receiving cards are third-party data subjects who never consented; the ethics application should cover them (folded into m2).
- **Google Places:** mostly correct (place IDs only; no Places content in test sets; nothing from Wongnai). Two gaps: the Venue entity stores **opening hours** with no source rule, and has **no venue name field**, while cards say "ร้าน X". State that name and hours are our own verified facts, or that Places details are fetched at display time with Google attribution (vj scope rule). Also note that obtaining a place ID requires a Places API call under the Maps terms (m10).
- **Abdul boundary:** respected (E13 public information only).

## 6. Defects

Severity: **blocker** (cannot go to the team), **major** (must fix before 13 Nov decisions), **minor** (fix before the proposal is submitted).

| ID | File / section | What | Why | Exact fix | Severity |
|---|---|---|---|---|---|
| M1 | matrix §3 C1; proposal §3.1, §3.2, §9.3, §13 TD1 | Testers "adults (18+)" | Contradicts judge-passed `LEG-05`: majority is 20 in Thailand; 18–19 need written parental consent | Replace "18+" with "aged 20 or over (or married, sui juris); no under-20 testers unless parental consent is also obtained, which we do not plan". In TD1's reasoning, note that this removes most first- and second-year undergraduates from the pool, so staff, later-year and graduate students are the realistic pool (this also strengthens option C) | major |
| M2 | proposal §6.2, §6.3 bullets 1 and 5, §6.4, §13 TD6; matrix C5 | Must hours under-costed; "~10 h lighter than omm" not credible | M4 omits venue verification and is dense; M6 grew 2 h for a 3–4× larger sample plus a field test; M1/M5 shrank despite new scope. Realistic midpoint about 155–160 h | Either (a) re-estimate M4 bottom-up (add a "verify venues" line, about 4–13 h) and M6 (about 22–35 h), recompute the scenario table, and change the claim to "about equal to omm's plan"; or (b) keep 138 h by making the smaller numbers the plan (10–15 venues, 4 moderated sessions, 15–20 testers) and say so. Place recruitment, consent and venue verification in P1–P4 explicitly. Reword TD6's reason accordingly | major |
| M3 | proposal §5, §7.3, §7.4, §10 Personal data row | No retention rules | `DR-01`, `DR-04`, `PRV-03`, `PRV-06`, `TR-03` require them; new data types (slip images, chat screenshots, business reply text, name on a public page) are not covered | Add a retention column to §7.3 or a short table: chat text 30 days (`DR-04`); slip and screenshot images deleted when the task closes or 30 days, whichever is earlier (proposal); business reply text and reply-page data 30 days after the card closes; audit 180 days or until report (`DR-04`); all pilot data within 30 days of the end (`TR-03`); note CRCA s.26 open issue 16 | major |
| M4 | proposal §5 | Requirement IDs reuse the file's IDs with different meanings (`SEC-01`, `LEG-01`, `DR-01`, `FR-01`…) | Breaks traceability; the file says numbers are never reused | Cite the file's existing IDs where the seed matches (`AI-HITL-01`, `FR-ACT-01`, `LEG-03`, `LEG-05`, `LEG-07`, `PRV-01`, `PRV-02`, `DR-04`, `CON-TEAM-01`), and give new seeds clearly provisional IDs (for example `FR-REQ-01…`, `SEC-WH-01`) or label the column "seed (not an SRS ID)" | major |
| M5 | proposal §4.2 table row "Request sent", §8.2, §8.4; matrix C4 | "User tapped Send in the shop's chat" treated as a known event | The server cannot observe it; the user may also edit or not send the text. Status honesty (G3, E6) is the core claim | Rename the state "Handoff opened (not yet confirmed sent)". Move to "Request sent" only when the user answers a "Did you send it?" quick reply, or when the reply page is first opened. Add this to the follow-up flow §8.3 and to E6 | major |
| M6 | proposal §5 SEC-02, §7.1 Reply page, §7.4, §9.2 | Reply-page security underspecified | LINE link previews can trigger GET answers or consume single-use tokens; token leakage; personal data on a public page | Add: answers by POST only after page load and a confirm tap; GET is side-effect free; explicit expiry (end of last time option, max 7 days); `Referrer-Policy: no-referrer`, `noindex`, no third-party scripts, tokens never logged; page shows only the user's nickname; flag answers made from the requester's own LIFF session; test "link preview does not change state" in §9.2 | major |
| m1 | matrix §1 last row | Says the requirements file is "still under review" and to ignore its ETA sections | The file has a final PASS; the proposal already relies on its IDs | Replace with "judge-passed (final PASS); use its IDs and ETA references" | minor |
| m2 | proposal §10 Personal data row, §9.3; §7.1 AI interpreter | Cross-border and ethics detail thin | `PRV-02` (record the s.28 basis), recon P-10 (zero-retention, no-training), `LEG-04` (approval is a precondition) | Add `PRV-02` and "zero-retention, no-training terms" as D06 criteria; change §9.3 to "obtain the approvals the university requires before any pilot (`LEG-04`); include businesses receiving cards in the application" | minor |
| m3 | proposal §8.4 | No Withdrawn or Expired states | Both are Must (M4) | Add `RequestSent --> Withdrawn: within 10 min, unanswered` and `RequestSent --> Expired: expiry job` | minor |
| m4 | proposal §10 Input errors row, FR seed `LEG-07` | Withdraw on the handoff path not explained | `LEG-07` requires telling the user what could not be undone | Add: "For a handoff, Withdraw marks the card withdrawn; the chat text was sent by the user and can only be unsent by the user in LINE; the assistant says so" | minor |
| m5 | matrix O8; proposal §6.1 M2, §6.5 item 3, §9.1 E13 | Abdul parity overstated | Recon P-2 lists daily, weekly, monthly and วันพระ recurrence; P-5 escalation; P-11 metering | Either add daily and weekly (cheap once monthly exists) or state in E13 and limits that recurrence parity is partial; mention P-5 as future work and P-11 under TD5 | minor |
| m6 | proposal §6.1 Won't; matrix | Voice-note input moved from omm's Could to Won't silently | Faithfulness | Add as conflict row or matrix note with the reason (hours, speech-to-text accuracy) | minor |
| m7 | proposal §9.1 E11/E12, §13 TD5 | vj metrics and jui push KPI dropped | Team time per booking keeps Wizard-of-Oz honest; push budget is a live risk | Add "team minutes per handled request (S2)" and "share needing a phone call" to E11; add "pushes per tester per month" to E5 or TD5; note that option B's 10 pushes per tester is tight | minor |
| m8 | proposal §10 AI disclosure row | Disclosure only on cards | `AI-EXP-01` also requires it in the welcome message and privacy notice | Add both | minor |
| m9 | proposal §9.1 E8, E11 | Decision rules on tiny samples | Interval about ±25 points (E8, n = 6–8); about 11–47% at n = 20 (E11) | State rule applies to the point estimate, report the interval, and call E11 inconclusive if the interval contains 25% | minor |
| m10 | proposal §7.3 Venue, §10 Venue data row | Venue name and opening hours have no source rule | Maps ToS s.3.2.3 bars caching Places content | Add "name and hours: our own verified facts; Places details, if shown, fetched at display time with Google attribution; obtaining a place ID uses the Places API under its terms" | minor |

No blockers.

## 7. What was not verified

- External sources (LINE docs, Maps ToS, ETA, PDPA, LINE plan prices) were not reopened; their `[V]` tags come from earlier validators and judges.
- Whether LINE's link-preview crawler fetches arbitrary URLs in OA chats in the way described in M6 is from general platform behaviour (UNVERIFIED for this case); the fix is cheap either way.
- Hour re-estimates in §2 and M2 are the judge's estimates.

---

## Re-check (9 Nov 2026)

Scope: `docs/combined/combined-proposal.md` v0.2 (686 lines) and `docs/combined/advantages-matrix.md` v0.2 (106 lines), against the defects above and the requirements file. I recomputed every hour figure by hand and checked every cited requirement ID against `requirement-types-research.md`.

### Final verdict: PASS

All 6 major and 10 minor defects are fixed. The new hours are honest and correct to the decimal. The scope cuts are named, and every requirement ID now matches the file. Five small items remain (R1–R5 below). None blocks the 13 November decisions; fix them before the proposal is submitted.

### Defect status

| ID | Status | Evidence |
|---|---|---|
| M1 age | Fixed | §3.1, §3.2, §9.3, §10, TD1, matrix C1 and J5: "20 or over, or married (sui juris)", with the effect on the tester pool stated. No "18+" left in either file |
| M2 hours | Fixed | §6.2 is costed line by line in three steps (v0.1 scope 173 h → evaluation shrunk 158.5 h → plan 134 h). The "10 h lighter" claim is withdrawn, and §6.2 now says the plan is smaller "only because it contains less". Recruitment, consent and venue checking are placed in P1–P4 |
| M3 retention | Fixed | §7.4 covers every data type, including slips, screenshots, reply-page data, the nickname on the page, the venue table, and CRCA s.26 open issue 16 |
| M4 IDs | Fixed | §5 uses the file's own IDs and gives new items `SEED-` IDs. All 47 non-SEED IDs cited exist in the requirements file, and their meanings match (spot-checked STK-03, FR-REM-01, UX-01, PRV-04, VER-02/03, AI-PI-02, TR-01, OPS-01–03, L10N-02, AI-BEH-01) |
| M5 Handoff opened | Fixed | §4.2, §8.3, §8.4, `SEED-FR-05`, E6. One inconsistency remains in the S0 path (R1) |
| M6 reply-page security | Fixed | §7.5 now has: answers by POST only, page loads change nothing, tokens of at least 128 bits stored hashed, expiry at the last offered slot (capped at 7 days), no-referrer, noindex, a strict script policy, tokens redacted from logs, nickname only, rate limits, and a test that a link preview changes nothing (§9.2). One mechanism is questionable (R2) |
| m1–m10 | Fixed | m1 matrix corrections row; m2 §7.1 D06 criteria and §9.3; m3 Withdrawn and Expired states in §8.4; m4 §7.2 and §10; m5 E13 and §12; m6 matrix C12; m7 E5, E11 and TD5; m8 §10 and `AI-EXP-01`; m9 §9 rules and E8/E11; m10 §7.3 Venue and §10 |

### Hours recomputed

- **Step 1 (v0.1 scope at the v0.1 evaluation size).** Every package's line items add up to its stated range. Package totals are 141 / 205 / 173 h. "Fits only A at 3.5 h" is correct (+0.25 h).
- **Step 2 (evaluation shrunk only).** M4 drops to 21–32 h and M6 to 19–28 h, giving 132 / 185 / 158.5 h. That is 16.75 h over scenario C at 3.5 h, as stated.
- **Step 3 (the plan).** Every line sums correctly: M1 20–26, M2 16–21, M3 10–15, M4 15–24, M5 19–27, M6 19–28, M7 12–16, total **111 / 157 / 134 h**.
  - The named moves net to 24.5 h, which equals 158.5 − 134.
  - Each package's change between step 2 and the plan matches its moves (M1 −3, M2 −7, M3 −2.5, M4 −7, M5 −5, M6 0).
  - Should items total 31 / 47 / 39 h; Could items total 28 / 42 / 35 h.
  - `CON-TEAM-01` (M1–M5 build and test): 80–113 h, midpoint 96.5 h; 89–125 h with S0.
- **Scenario table.** All 18 cells are correct (for example C at 3.5 h: +7.75 for Must, −2.75 for Must + S0, −31.25 for Must + all Should). "Low Must fits C at 3.0 h with +10.5 h" is correct. The 7.75 h margin is 5.5% of capacity; the file rounds this to "about 5%", which is acceptable.
- **Phase placement (scenario B at 3.5 h).**
  - Phase capacities are 23.625 / 39.375 / 23.625 / 15.75 / 47.25 / 0 / 7.875, totalling 157.5 h.
  - Placed hours are 22 / 37 / 21 / 15 / 31.5 / 0 / 7.5, totalling 134 h. Each package reconciles across phases (M1 23, M5 23, M6 23.5, M7 14).
  - P1–P4 need 95 h against 102.4 h of capacity; P5 has 15.75 h spare.
  - The table is shown for scenario B, not C, because exam dates are unknown. The file says so. If both exam fortnights fall in P1–P4, the 7.4 h margin there is not enough (2 × 7.875 = 15.75 h); TD4's 1–14 March overflow window and §6.5 then apply. This is stated, so it is acceptable.

### Do the scope moves weaken the core differentiator?

The moves are named honestly, but **in scenario C they weaken the "acts" story noticeably.** The team should hear this plainly before choosing TD6.

- **What remains in Must** still goes beyond Abdul. The assistant interprets a request, drafts a Thai card with AI disclosure, routes it to a hand-checked shop LINE ID, opens the shop's chat for a one-tap send, and keeps honest states (opened, sent, answered, done with evidence). It follows up within opening hours, gives 10 minutes to withdraw, turns the outcome into a reminder, and scores the channel. Abdul does none of this (recon §6). So the claim "goes beyond Abdul" survives.
- **What is lost without S0:**
  - The business can no longer answer us directly. Every outcome comes back through the user, so the loop closes by the assistant asking the user, not by itself.
  - `STK-03` (a business answers with one tap and no sign-up) is not met, and jui's central idea becomes optional.
  - E11's reply-page decision rule cannot run, and the "answered via reply page" evidence level, the strongest one, is never produced.
  - A sceptical examiner could call the Must path "a deep link plus a reminder to ask the shop".
- **A second, smaller loss:** monthly recurrence moved to S3. Rent and utility bills, omm's core obligations, are monthly, so the Must obligation flow covers only one-off items.
- **Is this stated?** Partly. §1, §2 gap 1, §4.4, matrix J2 and TD6 are worded honestly. But the proposal never lists these consequences in one place, and TD6 option A says only that it "keeps the 'acts' part".

My judgement: this is an acceptable trade-off **only** with the automatic promotion rule. If the 13 Nov calendar shows scenario B or better, S0 enters Must and the full design stands. If the team is in scenario C, it should consider funding S3 (4 h) from the 7.75 h margin. A smaller S0 (Accept or Decline only, no notes) may also fit. Both are team choices. See R3.

### Remaining items (minor, fix before submission)

| ID | File / section | What | Exact fix |
|---|---|---|---|
| R1 | proposal §4.2 "Request sent" row, §5 `SEED-FR-05`, §8.2 ("First open, so state = Request sent"), §8.4 | A page load (GET) changes state, which contradicts §7.5 ("GET never changes state"). A link-preview fetch, or the requester opening their own link, would wrongly mark the card "sent" | Drop "first page open" as a trigger. Use "Yes, sent", or the first answer submitted by POST from a session that is not the requester's. Alternatively, record the first open as a separate observation that changes no state |
| R2 | proposal §7.5 last rule, §9.2 | Spotting a "self-answer" needs a LINE login token from the page (a LIFF ID token). That would make the page a LIFF app, so a business opening it may see a LINE login or consent screen. This conflicts with "no sign-up" (`STK-03`) and would collect the business person's LINE ID | Do not require LINE login on the page. Flag a self-answer using a marker set on the requester's own device when they preview the card (for example a first-party cookie). Call it best-effort |
| R3 | proposal §6.1 S0 row, §13 TD6 | The consequences of keeping S0 out of Must are not stated in one place | Add a short "If S0 stays Should" note: `STK-03` not met; E11's reply-page rule not run; loop closes only through the user; the strongest evidence level not produced; Must obligations are one-off only. In TD6, offer funding S3 from the C margin as a sub-option |
| R4 | proposal §7.4 Evidence row, §8.1 last note | Deleting the slip "when the task closes" happens at the moment of Done. The label "Done (slip attached)" then points to a deleted file, and the E6 audit cannot check it | Keep the slip for 30 days, or until the E6 audit of that task, whichever is earlier. After deletion, change the label to "slip was attached (deleted per retention)" and keep the evidence-level column |
| R5 | matrix §4, jui line | Still says jui contributes "the request card that closes the loop by tap" without saying it is now Should S0 | Add "(Should S0; Must when the calendar is scenario B or better)" |

Not re-verified: external sources (no web access in this re-check), and whether LINE's link-preview crawler fetches arbitrary URLs. The rule in §7.5 is correct either way.
