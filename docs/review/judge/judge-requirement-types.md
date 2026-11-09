# Judge verdict: requirement-types research

- Work item: `docs/requirements/requirement-types-research.md` (413 lines, dated 9 November 2026)
- Judge date: 9 November 2026. All web sources below were accessed on 9 November 2026.
- Question being answered: "Are Functional and Non-Functional Requirements enough, or do we need more kinds of requirements? Please cover all the requirements."

## Verdict: REVISE (targeted)

The core answer is right and well argued. FR and NFR are not enough. The 16-category scheme, the AI category, the ID scheme, the attributes and the SRS outline are usable. Most of the cited facts I checked are correct.

Three major defects stop it from passing as is:

1. The Thai Electronic Transactions Act (ETA) section numbers are wrong. The error appears in five places, including the requirement text.
2. The data-retention requirements conflict with each other and with a LINE rule that the file itself quotes.
3. The effort plan does not add up. The file sets 250 person-hours for build and test alone, but its own estimate for the whole project is 230–300 person-hours. The v0.9 targets for 22 November also need more work than the team has left.

All three can be fixed by editing the file. No new research is needed. Once they and the minor defects below are fixed, this should pass.

## Spot-checks (11 claims)

| # | Claim in the file | What I found | Result |
|---|---|---|---|
| 1 | ISO/IEC 25010:2023 changes: Safety added; Usability renamed Interaction capability; Portability renamed Flexibility; new sub-characteristics inclusivity, self-descriptiveness, resistance and scalability; user interface aesthetics replaced by user engagement; maturity replaced by faultlessness; accessibility split; quality in use moved to 25019 | Matches summaries of the foreword (quality.arc42.org; IEC and iTeh preview listings). iso25000.com lists the same sub-characteristics for the first four characteristics. [S] | Correct |
| 2 | OWASP Top 10 for LLM Applications 2025, LLM01–LLM10, including LLM06 Excessive Agency and LLM10 Unbounded Consumption | genai.owasp.org/llm-top-10/ gives the same list. [V] | Correct |
| 3 | LINE reply tokens can be used once and within one minute; at most 5 messages per call; "other endpoints" limited to 2,000 requests per second | developers.line.biz/en/reference/messaging-api/nojs/ says: "Reply tokens can only be used once." "Reply tokens must be used within one minute after receiving the webhook." Messages "Max: 5". 2,000 requests per second. [V] | Correct |
| 4 | Push, multicast, broadcast and narrowcast count toward the monthly quota; reply messages do not | developers.line.biz/en/docs/messaging-api/pricing/ says the same. [V] | Correct |
| 5 | LINE User Data Policy: friend and group information may be kept for at most 24 hours (s.3.2.3), used only while the user uses the service (3.2.8), and deleted when the user withdraws (3.5); no proxy sending without showing details first (3.3) | terms2.line.me/LINE_Developers_user_data_policy says: "Friend and Group information must not be stored for more than 24 hours." Sections 3.2.8, 3.3.1–3.3.2 and 3.5.2 match. One nuance: s.3.3 is about proxy sending "via the user's account". See defect m7. [V] | Correct, with a nuance |
| 6 | PDPA s.30 (access within 30 days), s.37(3) (deletion system), s.37(4) (breach notice within 72 hours where feasible), s.83 (fine up to ฿3 million covering s.24, s.28 and s.37) | Unofficial English translation hosted by KMUTT: s.30 "shall not exceed thirty days"; s.37(3) and s.37(4) as quoted; s.83 lists s.24, s.28 and s.37, up to Baht three million. [V of the translation] | Correct |
| 7 | **ETA s.13 para 2(2):** a message sent by an automatic system is the originator's. **s.15:** the receiver may treat it as what the originator intended. | ETDA's official English translation of ETA B.E. 2544 (pp. 5–6) says otherwise. **s.13** is offer and acceptance by data message. **s.15 para 2(2)** says a message sent by "an information system programmed to operate automatically in advance, by the originator…" is deemed the originator's. **s.16** is when the receiver may regard a message as the originator's. **s.17** is when the receiver may treat it "as being what the originator intended to send". The file relied on an English draft with different numbering. [V] | **Wrong** (defect M1) |
| 8 | ETA s.17/1 input-error withdrawal (added by ETA No. 3, 2019) | ETA No. 3 translation on mdes.go.th: s.17/1 applies to an input error "sent through an automated electronic message system of another party" that "does not provide the person with a channel for correcting the resulting error". [V] | Correct. The file's caveat (open issue 9) is appropriate. |
| 9 | PDPC AI guideline at pdpc.or.th/28888, posted 7 Oct 2026: non-binding; human-in-the-loop for high-risk cases; tell users at the start that they are talking to a chatbot; no bundled consent for training; 90-day reference for s.33 erasure; cover reads "Draft of Guidelines" | Page title and posting date (7 ต.ค. 2569) confirmed. PDF v2.pdf: "ไม่มีสภาพบังคับตามกฎหมายและไม่มีผลผูกพันในการพิจารณาความผิด" (no legal force, not binding on findings of offence); "Human-in-the-loop"; "ควรแจ้งให้ผู้ใช้งานทราบตั้งแต่ต้นว่ากำลังสื่อสารกับระบบอัตโนมัติ" (should tell users from the start that they are communicating with an automated system); the chatbot no-bundled-consent example; "มาตรา 33 ไม่สามารถทำได้ภายในกำหนดเวลา 90 วัน" (s.33 erasure cannot be done within the 90-day period); English header "Draft of Guidelines…". [V] | Correct |
| 10 | The draft Thai AI Act is not enacted. The public hearing closed 14 Aug 2026. Next steps: revision, Cabinet, Parliament. | Enersys (1 Oct 2026): "Thailand does not yet have an AI Act in force"; hearing opened early July and closed 14 August 2026; same next steps. [S] | Correct, and correctly tagged [S] |
| 11 | ISO/IEC/IEEE 29148:2018 SRS clause numbers (for example 9.6.15 logical database, 9.6.16 design constraints) | A DIN-hosted table of contents (via search) shows 9.6.14 performance, 9.6.15 logical database and 9.6.16 design constraints. These fit the file's numbering. [S] | Consistent |

I also checked the date example in `L10N-02` with a calendar computation: 14 December 2026 is a **Monday**, not a Saturday (defect m1).

## Judgement by criterion

- **Answers the question.** Yes. Section 0 gives a clear "No" with five reasons, and Section 1.2 lists every category. Good.
- **Completeness.** Strong coverage, but some gaps:
  - Section 1.3.7 cites NIST AI 600-1 content risks and OWASP LLM09 Misinformation, but no AI requirement covers harmful or abusive content or misinformation in answers (m4).
  - No requirement implements the LINE User Data Policy duties to offer an "unlink from LINE" and "withdraw from the service" feature (s.3.2.10), or the PDPA right to withdraw consent (s.19) (m5).
  - The privacy of non-users is not covered. Bill-split participants and LINE group members are data subjects who never consented (part of M2).
  - Business rules (for example rounding and split policy) sit inside FR. That is acceptable at this scale; no defect.
- **Redundancy.** QR-SE and QR-IC point to SEC and UX. That is handled. The confirm-before-acting rule, however, is stated four times (`FR-ACT-01`, `AI-HITL-01`, `LEG-02`, `LEG-06`), and AI disclosure twice (`AI-EXP-01`, `LEG-08`). Four copies of one rule invite drift (m2).
- **Correctness.** Mostly correct and honestly tagged. The ETA error is the exception (M1).
- **Requirement quality against ISO/IEC/IEEE 29148.** Most examples are measurable and have a stated verification method. Weak ones are listed under m3.
- **Scale for 3 students at 3–4 hours a week.** The category list is fine if each category stays small, as the file says. The effort figures and the v0.9 targets are not feasible (M3).
- **SRS structure.** Usable. It follows 29148 cl. 9.6 and clearly separates v0.9 from v1.0. Putting `DOC` in the project plan rather than the SRS is correct. Its v0.9 column needs trimming (M3).

## Defects

### Major

**M1. Wrong ETA section numbers (attribution and reliance).**
- **Where:** Section 0 point 4 (line 20); Section 1.1 ETA row (line 44); Section 1.3.9 Source line and the ETA paragraph (lines 198 and 205); `LEG-06` (line 206); Section 4 open issue 9 (line 406).
- **What is wrong:** The file says s.13 para 2(2) makes an automatic system's message the originator's, and s.15 lets the receiver treat it as intended. In the enacted Act (ETDA official English translation): s.13 is offer and acceptance; **s.15 para 2(2)** is the automatic-system attribution rule; **s.16** covers when the receiver may regard a message as the originator's; **s.17** lets the receiver treat it as what the originator intended unless it knew or should have known of an error. Open issue 9 states the wrong mapping as a "note for the team", which makes the mistake more likely to spread.
- **Why it matters:** This is the legal basis for `LEG-06` and `LEG-07`, and it will be quoted in the proposal. A wrong section number in a legal citation is a credibility defect that examiners can see.
- **Exact fix:**
  - Replace "s.13 para 2(2)" with "s.15 para 2(2)" and "s.15" (the receiver rule) with "s.17 (see also s.16)" everywhere listed.
  - In `LEG-06`, change "(ETA s.13, s.15; …)" to "(ETA s.15 para 2(2), s.17; …)".
  - Change the source to the ETDA official English translation of ETA B.E. 2544 and the tag to [V]. Drop the SOFTIC draft as a source for numbering.
  - Rewrite the "note for the team" in open issue 9 to give the correct mapping.

**M2. The data-retention requirements conflict with each other and with a quoted LINE rule.**
- **Where:** Sections 1.3.6 (`DR-04`), 1.3.8, 1.3.11 (`TR-03`), and 1.1 (Computer-Related Crime Act row).
- **What is wrong:**
  - (a) Section 1.3.6 quotes LINE User Data Policy s.3.2.3 ("Friend and Group information must not be stored for more than 24 hours"), but no `DR` requirement enforces it. `DR-04` keeps conversation text, which includes group chats and bill-split participants, for 30 days.
  - (b) `DR-04` keeps audit records for 180 days, but `TR-03` deletes or anonymises all pilot data within 30 days of the pilot's end. No precedence rule says which wins.
  - (c) The Computer-Related Crime Act s.26 (traffic data kept for at least 90 days) conflicts with the 30-day deletion in `DR-04`. Section 2.1 uses exactly this kind of "30-day vs 90-day" conflict as its warning example, yet the file does not flag it in its own set.
- **Why it matters:** The set is not consistent (29148 set characteristic), and there is a real risk of breaching LINE policy, which can lead to channel suspension (Developers Agreement §5.6, cited in the file).
- **Exact fix:**
  - Add `DR-0x`: "Profile and membership information obtained from LINE friend or group APIs shall be deleted within 24 hours of retrieval and shall not be used after the user leaves the service (LINE User Data Policy s.3.2.3, s.3.2.8)."
  - Add a non-user privacy requirement, `PRV-0x`: "Data about bill-split participants and group members who are not users shall be limited to display name and amount and shall be deleted when the split is settled or after N days."
  - Add one precedence line to `TR-03`: audit logs are kept until the end of the evaluation report, then deleted.
  - Add an open issue noting the CCA s.26 vs `DR-04` conflict, pending advice on whether the team counts as a "service provider".

**M3. The effort budget and the v0.9 targets are not feasible.**
- **Where:** `CON-TEAM-01` (Section 1.3.10); `VER-01` (Section 1.3.15); Section 3 v0.9 column.
- **What is wrong:**
  - Section 1.3.10 estimates 230–300 person-hours for the *whole* project, which includes the proposal, documents, meetings, the pilot and the report. `CON-TEAM-01` gives 250 of those hours to build and test alone. That leaves almost nothing for everything else.
  - `VER-01` requires every Must requirement to have a linked test or evaluation case by the proposal.
  - Section 3 asks v0.9 to include all Must use cases, a full AI requirements list with thresholds, one measured requirement per ISO 25010 characteristic, an ER diagram, a retention table, a threat list, and a test strategy plus evaluation protocol.
  - About 13 days remain before 22 November, which is roughly 20–25 team-hours. That time is also needed for merging three directions and writing the combined proposal.
- **Why it matters:** The "feasible" criterion in the file's own Section 2.1 checklist fails. The team would either miss the deadline or fill the SRS with numbers it cannot defend, which Section 3's own "what not to do" warns against.
- **Exact fix:**
  - Set `CON-TEAM-01` to about 50–60% of total hours (about 120–160 person-hours, labelled as an estimate).
  - Change `VER-01` to: "every Must requirement shall have an acceptance criterion before the proposal, and a linked test or evaluation case before the Phase 1 gate."
  - Cut the v0.9 column to: Section 1 in full; Section 2 in full; IF named; FR as Must use cases only; AI as a list of requirements with "TBD after spike" thresholds; QR as one line per characteristic or "N/A"; DR as entity list plus retention table; SEC/PRV and LEG as checklists; VER as strategy only. Everything else moves to v1.0.

### Minor

**m1. Wrong weekday in a date example.**
- **Where:** `L10N-02`.
- **What is wrong:** "ส. 14 ธ.ค. 2569 / Sat 14 Dec 2026". 14 December 2026 is a Monday. This is a requirement about showing dates correctly.
- **Fix:** "จ. 14 ธ.ค. 2569 / Mon 14 Dec 2026".

**m2. Duplicate requirements.**
- **Where:** Confirm-before-acting appears in `FR-ACT-01`, `AI-HITL-01`, `LEG-02` and `LEG-06`. AI disclosure appears in `AI-EXP-01` and `LEG-08`.
- **Fix:** Keep one master requirement for each (for example `AI-HITL-01` for confirmation and `AI-EXP-01` for disclosure). Turn the `LEG` items into compliance rows that trace down to the master ("satisfied by AI-HITL-01").

**m3. Some example requirements break 29148 qualities.**
- **What is wrong:**
  - `QR-MA-01` and `PRV-01` each state two requirements (not singular).
  - `QR-SA-02` uses "look like money transfers to unknown accounts" (ambiguous).
  - `QR-CO-01` says "4 major Thai banking apps" without naming them.
  - `QR-PE-02` says "without errors" but gives no error measure.
  - `BR-03` "evidence that decides whether … beats …" has no decision criterion.
  - `UX-01` "after the first request" is ambiguous.
- **Fix:** Split the compound ones. Name the apps. Define "unknown account" (for example a PromptPay ID not previously confirmed by the user). Give an error-rate threshold. For `BR-03`, state the comparison metric (for example task success and corrections per task against a reminder-only baseline).

**m4. No content-safety requirement in the AI category.**
- **Where:** Section 1.3.7.
- **What is wrong:** No AI sub-type covers harmful or abusive content or misinformation in free-text answers. NIST AI 600-1 and OWASP LLM09 are cited for this but not turned into requirements.
- **Fix:** Add `AI-SAFE-01`: "Free-text answers on health, legal or financial questions shall carry a 'not professional advice' notice and shall not recommend a specific action; an adversarial set of ≥ 20 harmful-content prompts shall yield 0 policy-violating outputs." Add an AREA code `SAFE`.

**m5. Missing withdrawal and unlink requirements.**
- **Where:** Section 1.3.8.
- **What is wrong:** No requirement covers consent withdrawal (PDPA s.19) or the LINE User Data Policy s.3.2.10 duty to provide an "unlink from LINE" and "withdraw from the service" feature.
- **Fix:** Add `PRV-0x`: "The user shall be able to withdraw consent and leave the service from the settings page; on withdrawal, the system shall stop processing and delete LINE user information per DR retention rules (PDPA s.19; LINE UDP s.3.2.10, s.3.5.2)."

**m6. Age of majority left unverified.**
- **Where:** `LEG-05`; open issue 7.
- **What is wrong:** The Thai age of majority (20, Civil and Commercial Code s.19) is left UNVERIFIED, but it is easy to verify and matters because first-year university students are often 18–19.
- **Fix:** Verify it and keep `LEG-05`. Note that the target users (students) include minors.

**m7. Over-broad reading of LINE User Data Policy s.3.3.**
- **Where:** Section 0 point 3; `LEG-02`.
- **What is wrong:** s.3.3 governs proxy sending "via the user's account" (for example LIFF send or share features). Messages our Official Account sends to a business are not literally covered.
- **Fix:** Say s.3.3 applies directly to any LIFF send or share feature, and that we apply it by analogy to bot-sent messages to third parties.

**m8. Overlapping AI metrics.**
- **Where:** `QR-FS-01` (≥ 85% task completion) vs `AI-EVAL-02` (≥ 90% intent and ≥ 85% slot accuracy).
- **What is wrong:** Two different pass marks for overlapping things, with no stated link between them.
- **Fix:** Make `QR-FS-01` reference `AI-EVAL-02` as its measure, or define task completion separately from intent and slot accuracy.

## What is good and should be kept

- A clear, well-reasoned answer to the team's question, with 16 categories each carrying a definition, rationale, source, examples and a verification method.
- An AI category that turns LLM risks into testable requirements: confirmation enforced in server code, an injection test set, a held-out evaluation split, and cost caps.
- Careful evidence tagging and honest open issues: paywalled ISO text, the status of the PDPC guideline, and the draft AI Act.
- A lightweight traceability scheme (trace columns plus IDs in tests and commits) that suits three students.

---

## Re-check (9 Nov 2026)

I checked only the changed parts of `docs/requirements/requirement-types-research.md` (now 418 lines). I also looked for new contradictions, especially among the retention rules and between the master requirements and their compliance rows.

### Status of each defect

| Defect | Status | What I checked |
|---|---|---|
| M1 ETA section numbers | **Fixed** | All five places now cite s.15 para 2(2), s.16 and s.17, from ETDA's English translation, tagged [V]. Lines 20, 44, 171, 203, 210, 211 and open issue 9 agree with each other. The quoted wording of s.15 para 2(2) matches the ETDA text I read. The SOFTIC draft is no longer used as a source. |
| M2 Retention conflicts | **Fixed** | `DR-07` (24 h, LINE UDP s.3.2.3 and s.3.2.8) added. `DR-04` now covers one-to-one chat only, and audit records are kept for "180 days or until the evaluation report, whichever is earlier". `PRV-06` covers non-users. `TR-03` adds a precedence rule: audit logs are kept until the report, and every shorter limit still applies. Open issue 16 records the Computer-Related Crime Act s.26 conflict and a sensible way out (keep traffic metadata, not content, for 90 days). The rules do not contradict each other: where `DR-07` (24 h) and `PRV-06` (30 days) overlap for data from LINE group APIs, the shorter-limit rule in `TR-03` makes 24 h apply. |
| M3 Feasibility | **Fixed** | `CON-TEAM-01` is now 120–160 person-hours (50–60% of the 230–300 total, labelled as an estimate). `VER-01` now requires only acceptance criteria at the proposal and test links at the Phase 1 gate. The v0.9 column has been cut as recommended: TR, OPS and traceability moved to v1.0, and AI thresholds are "TBD after spike". |
| m1 Weekday | Fixed | "จ. 14 ธ.ค. 2569 / Mon 14 Dec 2026". |
| m2 Duplicates | Fixed | `AI-HITL-01` and `AI-EXP-01` are now marked MASTER. `LEG-02`, `LEG-06` and `LEG-08` are compliance rows that say they are "Satisfied by" a master. `FR-ACT-01` now defines only the card's content. The trace links run in both directions and agree. |
| m3 Requirement quality | Fixed | `QR-MA-01/02` split. `PRV-01` and `PRV-08` split. `QR-SA-02` defines "unknown account". `QR-CO-01` names four apps (tagged [I] as the team's choice). `QR-PE-02` sets ≤ 1% errors. `BR-03` has a decision rule. `UX-01` is reworded clearly. |
| m4 Content safety | Fixed | `AI-SAFE-01` and `AI-SAFE-02` added; the AREA code `SAFE` is added to the ID scheme. |
| m5 Withdrawal and unlink | Fixed | `PRV-07` added (PDPA s.19; LINE UDP s.3.2.10 and s.3.5.2). |
| m6 Age of majority | Fixed | `LEG-05` and open issue 7 now explain it: majority at 20 (Civil and Commercial Code s.19, tagged [S]). The PDPA s.20(1) parental-consent reading matches the translation I checked. |
| m7 Scope of LINE UDP s.3.3 | Fixed | Now "via the user's account": applies directly to LIFF send or share features and by analogy to bot-sent messages (§0, §1.3.7, `LEG-02`). |
| m8 Overlapping AI metrics | Fixed | `QR-FS-01` is now the end-to-end measure and says `AI-EVAL-02` supplies its component pass marks. |

### Tags
No unsupported tags found. The new [V] tags point to sources I opened myself: the ETDA translation of the ETA, the LINE User Data Policy and the PDPA translation. The Civil and Commercial Code and the CCA s.26 are tagged [S], and the app list is tagged [I].

### Remaining notes (non-blocking; no change needed to pass)
- `PRV-07` says "delete LINE user information under the `DR` retention rules". LINE UDP s.3.5.2 requires deleting *all* LINE user information, including internal identifiers, when a user withdraws, apart from narrow exceptions (legal duty, handling inquiries). Suggested wording: "delete all LINE user information including identifiers, except audit records kept under `DR-04` for handling inquiries (UDP s.3.5.2.2)".
- With `DR-04` narrowed to one-to-one chats, the retention of *group* chat text is no longer stated. Add one line (for example: not stored beyond the task, or 24 h).

## Final verdict: PASS
