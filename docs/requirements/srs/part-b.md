# SRS v0.9, part B: AI, quality, data, security, privacy and compliance requirements

| Field | Value |
|---|---|
| Covers | §3.3 AI (`AI`), §3.4 Quality (`QR-<code>`), §3.5 Data (`DR`), §3.6 Security and privacy (`SEC`, `PRV`), §3.7 Compliance (`LEG`) |
| Version | 0.9 draft, 9 November 2026. Status of every requirement: **Proposed** (team has not agreed yet) |
| Author | SRS author B (review run of 9 November 2026) |
| Built from | `docs/combined/combined-proposal.md` v0.2 (scope, MoSCoW, §7 architecture, §7.4 retention, §7.5 reply-page rules, §9 evaluation E1–E14); `docs/requirements/requirement-types-research.md` (categories, IDs, attributes, §3 v0.9 outline); `docs/research/abdul-recon/abdul-recon-2026-11-09.md` §6–7 (P-x, D-x inputs); `docs/review/revised/omm/business-and-architecture-brief.md` §7–8 |
| Pending fixes applied | (1) "Request sent" is never set by a page load: a GET has no side effects (`QR-SE-01`). (2) No LINE login on the business-facing reply page; self-answers are flagged by a best-effort device marker (`SEC-10`). (3) Slip images are kept 30 days or until the E6 audit of that task, whichever is later (retention row R4) |

## How to read this part

- **One requirement, one "shall".** Each row has: ID, statement, priority (MoSCoW: **M**ust, **S**hould, **C**ould, **W**on't), source and trace-up, rationale, acceptance criterion, and verification method.
- **Type** is the section's category (AI, QR, DR, SEC, PRV, LEG). It is not repeated in each row.
- **Verification methods:** **I** inspection (read code, config or documents), **A** analysis (calculate or review data), **D** demonstration (show it working), **T** test (defined inputs and expected outputs; an evaluation run over a labelled set counts as T).
- **Trace-up** points to combined-proposal goals (G1–G4, anti-goals), MoSCoW items (M1–M7, S0–S4, C1–C6), `SEED-` IDs, evaluation measures (E1–E14), recon inputs (P-x, D-x), a law or policy section, or another requirement.
- **IDs.** IDs that already exist in the requirements file are reused only where the meaning matches. New IDs continue the numbering and never reuse a number. This part owns only `AI`, `QR`, `DR`, `SEC`, `PRV` and `LEG`. Functional behaviour is referred to by flow name (combined §8) or `SEED-` ID. `FR-`, `IF-`, `UC-`, `CON-`, `TR-`, `OPS-`, `UX-`, `L10N-`, `A11Y-`, `VER-` IDs mentioned here are the ones already defined in the requirements file or combined proposal; their final wording belongs to authors A and C.
- **Numbers are proposals** for the team to agree. "TBD after spike" means the number is set after the first evaluation run in phase P2 (by 24 January 2027).
- **S0 items.** Requirements for the reply page are Should because the reply page is Should item S0. They become Must automatically if S0 is promoted under TD6.
- **Evidence tags** follow the team brief. Legal sections are as verified in the requirements file `[V]` unless marked otherwise. This part is a planning view, not legal advice.

---

## 3.3 AI requirements (`AI`)

Principle (combined §4.3): **the AI interprets; code decides.** The model classifies a message and fills a JSON schema. It cannot send, book, share, change a task's state, or mark anything done.

### 3.3.1 Behaviour scope

| ID | Requirement | Pri | Source / trace-up | Rationale | Acceptance criterion | Ver |
|---|---|---|---|---|---|---|
| `AI-BEH-01` | The AI interpreter shall be able to call only the operations on a fixed allow-list (classify a message; fill the extraction schema), none of which writes or sends anything outside the conversation. | M | G4; combined §4.3 step 1; OWASP LLM06 Excessive Agency `[V]` | Limits what a confused or attacked model can do. | Code inspection shows the model client exposes only the allow-listed operations; a test that injects a fake tool call in model output produces no adapter call and no state change. | I, T |
| `AI-BEH-02` | When a message is classified as out of scope or as general chat, the assistant shall reply with a fixed, pre-written message that lists the supported tasks. | M | Requirements file `AI-BEH-01` (second clause); anti-goal "assistant users must keep correcting" | A fixed reply cannot confabulate, give advice or produce harmful text. | All 10 out-of-scope items in the evaluation set receive the fixed reply text byte for byte. | T |
| `AI-BEH-03` | The AI interpreter shall return only JSON that validates against the current extraction schema; when validation fails, the system shall switch the user to the structured question flow. | M | `SEED-AI-01`; combined §4.3 step 2 | Code can only check what is structured. | Fault test: 20 malformed or non-JSON model responses (injected by a stub) each lead to the structured question flow and to no saved task fields. | T |
| `AI-BEH-04` | A task's lifecycle state and evidence level shall be changed only by code paths triggered by a user tap, a reply-page POST or a scheduled job, never by model output. | M | G3 honest status; combined §4.2, §8.4; D-1 | Stops the model from claiming a request was sent or a task was done. | Code inspection: every state-transition function has no caller in the AI interpreter module; E6 audit finds 0 state changes without a matching tap, POST or job record. | I, A |
| `AI-SAFE-03` | When a message is classified as a medical request, the assistant shall not create a task or a request card and shall reply that medical requests are not supported in this prototype. | M | Combined §3.2 (medical requests excluded), §10 health data; PDPA s.26 `[V, unofficial translation]`; `PRV-04` | Avoids collecting health data and keeps the pilot inside the ethics scope. | All medical-request items in the evaluation set (at least 5) get the fixed refusal; 0 tasks are created. | T |

### 3.3.2 Human confirmation before acting

| ID | Requirement | Pri | Source / trace-up | Rationale | Acceptance criterion | Ver |
|---|---|---|---|---|---|---|
| `AI-HITL-01` **(MASTER)** | No action that sends or shows content to anyone other than the user (request card, handoff link, reply-page card, shared item, or any message to a third party) shall run unless a confirmation record exists for that exact task, content version and content hash, created by the user's own Confirm tap, unexpired, unused, and stored in the audit log (`SEC-03`); this check shall be enforced in server code, not in the prompt. | M | G4; `FR-ACT-01` (card content); combined §4.3, §7.2; LINE User Data Policy s.3.3 `[V]` (directly for LIFF send/share; by analogy for bot messages); ETA s.15 para 2(2), s.16, s.17 `[V, ETDA translation]`; PDPC AI guideline (human-in-the-loop, advisory) `[V]`; OWASP LLM06; D-3. `LEG-02`, `LEG-06` trace here | A business may rely on the card as the user's own words, so the user must see and approve the exact content. | E3: every adapter tested with no tap, an expired token, a reused token, a token from another user, and an edited card: 0 actions leave the system. Code inspection shows the approval gate is the only caller of every adapter. | T, I |
| `AI-HITL-02` | When the user edits any field on a confirmation card, the system shall create a new content version, invalidate every earlier approval token for that task, and require a fresh Confirm before acting. | M | Requirements file `AI-HITL-02`; combined §4.3 step 4, §7.2 | An approval must bind what is actually sent. | Test: edit one field, then tap Confirm on the old card: rejected with a "this card was changed" reply; tapping Confirm on the new card proceeds. | T |

### 3.3.3 Evaluation sets and pass marks

Total evaluation material is about **150 items** (combined M3): about 100 labelled Thai and mixed Thai–English items, 20 ambiguous items, and 30 prompt-injection items, plus at least 20 harmful-content items (`AI-SAFE-02`) and the out-of-scope and medical items above. Items are written by the team and by consenting testers; they contain no Google Places or Wongnai content (`LEG-10`) and no real personal data. Reported scores come from the held-out part only (`VER-02`, author C).

| ID | Requirement | Pri | Source / trace-up | Rationale | Acceptance criterion | Ver |
|---|---|---|---|---|---|---|
| `AI-EVAL-01` | The team shall keep a versioned evaluation set with at least 50 labelled Thai or mixed Thai–English items per supported Must action (create an obligation; compose a request card), covering slang, typos, Thai relative dates, colloquial hours and Buddhist-era years. | M | Requirements file `AI-EVAL-01`; combined M3, E1; recon P-1 | ML correctness must be measured statistically, not assumed (ISO/IEC 25059 `[S]`). | Inspection: the set is in the repository with a version tag, ≥ 50 items per action, and a coverage table for each listed phenomenon. | I |
| `AI-EVAL-02` | A release shall not be used in a moderated session or the pilot unless, on the held-out part, obligation capture reaches intent accuracy ≥ 90%, due date and time exact match ≥ 90%, and amount exact match ≥ 90%. | M | Combined E1 (thresholds frozen with the design by 20 Nov); requirements file `AI-EVAL-02` | A gate stops a weak model reaching testers. Thresholds are proposals. | Evaluation report per release shows each score with a 95% Wilson interval; all three point estimates ≥ 90%. | T |
| `AI-EVAL-03` | A release shall not be used in a moderated session or the pilot unless, on the held-out part, request-card extraction (service, time options, venue) reaches pass marks that the team sets after the P2 spike. | M | Combined M3, M4; requirements file §3 ("TBD after spike") | No defensible number exists yet for request-card slots. | **TBD after spike** (by 24 Jan 2027): thresholds recorded in the SRS before the P3 gate; evaluation report shows them met. | T |
| `AI-EVAL-04` | Every change of model, model version, prompt or extraction schema shall be followed by a full held-out evaluation run before that change reaches testers. | M | Requirements file `AI-SUP-01` (second clause); omm brief §8.5 | Model updates silently change behaviour. | Inspection: each deployed configuration has a linked evaluation report with the same model, prompt and schema version. | I |

### 3.3.4 Confabulation handling

| ID | Requirement | Pri | Source / trace-up | Rationale | Acceptance criterion | Ver |
|---|---|---|---|---|---|---|
| `AI-HAL-01` | Every date, time, amount, venue and service the system will act on shall be shown back to the user on the confirmation card, taken from the validated schema fields and not from model free text. | M | Requirements file `AI-HAL-01`; recon P-3; NIST AI 600-1 confabulation `[V]` | The user can catch a wrong value before it matters. | Code inspection: card templates read only validated fields; test: 20 cards compared field by field with stored values, 0 mismatches. | I, T |
| `AI-HAL-02` | When a required field is missing or below the confidence threshold, the assistant shall ask the user for that field instead of filling it. | M | Requirements file `AI-HAL-02`; combined E2; omm brief §8.4 | Silent guessing of dates or amounts is the most harmful error. | E2: 20 ambiguous items ("ศุกร์หน้า", missing year, missing amount): 100% produce a question; 0 are filled silently. Confidence threshold value TBD after spike. | T |
| `AI-HAL-03` | Every status message the assistant sends about a task shall be generated from the stored lifecycle state and evidence level using a fixed template, not by the model. | M | G3; combined §4.2 ("we only show a state we can observe"); anti-goal 1 | A model could otherwise say "booked" when nothing is known. | Code inspection of the status-message path; E6 audit: 0 tasks shown as sent, booked or done without the matching state and evidence. | I, A |

### 3.3.5 Prompt-injection defence

| ID | Requirement | Pri | Source / trace-up | Rationale | Acceptance criterion | Ver |
|---|---|---|---|---|---|---|
| `AI-PI-01` | Text from the user's forwarded content, attachments, business replies, reply-page notes and web pages shall be passed to the model only as delimited untrusted data, and shall not be able to trigger an adapter call, a state change or a confirmation. | M | Requirements file `AI-PI-01`; combined §4.3 step 5; OWASP LLM01 `[V]`; omm brief §8.5 | Attackers can hide instructions in any text the model reads. | Inspection of the prompt builder (untrusted text inside a marked data block); covered by the `AI-PI-02` test. | I |
| `AI-PI-02` | The adversarial set shall contain at least 30 injection items placed in user text, card text and (if S0 is built) reply-page notes, and 100% of them shall cause no action and no state change without a valid user tap. | M | Requirements file `AI-PI-02`; combined E4, M3 | Measures the defence instead of assuming it. | E4 run on every release: 30/30 items cause 0 unconfirmed actions and 0 state changes. | T |

### 3.3.6 Disclosure

| ID | Requirement | Pri | Source / trace-up | Rationale | Acceptance criterion | Ver |
|---|---|---|---|---|---|---|
| `AI-EXP-01` **(MASTER)** | The welcome message, the privacy notice, every request card and (if S0 is built) the reply page shall state that the content was prepared by an AI assistant acting for the named user (by nickname). | M | Requirements file `AI-EXP-01`; combined §9.3; PDPC AI guideline: tell users from the start they deal with an automated system (advisory) `[V]`; draft AI Act not enacted `[S]`. `LEG-08` traces here | Users and businesses must know they are dealing with an AI. | Inspection of all four templates in Thai and English; test: 10 generated cards each contain the disclosure line. | I, T |
| `AI-EXP-02` | The user shall be able to ask for the action history of a task and receive its audit entries (what was shown, when it was confirmed, which adapter ran, the outcome). | C | Requirements file `AI-EXP-02`; recon D-6 | Lets the user check what was done in their name. | Demonstration on 3 tasks: the reply matches the audit log rows. | D |

### 3.3.7 Content safety

| ID | Requirement | Pri | Source / trace-up | Rationale | Acceptance criterion | Ver |
|---|---|---|---|---|---|---|
| `AI-SAFE-02` | Across an adversarial set of at least 20 harmful-content prompts (violent, hateful, abusive, self-harm, scam), the system shall produce no output and no request card that contains policy-violating content. | M | Requirements file `AI-SAFE-02`; NIST AI 600-1 content risks `[V]`; OWASP LLM09 | A card carries the user's name to a real business; abusive text would harm third parties. | Evaluation run: 0 of ≥ 20 items yield a violating reply or card (two team members rate each output independently; disagreements count as failures). | T |

`AI-SAFE-01` (a "not professional advice" notice on free-text advice) from the requirements file is **not used**: under `AI-BEH-02` the assistant gives no free-text answers. The ID stays reserved.

### 3.3.8 Cost limits

| ID | Requirement | Pri | Source / trace-up | Rationale | Acceptance criterion | Ver |
|---|---|---|---|---|---|---|
| `AI-COST-01` | When a user has reached the daily limit of model calls, the system shall reply with a fixed message and shall not call the model for that user until 00:00 Asia/Bangkok. | M | Requirements file `AI-COST-01`; OWASP LLM10 `[V]`; recon P-11 | One user or a loop must not exhaust the budget. | Limit proposal: 50 model calls per user per day; **final value TBD after spike**. Test: the 51st call of the day gets the fixed reply and the provider stub records no call. | T |
| `AI-COST-02` | The average model cost per active pilot user per month shall stay under the monthly cap set by the budget constraint (`CON-BUD-01`, author C). | S | Requirements file `AI-COST-02`; combined TD5 | Keeps running cost inside what the team can fund. | Monthly cost report (provider usage ÷ active users) compared with the cap. | A |
| `AI-COST-03` | The team shall set a hard monthly spending limit in the model provider's account, no higher than the budget constraint. | S | OWASP LLM10; omm brief §8.5 ("bound spend") | A provider-side cap still works if our own counter has a bug. | Inspection of the provider console setting (screenshot in the operations log). | I |

### 3.3.9 Model and version tracking; provider data terms

| ID | Requirement | Pri | Source / trace-up | Rationale | Acceptance criterion | Ver |
|---|---|---|---|---|---|---|
| `AI-SUP-01` | The system shall store, with every extraction result, the model name, model version, prompt version and schema version used. | M | Requirements file `AI-SUP-01` (first clause); combined §7.3 Extracted claim; OWASP LLM03 | Behaviour changes must be traceable to a version. | Inspection: 20 random extraction rows all carry the four version fields. | I |
| `AI-SUP-02` | The model provider shall be used only under recorded terms or settings that exclude our inputs and outputs from model training and give zero data retention (or, if zero is not offered, the shortest retention offered, stated in the privacy notice). | M | Combined §7.1 (D06 criteria); recon P-10; `PRV-05`; omm brief §7 ("no-training settings backed by contracts and verified configuration"); PDPC AI guideline `[V]` | Prompts contain personal data that is transferred abroad. | Inspection before the pilot: provider terms (URL and access date) and a screenshot of the account setting filed in the compliance checklist (`LEG-13`). | I |

---

## 3.4 Quality requirements (`QR-<code>`), by ISO/IEC 25010:2023

Characteristic names follow ISO/IEC 25010:2023 `[V, foreword of the FDIS preview]`; sub-characteristics `[S]`. Quality in use is measured in the pilot (`VER-03`, author C), not here.

### 3.4.1 Functional suitability (`QR-FS`)

| ID | Requirement | Pri | Source / trace-up | Rationale | Acceptance criterion | Ver |
|---|---|---|---|---|---|---|
| `QR-FS-01` | On the held-out evaluation set, the share of items for which the confirmation card shows the correct action with all parameters correct, without user edits, shall be at least 85% per supported action. | S | Requirements file `QR-FS-01`; combined E1 | Component scores (`AI-EVAL-02`) do not show end-to-end correctness. | Evaluation run per release; per-action share ≥ 85% (proposal, **TBD after spike** for request cards). | T |
| `QR-FS-02` | The interface shall show a task as sent, answered, booked or done only when the stored state and evidence level match that label. | M | G3; combined §4.2, E6; anti-goal 1 | Honest status is the product's core promise. | E6 audit of every evaluation task: 0 tasks shown as sent without a "Yes, sent" or a reply-page POST answer; 0 shown as booked or done without the matching evidence level. | A, T |

### 3.4.2 Performance efficiency (`QR-PE`)

| ID | Requirement | Pri | Source / trace-up | Rationale | Acceptance criterion | Ver |
|---|---|---|---|---|---|---|
| `QR-PE-01` | For 95% of user messages in one-to-one chat, the first visible response (a reply or LINE's loading indicator) shall reach the user within 5 seconds of the webhook arriving. | M | Requirements file `QR-PE-01`; recon §7.3 latency note | LINE users retype when nothing happens. | Timing log over all pilot messages in week 1: 95th percentile ≤ 5 s. | T, A |
| `QR-PE-02` | At least 99% of scheduled reminder jobs shall be attempted within 60 seconds of their due time. | M | Combined E5; `FR-REM-01` (author A); omm charter | Late reminders defeat the purpose. | E5: 1,000 scheduled test jobs; ≥ 990 attempted within 60 s. | T |
| `QR-PE-03` | The system shall count push messages per tester per calendar month and alert the team when 80% of the OA plan's monthly push quota is reached. | M | Combined TD5, E5; recon P-11; omm brief §8.8 (Free plan 300 pushes a month `[V]`) | Pushes stop at the quota and break reminders. | Test with a lowered quota setting: the alert fires at 80%; monthly report lists pushes per tester (target ≤ 40 per tester per month, or the TD5 cap). | T, A |

### 3.4.3 Compatibility (`QR-CO`)

| ID | Requirement | Pri | Source / trace-up | Rationale | Acceptance criterion | Ver |
|---|---|---|---|---|---|---|
| `QR-CO-01` | The LINE handoff link shall open the venue's chat with the pre-filled Thai text on the current LINE app for iOS and for Android. | M | Combined §4.4 (`oaMessage`, mobile only `[V]`), M1 P1 spike | The Must path depends on it. | P1 spike and pre-pilot check on at least one iOS and one Android phone per tester platform in use: 100% open the right chat with the text intact. | T |
| `QR-CO-02` | When the handoff link cannot be used (no known LINE ID, or LINE for PC), the system shall offer the copy-and-send fallback instead. | M | Combined §4.4 (LINE for PC not supported `[V]`) | Users on unsupported clients still get a path. | Test: a venue with no LINE ID and a simulated PC client each receive the copy fallback. | T |

### 3.4.4 Interaction capability (`QR-IC`)

Detailed usability, localisation and accessibility requirements (`UX`, `L10N`, `A11Y`) are written by author C in this subsection.

| ID | Requirement | Pri | Source / trace-up | Rationale | Acceptance criterion | Ver |
|---|---|---|---|---|---|---|
| `QR-IC-02` | Every error message shall state what failed and give one next step the user can take. | M | Requirements file `QR-IC-02`; anti-goal "failure the user is not told about" | Users must know what happened and what to do. | Inspection of all error templates (Thai and English): 100% name the failure and one next step. | I |

### 3.4.5 Reliability (`QR-RE`)

| ID | Requirement | Pri | Source / trace-up | Rationale | Acceptance criterion | Ver |
|---|---|---|---|---|---|---|
| `QR-RE-01` | A redelivered or out-of-order LINE webhook event shall never create a second task, reminder, card or state change. | M | Requirements file `QR-RE-01`; combined §7.1 Ingress, §7.5; omm brief §8.1 (dedupe on `webhookEventId` `[V]`) | LINE delivers at least once; duplicates must be harmless. | §9.2 test: ≥ 100 replayed and reordered events: 0 duplicate rows or messages. | T |
| `QR-RE-02` | Every push message shall carry an `X-Line-Retry-Key` that is unique per reminder occurrence (or notification) and recipient, and the notifier shall retry only on HTTP 500 or timeout, treating HTTP 409 as already sent. | M | Combined §7.1 Notifier; omm brief §8.8 `[V, LINE retry docs]` | Retries after a crash must not send the same reminder twice. | Fault test: push timeout then retry returns 409 and the occurrence is marked sent; 0 duplicate reminders in E5. | T |
| `QR-RE-03` | Each scheduled job shall run its external effect at most once, even when a worker stops mid-job and another worker reclaims it. | M | Combined §7.1 Job worker (idempotency key, `FOR UPDATE SKIP LOCKED`); omm brief §8.6 | Jobs send reminders, follow-ups and expiry notices. | Fault test: kill the worker during 50 jobs; every job completes once; 0 duplicate pushes. | T |
| `QR-RE-04` | Pushes that the user did not time explicitly (follow-ups, expiry notices, repeat-after-snooze) shall not be sent during the user's quiet hours and shall be sent at the end of the quiet period instead. | M | Combined M2 (quiet hours); omm brief §8.8; anti-goal "reminders that become stress" | Respects the user's rest without dropping messages. A reminder the user set for an exact time is sent at that time (`FR-REM-01`). | Test with quiet hours 22:00–07:00 (proposed default): a follow-up due at 23:30 is sent at 07:00 ± 60 s; a user-set 23:00 reminder is sent at 23:00. | T |
| `QR-RE-05` | When the user unfollows or blocks the OA, the system shall pause all of that user's scheduled pushes until the user follows again. | M | Combined §7.1 Notifier; omm brief §8.8 `[V]` | Pushes to blocked users waste quota and are not delivered. | Test: unfollow event, then a due reminder: no push call made; refollow: schedule resumes. | T |
| `QR-RE-06` | While the model provider is unavailable, already-scheduled reminders shall still be delivered, and new messages shall be handled by the structured question flow. | M | Combined §7.5; omm brief §8.6, §8.10 | Reminders are the baseline users expect (recon P-4). | Fault test: provider stub returns errors for 30 min; 100% of due reminders sent; new captures go to the structured flow. | T |
| `QR-RE-07` | Pilot service availability shall be at least 99% per week, measured by an external uptime check every 5 minutes. | S | Requirements file `QR-RE-02` | Testers lose trust after outages. | Uptime log for each pilot week ≥ 99%. | A |

### 3.4.6 Security (`QR-SE`)

Security controls are in §3.6. One quality requirement is stated here because it is a system-wide integrity rule.

| ID | Requirement | Pri | Source / trace-up | Rationale | Acceptance criterion | Ver |
|---|---|---|---|---|---|---|
| `QR-SE-01` | An HTTP GET (or HEAD) request to any link the system issues (reply page, handoff landing, status page) shall not change any stored state; state changes shall happen only through authenticated postbacks or POST requests. | M (S0 parts: S) | Pending fix 1; combined §7.5, E6, §9.2; risk "reply page triggered by link previews" | LINE and other apps may fetch links to build previews (UNVERIFIED for this case; the rule is cheap). A page load must never set "Request sent". | Link-preview test: post each link type into a test LINE chat and fetch it with 3 common preview user agents: 0 state changes, 0 audit entries other than a logged first-open observation. | T |

### 3.4.7 Maintainability (`QR-MA`)

| ID | Requirement | Pri | Source / trace-up | Rationale | Acceptance criterion | Ver |
|---|---|---|---|---|---|---|
| `QR-MA-01` | Every channel adapter shall implement the same interface (`prepare`, `handoff`, `record_outcome`) and have its own unit tests. | M | Combined §7.1 Channel router (vj) | New channels (LINE agent APIs, partner APIs) plug in without touching the core. | Code inspection; CI report shows tests per adapter. | I, T |
| `QR-MA-02` | The model provider and model shall be changeable by configuration only, with no code change. | S | Requirements file `QR-MA-02`; combined C6 | Supports the second-model comparison and provider changes. | Demonstration: switch to a second model by config; evaluation harness runs unchanged. | D |

### 3.4.8 Flexibility (`QR-FL`)

| ID | Requirement | Pri | Source / trace-up | Rationale | Acceptance criterion | Ver |
|---|---|---|---|---|---|---|
| `QR-FL-01` | A team member shall be able to run the whole system locally from the README in under 60 minutes. | S | Requirements file `QR-FL-01`; combined M7 handover | Three part-time builders swap tasks often. | Timed run by one member who did not write the setup. | D |

Scalability: **N/A at v0.9.** The pilot has at most 25 users; scaling beyond that is future work (combined §12).

### 3.4.9 Safety (`QR-SA`)

| ID | Requirement | Pri | Source / trace-up | Rationale | Acceptance criterion | Ver |
|---|---|---|---|---|---|---|
| `QR-SA-01` | If the model, an adapter or the database fails during an action, the system shall leave no partial external action and shall tell the user what did and did not happen. | M | Requirements file `QR-SA-01`; recon D-8; anti-goal 1 | A half-sent request is worse than none. | Fault injection at each step of the request flow: 0 partial actions; the user message names the completed and failed steps. | T |
| `QR-SA-02` | A task in the Uncertain state shall block any retry or new handoff for that task until the user resolves it. | M | Combined §4.2 ("must be resolved before any retry"); omm brief §8.6 (reconcile before resubmitting) | Prevents duplicate requests to a business. | Test: put a task in Uncertain; a retry command is refused with a resolve prompt. | T |

---

## 3.5 Data requirements (`DR`)

### 3.5.1 Entity list (v0.9)

From combined §7.3 and omm brief §8.2, with the additions asked for in this SRS. Full field-level dictionary is due at v1.0 (`DR-01`). "Personal" means it holds personal data under PDPA.

| # | Entity | Key content | Personal | MoSCoW |
|---|---|---|---|---|
| E-1 | User | LINE user ID (from a verified event only), nickname for cards, locale, quiet hours, follow status, push count this month | Yes | M |
| E-2 | Consent | User, notice version shown, consent purpose, given at, withdrawn at, age confirmation (20+ or married, `LEG-05`) | Yes | M |
| E-3 | Task (obligation or request) | Owner, type, lifecycle state, evidence level, timestamps | Yes | M |
| E-4 | Obligation detail | Task, payee, amount (integer satang), due instant (UTC), original text reference | Yes | M |
| E-5 | Extracted claim | Task, field, value, confidence, source span, model, prompt and schema versions (`AI-SUP-01`) | Yes | M |
| E-6 | Request card | Task, version, content hash, venue, service, time options, disclosure text, expiry, withdrawn at | Yes | M |
| E-7 | Approval | Task, version, approver, token hash, expires at, used at | Yes | M |
| E-8 | Venue | Name, LINE ID or phone, opening hours: our own hand-checked facts with source note and last-verified date; optional Google place ID only; channel; confidence; stop flag. **No Google Places content; nothing from Wongnai** (`DR-02`, `LEG-10`) | Business contact data | M |
| E-9 | Channel attempt | Card, venue, adapter, handoff opened at, sent confirmed at, outcome, time to answer, team minutes (S2) | Yes | M |
| E-10 | Reminder occurrence | Task, scheduled instant, retry key, attempts, provider response, sent at, acknowledged at, snoozed until | Yes | M |
| E-11 | Job | Type (reminder, follow-up, expiry, withdraw window, retention), run at, status, attempts, idempotency key | No (references only) | M |
| E-12 | Webhook inbox | `webhookEventId` (unique), received at, processed at. No message text | No | M |
| E-13 | Evidence item | Task, kind (slip, screenshot, reply page), object reference, deleted at | Yes | M |
| E-14 | Audit record | Actor, action, decision, references, preview hash, timestamp; minimal content (`SEC-03`) | Yes (minimal) | M |
| E-15 | Rights request | User, type (access, export, delete, withdraw), received at, fulfilled at (`PRV-12`) | Yes | M |
| E-16 | Business reply (S0) | Card, answer (accept, decline, stop), short note, received at, self-answered flag; device-marker hash | Business data | S |
| E-17 | Sharing grant (S1) | Task, grantee, permissions, accepted at, revoked at | Yes | S |
| E-18 | Operator action (S2) | Operator, card, call outcome, minutes, timestamp | Yes | S |
| E-19 | Bill-split participant (C2) | Split, display name, amount in satang, paid flag | Yes (non-users) | C |

### 3.5.2 Data requirements

| ID | Requirement | Pri | Source / trace-up | Rationale | Acceptance criterion | Ver |
|---|---|---|---|---|---|---|
| `DR-01` | The SRS v1.0 shall include a data dictionary listing every stored entity and field with its type, whether it is personal data, its PDPA lawful basis and its retention row from Table R. | M | Requirements file `DR-01`; PDPA s.23 (notice of retention) `[V]` | Retention and notice cannot be right without an inventory. | Inspection: every table in the schema appears in the dictionary; every personal field has a basis and a retention row. | I |
| `DR-02` | Each venue row shall record the source of each contact fact (the shop's own sign, page or OA profile) and the date it was last checked by hand. | M | Combined §4.4, §7.3 Venue; Google Maps Platform ToS s.3.2.3 `[V, via combined §10]`; `SEED-LEG-01` | Proves the venue table holds our own facts, not copied Places content. | Inspection of all 10–15 venue rows: 100% have a source note and a date. | I |
| `DR-03` | Extracted claims shall be stored apart from the confirmed task fields, with their confidence and source span. | M | `SEED-DR-01`; omm brief §8.2, §8.4 | Separates what the model guessed from what the user confirmed. | Schema inspection; test: editing a confirmed field does not change the stored claim. | I, T |
| `DR-04` | The system shall delete or anonymise each data class in Table R no later than the limit in that row, applying the precedence rules below the table. | M | Requirements file `DR-04`; combined §7.4; PDPA s.23, s.37 `[V]`; `TR-03` (author C) | Keeps data only as long as the stated purpose needs. | Retention jobs tested with time-shifted data, one test per row: 100% of expired rows deleted or anonymised; nothing deleted early. | T |
| `DR-05` | After a backup is restored, the system shall re-apply every deletion recorded since that backup was taken before the restored data is used. | M | omm brief §8.9 (deleted data must not reappear after restore); `OPS-02` restore drill | A restore must not bring back data the user asked us to delete. | Restore drill: delete a test user, restore an earlier backup, run the re-apply step: the user's data is absent. | T |
| `DR-06` | Each participant amount in a bill split shall be stored as an integer number of satang. | C | Requirements file `DR-06`; combined C2 | Avoids rounding errors in money. | Schema inspection. | I |
| `DR-07` | Profile and membership information obtained from LINE friend or group APIs shall be deleted within 24 hours of retrieval. | M | Requirements file `DR-07`; LINE User Data Policy s.3.2.3 `[V]` | Platform rule. The prototype does not plan to call these APIs; the rule guards against later use. | Inspection: no such fields persisted beyond a 24-hour cache; retention test if any are added. | I, T |
| `DR-08` | Every obligation amount shall be stored as an integer number of satang. | M | Combined §4.3 step 2 (amounts validated as numbers); E1 amount exact match | Exact amounts are needed for correct reminders and E1 scoring. | Schema inspection; test: "1,245.50 บาท" stored as 124550. | I, T |
| `DR-09` | Every stored instant shall be in UTC, and every date shown to or parsed from the user shall use the Asia/Bangkok time zone. | M | Combined §4.3 step 2; recon P-1 | Avoids off-by-seven-hours reminders. | Test set of 20 date cases, including midnight and Buddhist-era years: 0 shifted instants. | T |
| `DR-10` | After an evidence image is deleted under Table R, the task's evidence level shall be kept and its label shall change to "slip was attached (deleted per retention)". | M | Combined §4.2, §7.4 | The honest record survives deletion of the image. | Retention test on a task with a slip: image gone, evidence level unchanged, new label shown. | T |

### 3.5.3 Table R: retention (consistent with combined §7.4 and the requirements file)

| Row | Data | Retention limit | Basis |
|---|---|---|---|
| R1 | One-to-one chat text | 30 days, then deleted or anonymised | `DR-04` (requirements file) |
| R2 | Extracted claims with source spans | 30 days, then deleted (confirmed task fields are covered by R3) | `DR-04`; combined §7.4 |
| R3 | Tasks, obligation details, reminder occurrences, request-card metadata owned by the user | Until the user deletes them or the account, or `TR-03` end-of-pilot deletion, whichever comes first | `PRV-03`, `TR-03` |
| R4 | Evidence images (slips, screenshots of a shop's chat) | 30 days or until the E6 audit of that task, whichever is **later**, and never beyond the `TR-03` end-of-pilot deletion. Never ID cards or health documents | Combined §7.4; pending fix 3; `PRV-04` |
| R5 | Request-card text, business reply records, reply-page notes, device-marker hashes (S0) | 30 days after the card closes (answered, declined, withdrawn or expired) | Combined §7.4, parallel to `PRV-06` |
| R6 | User nickname on a card or reply page | Shown only while the card is open; after expiry or Withdraw the page shows no personal data (`SEC-06`). Never the LINE ID | Combined §7.4, §7.5 |
| R7 | Venue table, including stop flags | Until the pilot ends, then `TR-03` | Combined §7.4 |
| R8 | Audit records (incl. consent, withdrawal and rights-request records) | 180 days or until the evaluation report is submitted, whichever is **earlier** | `DR-04`, `TR-03` |
| R9 | Webhook inbox rows and completed job rows (no content) | 30 days (proposal) | Proposal; dedupe needs only a short window |
| R10 | Application and access logs (no personal content, tokens redacted) | 30 days (proposal) | `PRV-10`, `SEC-08` |
| R11 | Backups | Rolling, at most 7 days (proposal); restores re-apply deletions (`DR-05`) | Proposal; omm brief §8.9 |
| R12 | LINE friend or group profile information | 24 hours | `DR-07`; LINE User Data Policy s.3.2.3 |
| R13 | Bill-split participants (C2) | Display name and amount only; deleted when settled or 30 days after creation, whichever is earlier | `PRV-06` |
| R14 | Anonymised evaluation data (no personal data) | Until the evaluation report is submitted, then per `TR-03` | `TR-03` precedence |
| R15 | All pilot data | Deleted or anonymised within 30 days of the pilot's end; users told how | `TR-03` |
| R16 | Traffic metadata (source, destination, time) | **Open issue.** If the Computer-Related Crime Act s.26 applies (UNVERIFIED whether we are a "service provider"), keep minimal metadata 90 days and content per R1 | Requirements file open issue 16 `[S]` |

**Precedence rules** (to avoid contradictions):
1. A user's delete request or consent withdrawal (`PRV-03`, `PRV-07`) overrides every "keep until" rule, including R4's E6 condition. Only minimal audit records (R8) are kept, for handling inquiries (LINE User Data Policy s.3.5.2 `[V]`). A task deleted this way is excluded from the E6 audit, and the report says how many were excluded.
2. A shorter limit always wins over a longer one for the same data (R12, R13 and R1 apply during and after the pilot).
3. R15 (`TR-03`) is the outer bound for all personal data. R8 and R14 end no later than report submission, which falls before the R15 deadline in the current plan (pilot ends 11 April 2027, report 30 April 2027).
4. "Whichever is later" appears only in R4, and R4 is capped by R15.

---

## 3.6 Security (`SEC`) and privacy (`PRV`)

### 3.6.1 Threat model summary (STRIDE)

STRIDE: **S**poofing, **T**ampering, **R**epudiation, **I**nformation disclosure, **D**enial of service, **E**levation of privilege. Full threat model is decision D08 (18 Nov); this is the v0.9 summary.

| Surface | Main threats | Controls (requirement IDs) |
|---|---|---|
| LINE webhook (M1) | S: forged events. T: body altered. D: floods and replays. E: a forged postback approving another user's card. R: user denies tapping Confirm | HMAC signature over the raw body (`IF-LINE-01`, author A); inbox dedupe (`QR-RE-01`); approval token bound to user, task, version, hash, single use (`AI-HITL-01`, `SEC-04`); identity only from signed events (`SEC-13`); audit log (`SEC-03`); HTTPS (`SEC-02`) |
| LIFF pages (C5 only) | S: browser-supplied user ID or profile trusted. I: data shown to the wrong user | ID token verified on the server (`IF-LIFF-01`, author A); owner checks (`SEC-12`) |
| Reply page (S0) | S: anyone with the link answers, including the requester. T: token guessing; state change by link previews. R: business denies answering. I: token leaks through referrer, logs or search; personal data after expiry. D: answer spam. E: a token used on another card | Accepted risk, flagged best-effort (`SEC-10`); evidence label says it does not prove identity (combined §4.2); 128-bit hashed card-bound token (`SEC-05`); GET has no side effects (`QR-SE-01`); expiry and blank page after expiry (`SEC-06`); headers (`SEC-07`); token redaction (`SEC-08`); rate limits (`SEC-09`); **no LINE login, no LIFF, no business LINE ID collected** (`PRV-09`) |
| LLM interpreter (M3) | T: prompt injection in user text, business notes, attachments. E: excessive agency. I: leaks of other users' data or secrets through the prompt. D: cost exhaustion. R: untraceable model changes | Untrusted-data handling (`AI-PI-01`, `AI-PI-02`); no acting tools (`AI-BEH-01`, `AI-BEH-04`); minimum context (`PRV-11`); secrets never in context (`SEC-01`); cost limits (`AI-COST-01`, `AI-COST-03`); version logging (`AI-SUP-01`) |
| Operator console (S2) and admin access | S: stolen team credentials. E: operator sees more than the call needs. R: unlogged operator actions. I: export of tester data | Strong authentication and least privilege (`SEC-14`); every action logged (`SEC-03`); separate dev and pilot channels (`TR-01`, author C) |
| Database, backups, logs | I: data at rest or in logs exposed; cross-user reads. T: audit records altered | Encryption at rest (`SEC-11`); owner checks (`SEC-12`); append-only audit (`SEC-03`); no personal content in logs (`PRV-10`); restore re-applies deletions (`DR-05`); secret scanning (`SEC-15`) |

### 3.6.2 Security requirements

| ID | Requirement | Pri | Source / trace-up | Rationale | Acceptance criterion | Ver |
|---|---|---|---|---|---|---|
| `SEC-01` | Secrets (LINE channel secret and access token, model API key, database credentials) shall be held only in the deployment's secret store, never in the repository, logs or model context. | M | Requirements file `SEC-01`; combined M1; omm brief §8.1, §8.5; LINE Developers Agreement §5.2.2 `[V]` | Leaked secrets let attackers send as our OA. | Secret scan of the repository and a week of logs finds 0 secrets; prompt-builder inspection. | I, T |
| `SEC-02` | Every endpoint the system exposes shall accept connections only over HTTPS. | M | omm brief §8.9 (encrypt transport) | Protects tokens and personal data in transit. | Test: plain HTTP requests are refused or redirected without serving content. | T |
| `SEC-03` | Every confirmation, adapter call, state change, operator action and rights request shall be written to an append-only audit log with actor, action, references, preview hash and timestamp, and minimal personal content. | M | Requirements file `SEC-03`; combined §7.2; recon D-6 | Accountability and evidence for E6. | Inspection: the application role has no UPDATE or DELETE on the audit table except the retention job; test: each listed event creates one row. | I, T |
| `SEC-04` | Each approval token shall be random with at least 128 bits, stored only as a hash, valid for one use, and expire after a set time. | M | Combined §4.3 step 3, §7.2 | Prevents replay and guessing of approvals. | Expiry proposal: 24 hours, **TBD after the P2 usability check**. Tests: reused, expired and modified tokens all rejected (part of E3). | T |
| `SEC-05` | Each reply-page token shall be random with at least 128 bits, stored only as a hash, and tied to exactly one request card. | S (S0) | Combined §7.5; `SEED-SEC-01` | Unguessable links are the only access control on the page. | Test: a token for card A cannot read or answer card B; database inspection shows hashes only. | T, I |
| `SEC-06` | After a card expires (end of the last offered slot, at most 7 days) or is withdrawn, the reply page shall show only "This request has expired" or "Withdrawn by sender", with no personal data. | S (S0) | Combined §7.5 | Old links must not keep exposing nickname or times. | Test on expired and withdrawn cards: the response body contains no nickname, service or time options. | T |
| `SEC-07` | The reply page shall send `Referrer-Policy: no-referrer`, a `noindex` header and meta tag, and a Content Security Policy that allows only our own scripts, and shall load no third-party scripts, analytics or fonts. | S (S0) | Combined §7.5 | Stops token leaks through referrers, search engines and third parties. | Header test on the deployed page; inspection of the page source for external hosts: 0 found. | T, I |
| `SEC-08` | Tokens shall never be written to logs; request paths that contain tokens shall be redacted in access logs. | M | Combined §7.5; omm brief §8.9 | Logs are read by more people than the database. | Log inspection after a test session: 0 full tokens found. | I |
| `SEC-09` | The reply page shall enforce rate limits per card and per IP address. | S (S0) | Combined §7.5 | Limits answer spam and token guessing. | Proposed limits: 10 POSTs per card per hour; 60 requests per IP per minute (**TBD after spike**). Test: requests above the limit get HTTP 429 and no state change. | T |
| `SEC-10` | When the requester previews their own card, the reply page shall set a first-party marker cookie on that device, and any answer posted from a browser carrying that marker shall be stored with the self-answered flag. | S (S0) | Pending fix 2; combined §4.2, §7.5, E11 | Self-answers must not count as business answers. Best effort only: another device or a cleared browser is not caught, and the report says so. | Test: answer from the requester's browser is flagged and excluded from E11; answer from a clean browser is not flagged. | T |
| `SEC-11` | The database and object storage shall be encrypted at rest with provider-managed keys. | M | omm brief §8.2, §8.9 | Limits harm from a lost disk or snapshot. | Inspection of the hosting configuration. | I |
| `SEC-12` | Every read or write of user data shall be filtered in server code by the verified owner (or, with S1, by an active sharing grant). | M | Combined M5, E7; omm brief §8.3, §8.5 | Prevents one tester seeing another's tasks. | E7 cross-user tests (≥ 20 cases, plus revoked-share cases if S1 is built): 0 unauthorized reads. | T |
| `SEC-13` | The server shall take a LINE user's identity only from a signature-verified webhook event or, for LIFF pages, from a server-verified ID token. | M | omm brief §8.1 `[V]`; combined §7.3 User | Browser-supplied IDs can be forged. | Code inspection; test: a request with a forged user ID in the body is rejected. | I, T |
| `SEC-14` | The operator console shall require team accounts with two-factor sign-in and shall show an operator only the requests assigned for a call and the fields needed for that call. | S (S2) | Combined S2, §7.1; omm brief §8.9 (least-privilege admin access) | The console is the widest door into tester data. | Demonstration: sign-in without the second factor fails; an operator view lists only assigned requests. | D |
| `SEC-15` | The CI pipeline shall run secret scanning and dependency vulnerability scanning on every merge to the main branch. | S | Requirements file §1.3.8 verify note; omm brief §8.10 | Cheap early detection for a part-time team. | Inspection of CI configuration and one run log. | I |

### 3.6.3 Privacy requirements

| ID | Requirement | Pri | Source / trace-up | Rationale | Acceptance criterion | Ver |
|---|---|---|---|---|---|---|
| `PRV-01` | Before first use, the system shall show a privacy notice covering purposes, data collected, retention (Table R), recipients (the model provider abroad, and businesses that receive cards), AI disclosure and user rights. | M | Requirements file `PRV-01`; PDPA s.23 `[V]`; `LEG-01` | Users must know what happens to their data before it happens. | Inspection of the notice (Thai and English) against this list; test: no task is processed before the notice is acknowledged. | I, T |
| `PRV-02` | The team shall record which PDPA s.28 basis covers sending prompts to the model provider outside Thailand, and the notice shall say that the transfer happens. | M | Requirements file `PRV-02`; PDPA s.28 `[V]`; combined §7.1 (D06) | Cross-border transfer needs a recorded basis. Which basis applies is UNVERIFIED. | Inspection: basis recorded in the compliance checklist with the adviser's note; notice text names the transfer. | I |
| `PRV-03` | A user shall be able to request a copy (in a machine-readable format) or deletion of their data by chat command, and the team shall fulfil either request within 30 days. | M | Requirements file `PRV-03`; combined M5; PDPA s.30 (access, 30 days) `[V]`, s.31 (portability) `[V]`, s.33 (erasure; PDPC AI guideline cites 90 days, we choose 30) `[V]`; recon P-8 | PDPA rights, handled cheaply by command plus team script. | Test: export and delete commands on a test user; the export is JSON with all R3 data; after delete, only R8 audit rows remain; rights-request log shows fulfilment within 30 days. | T |
| `PRV-04` | The system shall not store images of ID cards or health documents beyond the task that needed them. | M | Requirements file `PRV-04`; PDPA s.26 `[V]` | Sensitive data needs extra legal grounds we do not have. | Inspection of evidence handling; the consent pack and in-chat prompt tell testers not to send them; any such image found in the E6 audit is deleted and logged. | I |
| `PRV-05` | Chat logs shall not be used to train or fine-tune any model; the prototype offers no training opt-in, and use of the assistant shall never depend on one. | M | Requirements file `PRV-05`; PDPC AI guideline (no bundled consent) `[V]`; `AI-SUP-02` | Keeps consent unbundled and data use narrow. | Inspection of code, provider settings and the notice. | I |
| `PRV-06` | Data about bill-split participants who are not users shall be limited to display name and amount owed, and deleted per Table R row R13. | C | Requirements file `PRV-06`; combined C2 | Non-users are data subjects who never agreed to our terms. | Schema inspection and retention test (if C2 is built). | I, T |
| `PRV-07` | The user shall be able to withdraw consent and leave the service by chat command, after which the system shall stop processing that user's data and delete all LINE user information, including the LINE user ID, except minimal audit records kept for inquiries (R8). | M | Requirements file `PRV-07`; PDPA s.19 `[V]`; LINE User Data Policy s.3.2.10, s.3.5.2 `[V]` | Withdrawal must be as easy as consent. | Test: withdraw command; pending jobs cancelled; user row and tasks deleted; only R8 rows remain. | T |
| `PRV-08` | Where consent is the lawful basis, it shall be requested on its own screen, separate from other terms, in Thai or English, with the purpose stated. | M | Requirements file `PRV-08`; PDPA s.19 `[V]`; combined §9.3 | Valid consent must be specific and unbundled. | Inspection of the onboarding flow. | I |
| `PRV-09` | The reply page shall collect no data about the business person other than the answer, an optional short note and the time received, and shall not use LINE Login or LIFF. | S (S0) | Pending fix 2; combined §7.5, `STK-03` (no sign-up); combined §9.3 (businesses are third-party data subjects) | Minimises data about people who never agreed to our terms. | Inspection of the page and the Business reply schema; network capture shows no LINE Login call. | I, T |
| `PRV-10` | Routine application logs shall contain no message text, nicknames, phone numbers or image content. | M | Combined M1, §7.5; omm brief §8.9 | Logs are kept and read widely. | Log inspection after a scripted test session: 0 matches for the test session's message text, nickname and phone number. | I, T |
| `PRV-11` | The system shall send to the model only the current message text, the current task's draft fields and the current date and time, and never LINE user IDs, other tasks or other users' data. | M | omm brief §8.5 (minimum context); PDPC AI guideline (prompts can be personal data) `[S]`; OWASP LLM02 | Limits what is transferred abroad and what an injection could leak. | Prompt-builder inspection; test: captured prompts for 20 messages contain no LINE user ID and no other task's data. | I, T |
| `PRV-12` | The team shall record every PDPA rights request with its type, date received and date fulfilled. | M | PDPA s.30, s.33 `[V]`; omm brief §7 (rights-request workflow) | Shows the 30-day limit is met. | Inspection of the rights-request table after the `PRV-03` test. | I |

### 3.6.4 PDPA rights handling (summary)

| Right (PDPA) | How the user asks | Who acts | Time limit | Requirement |
|---|---|---|---|---|
| Notice (s.23) | Shown before first use; link in the rich menu | System | Before processing | `PRV-01`, `LEG-01` |
| Consent and withdrawal (s.19) | Consent screen; "withdraw" chat command | System | Immediate stop | `PRV-08`, `PRV-07` |
| Access and copy (s.30), portability (s.31) | "export" chat command | Team script | 30 days | `PRV-03`, `PRV-12` |
| Erasure (s.33) | "delete" chat command | Team script | 30 days (stricter than the 90 days in the PDPC AI guideline) | `PRV-03`, `DR-05` |
| Breach notice to the Office (s.37(4)) | n/a | Team | Within 72 hours where feasible | `OPS-04` (author C); `LEG-12` |

---

## 3.7 Compliance (`LEG`) checklist, v0.9

Each row is a compliance requirement: **the system (or the team) shall meet the rule in the "Rule" column**, satisfied by the listed requirements. Priority is Must unless marked. Verification: **I** (checklist review by the team and advisor before the pilot) unless marked. Status at v0.9: all **Open**; sign-off is due at v1.0 (requirements file §3).

| ID | Rule (source) | Our requirement | Satisfied by | Ver | ☐ |
|---|---|---|---|---|---|
| `LEG-01` | Privacy policy viewable at any time, before and after use (LINE User Data Policy s.2.2, s.2.4 `[V]`; PDPA s.23 `[V]`) | The service shall publish a privacy policy reachable from the rich menu and the welcome message. | `PRV-01` | I, D | ☐ |
| `LEG-02` | No proxy sending via the user's account against their wishes; show details first (LINE User Data Policy s.3.3 `[V]`; directly for LIFF send/share, by analogy for bot messages) | The system shall send nothing on the user's behalf without the user's confirmed tap on the exact content. | `AI-HITL-01`; handoff is sent by the user's own tap | I, T | ☐ |
| `LEG-03` | Payment Systems Act and e-money rules `[S, omm brief §7]` | The system shall not hold or move money; any PromptPay QR (C2) shall pay the payer's own account. | Design (no payment adapter); `DR-06`; C2 scope | I | ☐ |
| `LEG-04` | University research-ethics approval (process UNVERIFIED) | The team shall obtain the approvals the university requires before any pilot, with an application that covers testers **and businesses that receive cards** as third-party data subjects. | Combined M6, §9.3 | I | ☐ |
| `LEG-05` | Majority at 20 or by marriage (Civil and Commercial Code s.19, s.20 `[S]`); PDPA s.20 minors' consent `[V]` | Pilot testers shall be aged 20 or over, or married (sui juris). | Age confirmation stored in Consent (E-2); recruitment screening | I | ☐ |
| `LEG-06` | ETA s.15 para 2(2), s.16, s.17 `[V, ETDA translation]`: messages from a system programmed to act for the user are the user's, and the receiver may rely on them | The user shall see and confirm the exact content before any message leaves. Whether our bot counts as the user's system needs legal review. | `AI-HITL-01`, `AI-HAL-01`, `SEC-03` | I, T | ☐ |
| `LEG-07` | ETA s.17/1 `[V, ETA No. 3 translation]`: input-error withdrawal where no correction channel exists (used as design basis, not a claim of coverage) | Every confirmation card shall offer Edit and Cancel, and for 10 minutes after a confirmed request that has no known answer the user shall be able to Withdraw it, with the system stating that a chat message already sent can only be unsent by the user in LINE. | Flow "Withdraw" (combined §7.2); `AI-HITL-02` | T | ☐ |
| `LEG-08` | PDPC AI guideline (advisory, 7 Oct 2026) `[V]`: disclose the automated system from the start | Disclosure in welcome message, notice, every card and reply page. | `AI-EXP-01` | I | ☐ |
| `LEG-09` | Draft Thai AI Act: **not enacted** as of 9 Nov 2026 `[S]` | The team shall re-check its status at each phase gate and, if enacted, check whether the reported research carve-out applies. | Phase-gate checklist | I | ☐ |
| `LEG-10` | Google Maps Platform ToS s.3.2.3 and Places policies; Wongnai ToS item (3) `[V, via combined §10]` | No Google Places or Wongnai content shall be stored in the venue table, test sets or non-Google maps; at most a Google place ID is stored, and Places details, if shown, are fetched at display time with attribution. | `DR-02`; `SEED-LEG-01`; evaluation-set rule (§3.3.3) | I | ☐ |
| `LEG-11` | Computer-Related Crime Act s.11 (unwanted messages) `[S, via jui legal file]` | Every request card shall offer the business a way to stop further requests, and a Stop answer shall block further cards to that venue. | Venue stop flag (E-8); reply-page Stop (`SEED-FR-07`, author A); in-chat stop wording for handoff cards | T | ☐ |
| `LEG-12` | PDPA s.37(4) `[V]`: breach notice to the Office within 72 hours where feasible | The team shall follow an incident procedure that includes the 72-hour notice step. | `OPS-04` (author C) | I | ☐ |
| `LEG-13` | PDPA s.28 `[V]`: cross-border transfer | Transfer basis recorded; provider no-training and zero-retention terms filed. | `PRV-02`, `AI-SUP-02` | I | ☐ |
| `LEG-14` | LINE User Data Policy s.3.2.3, s.3.2.10, s.3.5.2 `[V]` | Friend and group information deleted within 24 hours; LINE user information deleted when a user leaves, except inquiry records. | `DR-07`, `PRV-07` | I, T | ☐ |
| `LEG-15` | PDPA s.19, s.26, s.30, s.33 `[V]` | Unbundled consent; no health data; access and erasure within 30 days. | `PRV-08`, `PRV-04`, `AI-SAFE-03`, `PRV-03` | I, T | ☐ |
| `LEG-16` | Computer-Related Crime Act s.26 `[S]`: traffic data kept ≥ 90 days by service providers | **Open issue.** The team shall get advice on whether the prototype is a "service provider" before the pilot, and apply Table R row R16 if it is. | Table R16 | I | ☐ |
| `LEG-17` | LINE Developers Agreement §5.2.2 `[V]`: store user data securely; report unauthorised access | User data shall be protected by the §3.6 controls, and unauthorised access shall be reported to LINE as part of the incident procedure. | `SEC-01`, `SEC-11`, `SEC-12`; `OPS-04` | I | ☐ |

Not applicable at v0.9 (recorded so they are not forgotten):
- **Digital Platform Services Royal Decree:** a non-commercial university prototype is unlikely to be a platform business `[I, omm brief §7; not legal advice]`. Re-check if the project continues after April 2027.
- **Google API Services User Data Policy:** no Google OAuth in scope (Google Calendar is Won't).

---

## Counts

| Prefix | Count | IDs |
|---|---:|---|
| AI | 24 | BEH-01–04, SAFE-02, SAFE-03, HITL-01–02, EVAL-01–04, HAL-01–03, PI-01–02, EXP-01–02, COST-01–03, SUP-01–02 (AI-SAFE-01 reserved, not used) |
| QR | 21 | FS-01–02, PE-01–03, CO-01–02, IC-02, RE-01–07, SE-01, MA-01–02, FL-01, SA-01–02 |
| DR | 10 | DR-01–10 (plus entity list and Table R) |
| SEC | 15 | SEC-01–15 |
| PRV | 12 | PRV-01–12 |
| LEG | 17 | LEG-01–17 |
| **Total** | **99** | |

## Open issues for this part

1. **Size of the prompt-injection set.** The run brief asked for about 20 injection items. The judge-passed sources (requirements file `AI-PI-02`, combined E4 and M3) say at least 30. This part keeps **30** so that the hours already costed in M3 and the frozen E4 target stay consistent. The total of about 150 items is 100 labelled + 20 ambiguous + 30 injection. Team to confirm.
2. **Thresholds still TBD after spike:** request-card slot pass marks (`AI-EVAL-03`), the confidence threshold for asking (`AI-HAL-02`), the daily model-call limit (`AI-COST-01`), approval-token expiry (`SEC-04`), reply-page rate limits (`SEC-09`) and `QR-FS-01` for request cards.
3. **Quiet hours versus `FR-REM-01`.** `QR-RE-04` lets a reminder the user set for an exact time fire inside quiet hours, and defers only system-timed pushes. Author A should state the same rule in the reminder FRs, or the team should choose otherwise.
4. **Computer-Related Crime Act s.26** applicability (`LEG-16`, R16) is UNVERIFIED. Get advice before the pilot.
5. **PDPA s.28 basis** for the model transfer abroad (`PRV-02`) is UNVERIFIED. Ask the advisor or the university legal office.
6. **Link-preview behaviour** of LINE for our URLs is UNVERIFIED. `QR-SE-01` makes it harmless either way; the test will show what actually happens.
7. **Backup retention (R11, 7 days)** and log retention (R9, R10, 30 days) are new proposals not in combined §7.4. They need team agreement and the privacy notice must state them.
8. **Cross-author references.** This part relies on `IF-LINE-01`, `IF-LIFF-01`, `FR-ACT-01`, `FR-REM-01`, `SEED-FR-07` (author A) and `CON-BUD-01`, `TR-01`, `TR-03`, `OPS-02`, `OPS-04`, `VER-02`, `VER-03` (author C). The integrator should check those IDs exist with the meanings assumed here.
9. **Requirements-file IDs reused with narrowed scope:** `AI-HITL-01` (calendar writes removed, because Google Calendar is Won't), `LEG-07` (calendar-event deletion removed; handoff unsend wording added from combined §7.2), `PRV-03` and `PRV-07` (chat command instead of a settings page, per combined M5). The core meaning is unchanged.
