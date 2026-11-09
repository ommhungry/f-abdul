# Requirement types for our LINE AI assistant: research and recommendation

- Date: 9 November 2026. All web sources accessed on 9 November 2026 unless stated.
- Author: requirements-engineering researcher (review run of 9 November 2026).
- Question from the team: "Now that we are back to software only, are Functional and Non-Functional Requirements enough, or do we need more kinds of requirements? Please cover all the requirements."
- Evidence tags: **[V]** verified from a primary source I opened (or the official preview/sample of a paid standard). **[S]** secondary source (summary, vendor page, study guide, law-firm note). **UNVERIFIED** not checked. **[I]** our inference. Targets and numbers written for our product are **proposals for the team to agree**, not facts.
- Product scope note: the three member directions (omm, vj, jui; see `docs/review/TEAM-BRIEF.md`) are not merged yet. The example requirements below use actions that appear in at least one direction: reminders, calendar events, group polls, bill split with a PromptPay QR code, document reading, and "request cards" sent to a business. Swap the examples once the combined scope is fixed.

---

## 0. Short answer

**No. Functional and non-functional requirements (FR + NFR) are the right top split for what the software must do and how well, but on their own they are not enough for this project.**

Why:

1. **The standards themselves use more categories.** The requirements-engineering standard ISO/IEC/IEEE 29148:2018 defines separate business, stakeholder, system and software requirement documents, and its software requirements specification (SRS) has separate sections for external interfaces, logical database (data), design constraints, standards compliance and verification, not just "functions" and "attributes" [V, 29148 contents]. The business-analysis guide BABOK v3 puts functional and non-functional requirements *inside* one of four groups, next to business, stakeholder and transition requirements [S].
2. **"NFR" is a catch-all that hides things that fail differently.** A rule from Thai law, a LINE platform limit, a three-person team budget and a 99% uptime target are all "non-functional", but they come from different owners and are checked in different ways. Mixing them makes it easy to forget one.
3. **Our product is an AI that acts for the user.** Large language model (LLM) output is probabilistic, so "the bot shall understand the request" is not testable. We need AI requirements with test sets and pass marks. Acting on the user's behalf is a named risk ("Excessive Agency", OWASP LLM06:2025) [V], LINE's User Data Policy s.3.3 forbids proxy sending or posting *via the user's account* against the user's wishes and requires showing the details first [V]. That applies directly to any LIFF send or share feature we build; we apply it by analogy to messages our Official Account sends to third parties for the user. Both need explicit requirements.
4. **Thai law and LINE policy add hard rules.** The Personal Data Protection Act (PDPA) adds consent, notice, deletion, breach-notice and cross-border-transfer duties [V, unofficial English translation]. Under the Electronic Transactions Act, a message sent by a system programmed to act automatically for the user is deemed the user's own (s.15 para 2(2)), and the receiver may treat it as what the user intended (s.17; see also s.16) [V, ETDA translation], and a user who makes an input error in an automated system with no way to correct it may withdraw it (s.17/1) [V]. That is the legal reason for confirm-before-acting and Edit/undo. The PDPC's AI guideline of 7 Oct 2026 (advisory) asks for chatbot disclosure and human-in-the-loop [V]. Thailand's AI Act is still a draft [S]. These are constraints, not optional quality goals.
5. **We are graded on deliverables.** The proposal (22 Nov 2026) and prototype (30 Apr 2027) have document and evaluation requirements that are not product features at all.

**Recommendation:** keep FR and NFR as two of the families, but write the SRS with **16 categories** (Section 1). For three students at 3-4 hours a week, most categories need only a handful of requirements each. The point is to *not forget* a category, not to fill pages.

---

## 1. The requirement categories we need

### 1.1 What the sources say (summary table)

| Source | What it says about requirement types | Tag |
|---|---|---|
| **ISO/IEC/IEEE 29148:2018** (2nd edition, Nov 2018), *Systems and software engineering — Life cycle processes — Requirements engineering* | Four requirement documents: Business Requirements Specification (BRS, cl. 8.2/9.3), Stakeholder Requirements Specification (StRS, 9.4, includes "User requirements" 9.4.15), System Requirements Specification (SyRS, 9.5), Software Requirements Specification (SRS, 9.6). SRS content: purpose, scope, product perspective, product functions, user characteristics, limitations, assumptions and dependencies, apportioning of requirements, specified requirements, **external interfaces, functions, usability, performance, logical database, design constraints, standards compliance, software system attributes, verification**, supporting information. SyRS adds **security, information management, policy and regulation, life-cycle sustainment**. Clause 5.2.5-5.2.8: characteristics of individual requirements and of sets, requirement language, requirement attributes. It replaced the 2011 edition. | [V] table of contents and scope from the official iTeh preview of the standard: https://standards.iteh.ai/catalog/standards/iso/8cf2bc2b-8b5e-4907-a82a-d1c5676c9e85/iso-iec-ieee-29148-2018 |
| **IEEE 830-1998** (Recommended Practice for SRS) | Historic SRS layout: 1 Introduction, 2 Overall description, 3 Specific requirements (external interfaces, functions, performance, logical database, design constraints, software system attributes). Superseded by ISO/IEC/IEEE 29148:2011, now 29148:2018. Many university templates still use its three-part layout. | Supersession [V] IEEE SA page https://standards.ieee.org/standard/830-1998.html (via search result). Section layout [S] from memory and study guides; the 29148:2018 SRS headings above match it closely. |
| **IIBA BABOK Guide v3** (2015) | Requirements classification schema: **business** requirements (why the change), **stakeholder** requirements (needs of a stakeholder group), **solution** requirements split into **functional** and **non-functional**, and **transition** requirements (temporary capabilities to move from current to future state: data conversion, training, continuity; obsolete after cutover). | [S] The official IIBA page is behind a login. Consistent across study guides, e.g. https://businessanalyst.techcanvass.com/types-of-requirements-as-per-babok |
| **ISO/IEC 25010:2023** (2nd edition) product quality model | Nine characteristics (see Section 1.3.4). Foreword lists the changes from 2011: **Safety added** (sub-characteristics operational constraint, risk identification, fail safe, hazard warning, safe integration); **Usability → Interaction capability; Portability → Flexibility**; new sub-characteristics **inclusivity, self-descriptiveness** (interaction capability), **resistance** (security), **scalability** (flexibility); **user interface aesthetics → user engagement; maturity → faultlessness**; **accessibility split into inclusivity and user assistance**. The quality-in-use model moved to **ISO/IEC 25019**, the overview to ISO/IEC 25002. | [V] Foreword of the FDIS preview, https://standards.iteh.ai (ISO/IEC FDIS 25010:2023 sample). Sub-characteristic names [S] https://iso25000.com/en/iso-25000-standards/iso-25010 |
| **ISO/IEC 25059:2023** (1st edition, published 2023-06-28) *Quality model for AI systems* | An extension of the SQuaRE quality models for AI. Adds **functional adaptability** (under functional suitability), modifies **functional correctness** (ML is not correct in all cases, so measure correctness and incorrectness), adds **user controllability** and **transparency** (under usability, now interaction capability), **robustness** (under reliability), **intervenability** (under security). ISO marks it "to be revised"; a 2nd edition (ISO/IEC FDIS 25059, "Quality models for AI systems") is in progress. | Title, date, status [V] https://committee.iso.org/standard/80655.html. Characteristic list [S] https://iso25000.com/index.php/en/iso-25000-standards/iso-25059 and https://adamleonsmith.substack.com/p/isoiec-25059-gets-a-rewrite-ai-quality. Status of 2nd edition UNVERIFIED (iso.org page blocked). |
| **Volere Requirements Specification Template** (edition 20, Robertson & Robertson) | 27 sections. Beyond functional: mandated constraints; naming conventions; facts and assumptions; scope of the work; business data model and data dictionary; look and feel; usability and humanity; performance; operational and environmental; maintainability and support; security; **cultural**; **compliance**; open issues; off-the-shelf solutions; new problems; tasks; **migration to the new product**; risks; costs; **user documentation and training**; waiting room; ideas for solutions. Each requirement carries a **fit criterion** (a measurable test). | [V] https://www.volere.org/templates/volere-requirements-specification-template/. Snow-card field list [S] (shown only as an image). |
| **NIST AI RMF 1.0** (NIST AI 100-1, 26 Jan 2023) | Four functions: Govern, Map, Measure, Manage. Seven characteristics of trustworthy AI: valid and reliable; safe; secure and resilient; accountable and transparent; explainable and interpretable; privacy-enhanced; fair with harmful bias managed. NIST says the framework is being revised (no date given). | [V] https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.100-1.pdf (section 3) and https://www.nist.gov/itl/ai-risk-management-framework |
| **NIST AI 600-1** Generative AI Profile (July 2024) | Twelve generative-AI risks: CBRN information; **confabulation** (confident but false output); dangerous, violent or hateful content; **data privacy**; environmental impacts; harmful bias or homogenization; **human-AI configuration**; information integrity; **information security**; intellectual property; obscene or abusive content; **value chain and component integration**. | [V] https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.600-1.pdf (section 2) |
| **OWASP Top 10 for LLM Applications 2025** (current version) | LLM01 Prompt Injection; LLM02 Sensitive Information Disclosure; LLM03 Supply Chain; LLM04 Data and Model Poisoning; LLM05 Improper Output Handling; **LLM06 Excessive Agency**; LLM07 System Prompt Leakage; LLM08 Vector and Embedding Weaknesses; LLM09 Misinformation; **LLM10 Unbounded Consumption** (cost). | [V] https://genai.owasp.org/llm-top-10/ |
| **ISO/IEC 42001:2023** (Dec 2023) AI management system | Requirements for an *organisation's* AI management system (policy, risk and impact assessment, lifecycle controls). It is not a product-requirements standard. We borrow two ideas: a short AI impact assessment and documented AI lifecycle records. We do not seek certification. | Title, date [V] https://committee.iso.org/standard/81230.html. Annex A control names UNVERIFIED (paid text). |
| **Thai PDPA**, B.E. 2562 (2019) | Consent (s.19), minors (s.20), notice (s.23), lawful bases (s.24), sensitive data (s.26), cross-border transfer (s.28), access within 30 days (s.30), portability (s.31), objection (s.32), erasure (s.33), security, deletion system and **breach notice to the Office within 72 hours where feasible** (s.37), records of processing (s.39), processor duties (s.40), data protection officer (s.41), fines (s.82-90). | [V] unofficial English translation hosted by KMUTT: https://cc.kmutt.ac.th/Files/Act%20Eng/personal-data-protection-act-2019-en.pdf. Official text is Thai (Royal Gazette). |
| **Thai Electronic Transactions Act (ETA)** B.E. 2544 (2001), amended 2008 and 2019 (No. 3 and No. 4) | s.7 data messages are not denied legal effect only because they are electronic; s.8-9 writing and e-signatures (amended 2019). **Automated systems:** s.13 covers offer and acceptance by data message. **s.15 para 2(2)** — as between originator and addressee, a data message sent by "an information system programmed to operate automatically in advance, by the originator or a person authorized to act on behalf of the originator" is deemed the originator's. **s.16** — when the addressee may regard a message as the originator's and act on it. **s.17** — in the case of s.15 or s.16 para 1, the addressee may regard the message "as being what the originator intended to send" and act on it, unless it knew or should have known of a transmission error. ETA No. 3 (2019) added a definition of "automated electronic message system", s.13/2 (contracts formed by automated systems are not invalid only because no person intervened), and **s.17/1** (a natural person who makes an input error through another party's automated system that "does not provide the person with a channel for correcting the resulting error" may withdraw the erroneous part, if they notify promptly and have not materially used or benefited from what they received). A revised draft ETA was under public hearing 12 May-15 June 2026 and also covers input errors. | s.13/2, s.17/1 and the definition [V] ETA No. 3, unofficial English translation, MDES: https://www.mdes.go.th/law/detail/5099-ELECTRONIC-TRANSACTIONS-ACT--NO-3---B-E-2562-2019- . s.13-17 [V] ETDA's English translation of ETA B.E. 2544 (for information only; the Thai text is authoritative), pp. 5-6: https://etda.or.th/getattachment/8faa736b-3235-49c8-8b01-d37ff53a9a45/ENG-Version.aspx . s.7-9 [S]. 2026 draft ETA [S] Tilleke & Gibbins, https://www.tilleke.com/insights/thailand-set-to-overhaul-its-e-transactions-framework/ |
| **Thai draft Artificial Intelligence Act** (ETDA) | **Not enacted** as of 9 Nov 2026. ETDA released a revised draft on 2 July 2026; public hearing closed 14 August 2026; next steps are revision, Cabinet approval in principle and Parliament. Risk-based: *prohibited* AI (e.g. subliminal manipulation, unfair broad-scale discrimination), *high-risk* AI designated by royal decree (security, health, environment, energy, telecoms, transport, utilities), and *designated* AI needing notification, registration or licence. Transparency: machine-readable marks on AI-generated images/audio/video; disclosure of AI-generated content on sensitive topics; platform labelling duties. No general chatbot-disclosure duty reported. High-risk deployers: risk management, human oversight staff, operational logs kept ≥ 6 months, incident notice. Joint strict liability; fines reported at ฿1-5 million. Extraterritorial; foreign providers need a local representative. **Carve-outs reported for educational research by higher-education institutions with ethics-committee approval, and for R&D before distribution or service.** | [S] Tilleke & Gibbins via Legal 500 (3 Jul 2026): https://www.legal500.com/intelligence/thailand/media-telecoms-it-entertainment/thailand-releases-new-draft-artificial-intelligence-act ; Baker McKenzie (16 Jul 2026): https://www.bakermckenzie.com/en/insight/publications/2026/07/thailand-draft-ai-act--seven-key-implications ; status after hearing [S] Enersys (1 Oct 2026): https://enersys.co.th/en/insights/thailand-ai-law-status-what-applies-now-2026 . Draft text on the Council of State portal not opened. |
| **PDPC AI guideline** — ประกาศคณะกรรมการคุ้มครองข้อมูลส่วนบุคคล เรื่อง แนวทางการดำเนินการด้านการคุ้มครองข้อมูลส่วนบุคคลเกี่ยวกับการพัฒนาและการใช้งานเทคโนโลยีปัญญาประดิษฐ์ พ.ศ. 2569 (Guidelines on personal data protection in the development and use of AI, 2026) | Issued by the Personal Data Protection Committee under PDPA s.16(3), prepared with ETDA; posted 7 Oct 2026 (B.E. 2569). The document itself says it is **preliminary advice with no legal force and no binding effect** on findings of offence. Points relevant to us: AI should not run fully automatically in high-risk cases — use **human-in-the-loop** so a person can intervene, review or stop a wrong AI decision promptly; **tell users from the start that they are talking to an automated system (chatbot)**; do not bundle consent (e.g. do not require consent to train on chat logs as a condition of using a chatbot); right of access should cover meaningful information about the logic; erasure in RAG systems by deleting the person's index entries; where erasure under s.33 cannot be done "within 90 days" for data inside a model, use output filtering or guardrails. Secondary sources add: prompts, embeddings and outputs can be personal data; vendor contracts should bar training on client data; 72-hour breach clause with processors. Reported Royal Gazette publication 2 Nov 2026. | Official page [V] https://www.pdpc.or.th/28888/ ; guideline PDF [V, read in Thai] https://www.pdpc.or.th/wp-content/uploads/2026/10/v2.pdf (cover still reads "Draft of Guidelines" in English). Article of 14 Oct 2026 [S] Tilleke & Gibbins: https://www.tilleke.com/insights/thailand-issues-personal-data-protection-guidelines-for-ai-systems/ . Gazette date [S] TRAINIAC (4 Nov 2026), UNVERIFIED against the Gazette. |
| **Thai Computer-Related Crime Act** B.E. 2550 (2007, amended 2017) s.26 | Service providers keep computer traffic data for at least 90 days. Whether a student prototype counts as a "service provider" is UNVERIFIED. | [S] UNODC fiche and Tilleke & Gibbins summaries via search. |
| **LINE platform** (LY Corporation) | LINE Developers Agreement (updated 3 Feb 2025); LINE User Data Policy; LINE Official Account Terms of Use; LINE MINI App Policy. Technical rules in the Messaging API and LIFF docs. See Section 1.3.9. | [V] https://developers.line.biz/en/terms-and-policies/ and linked documents |
| **WCAG 2.2** (W3C Recommendation; current dated version 12 Dec 2024) | New success criteria: 2.4.11 Focus Not Obscured (Minimum, AA), 2.4.12 (AAA), 2.4.13 Focus Appearance (AAA), 2.5.7 Dragging Movements (AA), **2.5.8 Target Size (Minimum, AA)**, 3.2.6 Consistent Help (A), 3.3.7 Redundant Entry (A), **3.3.8 Accessible Authentication (Minimum, AA)**, 3.3.9 (AAA). 4.1.1 Parsing removed. | [V] https://www.w3.org/TR/WCAG22/ |

### 1.2 The 16 categories at a glance

| # | Category | ID prefix | Mainly from | Lives in |
|---|---|---|---|---|
| 1 | Business requirements | `BR` | BABOK, 29148 BRS | SRS §1 (short) |
| 2 | Stakeholder and user requirements (user stories, use cases) | `STK`, `US`, `UC` | BABOK, 29148 StRS | SRS §2 |
| 3 | Functional requirements | `FR` | all | SRS §3.2 |
| 4 | Quality (non-functional) requirements by ISO 25010:2023 | `QR-<code>` | ISO 25010, 25059 | SRS §3.4 |
| 5 | External interface requirements | `IF` | 29148 9.6.11, IEEE 830 | SRS §3.1 |
| 6 | Data requirements | `DR` | 29148 9.6.15, Volere §7 | SRS §3.5 |
| 7 | AI-specific requirements | `AI` | ISO 25059, NIST AI RMF and 600-1, OWASP LLM | SRS §3.3 |
| 8 | Security and privacy requirements | `SEC`, `PRV` | ISO 25010 security, OWASP, PDPA | SRS §3.6 |
| 9 | Legal, regulatory, platform-policy compliance | `LEG` | 29148 9.5.15/9.6.17, Volere §17, PDPA, LINE | SRS §3.7 |
| 10 | Constraints (time, team, budget, platform, technology) | `CON` | 29148 9.6.16, Volere §3 | SRS §2.5 |
| 11 | Transition and deployment requirements | `TR` | BABOK, Volere §22 | SRS §3.8 |
| 12 | Operational and support requirements | `OPS` | 29148 9.5.9, Volere §13-14 | SRS §3.9 |
| 13 | Usability, localisation (Thai/English) and accessibility | `UX`, `L10N`, `A11Y` | ISO 25010 interaction capability, Volere §11, §16, WCAG 2.2 | SRS §3.4.4 (own subsection) |
| 14 | Documentation and academic deliverable requirements | `DOC` | Volere §25, 29148 project constraints, course rules | Project plan, SRS appendix |
| 15 | Acceptance criteria and verification requirements | `VER` (plus an attribute on every requirement) | 29148 9.6.19, Volere fit criterion | SRS §4 |
| 16 | Assumptions, dependencies and out-of-scope (not requirements, but recorded) | `ASM`, `DEP`, `OOS` | 29148 9.6.8, Volere §5, §26 | SRS §2.6 |

Category 16 is not a requirement type. It is listed because a requirement that rests on a false assumption fails silently. For example, "the user can forward a request card to a business's Official Account" is still UNVERIFIED in the jui proposal.

### 1.3 Each category in detail

Format for each: **Definition. Why it matters for us. Source. Example requirements (for this product). How to verify.**
"Shall" marks a binding requirement. Numbers are proposals.

#### 1.3.1 Business requirements (`BR`)

- **Definition.** The goals and measurable outcomes that justify building the product at all. They say *why*, not *what*. For a student project the "business" is partly the course: the goal is a working, evaluated prototype.
- **Why it matters.** It stops scope creep. Every feature must trace up to a business requirement or be cut. Our three member directions differ mainly at this level (obligations vs booking vs broad actions), so this is where the merge decision gets written down.
- **Source.** BABOK v3 business requirements [S]; ISO/IEC/IEEE 29148:2018 BRS, cl. 9.3 (business purpose, scope, goals and objectives, business model, project constraints) [V].
- **Examples.**
  - `BR-01` By 30 April 2027, the team shall demonstrate a LINE assistant that completes at least 5 named tasks end to end for real test users, each with a pass mark set in `AI-EVAL` requirements.
  - `BR-02` In the pilot, at least 60% of pilot users shall complete one task without help from the team within their first session. (Proposal.)
  - `BR-03` The pilot shall compare the acting assistant with a reminder-only baseline on the same tasks, measuring task success rate and median user steps (messages plus taps) per task. "Acts" is judged better if its task success is at least equal to the baseline and its median steps per task are at least 30% lower. (Thresholds are proposals.)
- **Verify.** Pilot data and the final evaluation report; reviewed by the advisor.

#### 1.3.2 Stakeholder and user requirements (`STK`, `US`, `UC`)

- **Definition.** What each stakeholder group needs, in their own terms. Stakeholders here: end users (students and staff, or working adults, depending on the merged scope), people in a user's LINE group, businesses that receive request cards, the team as operators, the advisor and examiners, LINE (platform owner), and the LLM provider.
  - **User story** (`US`): "As a ⟨role⟩, I want ⟨goal⟩ so that ⟨benefit⟩", with acceptance criteria. Good for backlog and sprints.
  - **Use case** (`UC`): a numbered main flow plus alternative and error flows. Good for actions with confirmation steps and failure paths, which is most of ours.
- **Why it matters.** The "business that receives a request card" and "friends in a group bill split" are stakeholders who never installed our product. Their needs (no sign-up, no spam, clear identity of the sender) are easy to miss if we only write user stories for the main user.
- **Source.** BABOK stakeholder requirements [S]; 29148 StRS cl. 9.4, including 9.4.15 user requirements and 9.4.17 operational scenarios [V].
- **Examples.**
  - `STK-03` (Business receiving a request) A business shall be able to answer a request card with one tap, without creating an account.
  - `US-07` As a student who paid for a group dinner, I want the assistant to split the bill and send each friend a PromptPay QR code to my own account, so that I am repaid without chasing people. *Acceptance:* given 4 participants and a total of ฿1,000 with equal split, each friend receives a QR code encoding ฿250 and my PromptPay ID; I confirm before anything is sent.
  - `UC-02 Create calendar event from chat`: main flow (user asks → assistant extracts title, date, time → shows confirmation card → user taps Confirm → event created → link returned); alternatives (missing time → ask; ambiguous "next Friday" → show the resolved date); errors (calendar not linked → link flow; API error → tell the user, nothing is created).
- **Verify.** Review with 3-5 representative users (interviews, walkthrough of use cases); acceptance tests derived from each story.

#### 1.3.3 Functional requirements (`FR`)

- **Definition.** What the system shall do: inputs, behaviour, outputs, for each feature.
- **Why it matters.** The core of the build. For an AI product, write FRs about the *system* behaviour (what is stored, what is sent, what is confirmed), and put the *quality of AI understanding* in `AI` requirements with test sets. Otherwise FRs become untestable ("shall understand Thai").
- **Source.** 29148 SRS 9.6.12 functions [V]; BABOK functional requirements [S]; IEEE 830 §3.2 [S].
- **Examples.**
  - `FR-REM-01` The system shall send a reminder message to the user's LINE chat at the stored time, within ±60 seconds.
  - `FR-BILL-03` The system shall generate one PromptPay QR payload per participant that encodes the payee's own PromptPay ID and the participant's share, rounded to 2 decimals, with the rounding remainder assigned to the payer.
  - `FR-ACT-01` The confirmation card shall list the action type, recipient, date and time, amount (if any) and service, and shall offer Confirm, Edit and Cancel buttons. (The rule that nothing runs without Confirm is the master requirement `AI-HITL-01`; this FR only defines the card's content.)
- **Verify.** Unit and integration tests per FR; end-to-end demo script.

#### 1.3.4 Quality (non-functional) requirements, by ISO/IEC 25010:2023 (`QR-<code>`)

- **Definition.** How well the system does its job. ISO/IEC 25010:2023 gives nine product quality characteristics; we use them as a checklist so no area is skipped.
- **Why it matters.** "Fast" and "secure" are not testable. Each quality requirement needs a number and a measurement method (Volere's "fit criterion").
- **Source.** ISO/IEC 25010:2023 [V foreword; S sub-characteristics]; ISO/IEC 25059:2023 for AI additions [S].

| Characteristic (2023 name) | Sub-characteristics | Example requirement for our product (proposal) | Verify by |
|---|---|---|---|
| Functional suitability (`QR-FS`) | completeness, correctness, appropriateness; *AI: functional adaptability, correctness measured statistically (25059)* | `QR-FS-01` End-to-end task completion (the correct action executed with the correct parameters after confirmation) on the held-out evaluation set shall be ≥ 85% per action. This is the product-level measure; `AI-EVAL-02` sets the component pass marks (intent, slots) that feed it. | Test set run |
| Performance efficiency (`QR-PE`) | time behaviour, resource utilisation, capacity | `QR-PE-01` For 95% of messages, the first reply (answer or "working on it" loading indicator) shall reach the user within 5 s. `QR-PE-02` With 50 simulated concurrent users for 10 minutes, the share of requests ending in a server error (HTTP 5xx) or a failed tool call shall be ≤ 1%. | Load test with recorded timings |
| Compatibility (`QR-CO`) | co-existence, interoperability | `QR-CO-01` Generated PromptPay QR codes shall scan and pre-fill the correct payee and amount in each of these apps: K PLUS (Kasikornbank), SCB EASY (Siam Commercial Bank), Krungthai NEXT (Krungthai Bank) and Bangkok Bank Mobile Banking. (App list is our choice [I]; confirm current app names before testing.) | Manual test matrix |
| Interaction capability (was usability) (`QR-IC`) | appropriateness recognisability, learnability, operability, user error protection, user engagement, inclusivity, user assistance, self-descriptiveness; *AI: user controllability, transparency (25059)* | `QR-IC-01` A first-time user shall be able to find "what can you do?" help from the rich menu in one tap. `QR-IC-02` Every error message shall state what failed and give one next step the user can take; checked on all error message templates. | Usability test (5 users, task success, SUS score) |
| Reliability (`QR-RE`) | faultlessness (was maturity), availability, fault tolerance, recoverability; *AI: robustness (25059)* | `QR-RE-01` Webhook processing shall be idempotent: a redelivered LINE event shall not create a second calendar event or second message. `QR-RE-02` Pilot availability ≥ 99% per week, measured by an external uptime check. | Replay test; uptime log |
| Security (`QR-SE`) | confidentiality, integrity, non-repudiation, accountability, authenticity, resistance (new); *AI: intervenability (25059)* | See `SEC` in 1.3.8. | Security tests |
| Maintainability (`QR-MA`) | modularity, reusability, analysability, modifiability, testability | `QR-MA-01` Each tool the agent can call shall be a separate module with its own unit tests. `QR-MA-02` The LLM provider and model shall be changeable by editing configuration only, with no code change. | Code review; CI coverage report |
| Flexibility (was portability) (`QR-FL`) | adaptability, scalability (new), installability, replaceability | `QR-FL-01` A new team member shall be able to run the whole system locally from the README in under 60 minutes. | Timed onboarding test |
| Safety (new in 2023) (`QR-SA`) | operational constraint, risk identification, fail safe, hazard warning, safe integration | `QR-SA-01` If the LLM or a tool fails mid-action, the system shall fail safe: no partial external action, and the user is told what did and did not happen. `QR-SA-02` When a bill split or request would show a PromptPay ID that the user has not confirmed before (an "unknown account"), the confirmation card shall show a warning naming that ID. | Fault-injection test |

ISO 25010:2023 moved **quality in use** (effectiveness, efficiency, satisfaction, freedom from risk, context coverage) to ISO/IEC 25019 [V foreword]. We measure quality in use in the pilot (Section 1.3.15), not in the SRS.

#### 1.3.5 External interface requirements (`IF`)

- **Definition.** How our software talks to systems we do not control, and to users' screens. Covers user interfaces, software interfaces (APIs), and communication protocols.
- **Why it matters.** Almost every action in our product goes through someone else's API, and each has hard limits. A LINE reply token works once and only within one minute; reply messages are free but push messages count against the Official Account's monthly quota [V, LINE docs]. These limits shape the design.
- **Source.** 29148 SRS 9.6.11 external interfaces [V]; IEEE 830 §3.1 [S]; LINE Messaging API reference and pricing pages [V]; LIFF docs [V]; Bank of Thailand Thai QR Code Payment Standard (based on EMVCo merchant-presented QR) [S, BoT policy guideline, not opened]; Google OAuth unverified-app limit of 100 new users [V].
- **Interfaces to specify:**
  - **LINE Messaging API** (webhook in, reply/push out, rich menu, loading indicator). Facts [V]: signature in `x-line-signature` header must be verified; reply tokens can be used once and within one minute; max 5 message objects per reply/push call; "other endpoints" limited to 2,000 requests per second; webhook redelivery is off by default and not guaranteed; push, multicast, broadcast and narrowcast count toward the monthly quota, reply messages do not.
  - **LIFF** (LINE Front-end Framework, web pages opened inside LINE): confirmation pages, settings, business answer page. Fact [V]: send the ID token or access token to the server and verify it there; do not send profile data from `liff.getProfile()` to the server.
  - **LLM provider API** (model, version, timeouts, token limits, data-retention terms).
  - **Calendar API** (Google Calendar; OAuth consent; 100-user cap until Google verifies the app).
  - **PromptPay QR** (EMVCo TLV payload with CRC; the exact Thai tag mapping should be taken from the BoT spec, sources conflict [S]).
  - **Web search API** if used for "search and compare".
- **Examples.**
  - `IF-LINE-01` The system shall reject any webhook request whose `x-line-signature` does not match the HMAC of the body computed with the channel secret.
  - `IF-LINE-02` The system shall answer within the reply-token window (one minute) using the reply API, and shall use push messages only for reminders and asynchronous results, so that the monthly push quota is not exceeded during the pilot.
  - `IF-LIFF-01` The server shall identify a LIFF user only from an ID token verified with LINE's verify endpoint.
- **Verify.** Contract tests with recorded API responses; a signature-failure test; a quota report from LINE's "get number of messages sent this month" endpoint.

#### 1.3.6 Data requirements (`DR`)

- **Definition.** What data the system stores, its structure (data model and data dictionary), how long it is kept, and how good it must be (quality).
- **Why it matters.** We store personal data (LINE user IDs, reminders, calendar tokens, bill participants, possibly document photos). PDPA s.23 requires telling users the retention period, and s.37(3) requires a system to delete data when it is no longer needed [V]. LINE's User Data Policy limits storing friend and group information to 24 hours and requires deletion when a user leaves the service [V, s.3.2.3, 3.2.8, 3.5].
- **Source.** 29148 SRS 9.6.15 logical database requirements and SyRS 9.5.14 information management [V]; Volere §7 business data model and data dictionary [V]; ISO/IEC 25012 data quality model (named in ISO 25010:2023 introduction) [V name only].
- **Examples.**
  - `DR-01` The SRS shall include a data dictionary listing every stored entity, each field, its type, whether it is personal data, its PDPA lawful basis, and its retention period.
  - `DR-04` One-to-one conversation text shall be kept for at most 30 days, then deleted or anonymised; group-chat text the bot receives shall be kept for at most 24 hours and only to process the request it contains (see `DR-07`); audit records of confirmed actions shall be kept for 180 days or until the evaluation report is submitted, whichever is earlier (see `TR-03`). (Proposal; the team decides the numbers. Possible conflict with the Computer-Related Crime Act s.26 90-day traffic-data rule; see open issue 16.)
  - `DR-07` Profile and membership information obtained from LINE friend or group APIs shall be deleted within 24 hours of retrieval and shall not be used after the user leaves the service (LINE User Data Policy s.3.2.3, s.3.2.8) [V].
  - `DR-06` Each participant amount in a bill split shall be stored as an integer number of satang, never as a floating-point number.
- **Verify.** Inspection of the schema against the data dictionary; a scheduled deletion job tested with time-shifted data.

#### 1.3.7 AI-specific requirements (`AI`)

- **Definition.** Requirements on the behaviour of the language model and the agent around it: what it may do, how often it must be right, how it handles uncertainty and attack, and how much it may cost.
- **Why it matters.** This is the category FR + NFR handles worst. LLMs confabulate (NIST AI 600-1) [V], can be steered by text inside a document or a web page (prompt injection, OWASP LLM01) [V], and an agent with tools can act too much (LLM06 Excessive Agency) [V] or spend too much (LLM10 Unbounded Consumption) [V]. ISO 25059 says ML correctness must be measured statistically, not assumed [S]. NIST AI RMF lists "valid and reliable", "safe", "explainable", "privacy-enhanced" among trustworthiness characteristics [V].
- **Source.** ISO/IEC 25059:2023 [S]; NIST AI RMF 1.0 and AI 600-1 [V]; OWASP Top 10 for LLM Applications 2025 [V]; LINE User Data Policy s.3.3 (no proxy sending via the user's account against their wishes; show details first; applied by analogy to bot-sent messages) [V].
- **Sub-types and examples:**
  - **Model behaviour and scope** — `AI-BEH-01` The assistant shall only call tools from a fixed allow-list; any request outside its supported tasks shall get a short "I can't do that yet" reply listing what it can do.
  - **Human confirmation before actions (MASTER requirement)** — `AI-HITL-01` No action that writes or sends outside the conversation (calendar write, message, request card or QR code to any third party) shall run unless the user has been shown the exact content to be sent (`FR-ACT-01`) and a confirmation record exists for that exact action and parameters, created by the user's tap and stored in the audit log (`SEC-03`). This check shall be enforced in server code, not in the prompt. `LEG-02` and `LEG-06` trace to this requirement. (OWASP LLM06; LINE User Data Policy s.3.3 directly for LIFF send/share, by analogy for bot messages; ETA s.15 para 2(2), s.17.)
  - **Evaluation sets and thresholds** — `AI-EVAL-01` The team shall keep a versioned Thai/English evaluation set of at least 50 labelled requests per supported action (including slang, typos, mixed Thai-English, and Buddhist-era dates). `AI-EVAL-02` A release shall not be demoed unless intent accuracy ≥ 90% and slot (date, time, amount, people) exact-match ≥ 85% per action. (Thresholds are proposals; the jui proposal uses ≥ 85%.)
  - **Hallucination (confabulation) handling** — `AI-HAL-01` Any date, time, amount or name the assistant will act on shall be shown back to the user in the confirmation card, taken from the parsed tool arguments, not from free text. `AI-HAL-02` When a required field is missing or low-confidence, the assistant shall ask instead of guessing.
  - **Prompt-injection defence** — `AI-PI-01` Text from documents, images, web pages and other people's messages shall be passed to the model as untrusted data and shall not be able to trigger a tool call without the user's confirmation. `AI-PI-02` The adversarial test set shall contain ≥ 30 injection cases; 100% shall result in no unconfirmed action.
  - **Explainability and transparency** — `AI-EXP-01` (MASTER requirement for AI disclosure) The welcome message, the privacy notice and every message sent to a third party (business or group) shall state that it comes from an AI assistant acting for the named user. `LEG-08` traces to this requirement. `AI-EXP-02` The user shall be able to ask "why did you do that?" and get the stored action log entry.
  - **Cost limits** — `AI-COST-01` Each user shall have a daily token budget; above it the assistant shall reply with a fixed message and not call the LLM. `AI-COST-02` Average LLM cost per active pilot user shall stay under a monthly cap set in `CON-BUD`. (OWASP LLM10.)
  - **Content safety and misinformation** — `AI-SAFE-01` Free-text answers on health, legal or financial questions shall carry a "not professional advice" notice and shall not recommend a specific action. `AI-SAFE-02` An adversarial set of ≥ 20 harmful-content prompts (violent, hateful, abusive, self-harm, scam) shall yield 0 policy-violating outputs. (NIST AI 600-1 content risks; OWASP LLM09.)
  - **Model and supply chain** — `AI-SUP-01` The model name and version used for every response shall be logged; changing model requires re-running the evaluation set. (OWASP LLM03; NIST "value chain and component integration".)
- **Verify.** Automated evaluation runs (scripts over the test set, results stored per release); adversarial test set; code inspection that the confirmation check sits before every write tool; cost dashboard.

#### 1.3.8 Security and privacy requirements (`SEC`, `PRV`)

- **Definition.** Security: protect the system and data from unauthorised access, change and misuse (ISO 25010 security). Privacy: handle personal data lawfully, fairly and only for stated purposes (PDPA; NIST "privacy-enhanced").
- **Why it matters.** We hold OAuth tokens that can write to a user's calendar, and we send messages in the user's name. A leak or a hijack harms real people. PDPA s.83 sets an administrative fine of up to ฿3 million for breaching, among others, s.24 (lawful basis), s.28 (cross-border transfer) and s.37 (security, deletion, breach notice) [V, unofficial translation].
- **Source.** ISO 25010:2023 security sub-characteristics [S]; OWASP LLM02, LLM07 [V]; PDPA s.19, 23, 26, 28, 30-37 [V unofficial translation]; LINE Developers Agreement §5.2.2 (store user data securely, report unauthorised access) [V]; LINE User Data Policy [V].
- **Examples.**
  - `SEC-01` OAuth refresh tokens and the LINE channel secret shall be stored encrypted and never written to logs or sent to the LLM.
  - `SEC-03` Every confirmed action shall be written to an append-only audit log with user ID, action, parameters, confirmation time and result (supports accountability and non-repudiation).
  - `PRV-01` Before first use, the system shall show a privacy notice covering purposes, data collected, retention, recipients (including the LLM provider), and user rights (PDPA s.23).
  - `PRV-08` Where consent is the lawful basis, it shall be requested on its own screen, separate from other terms, in clear Thai or English, with the purpose stated (PDPA s.19).
  - `PRV-02` Because the LLM provider processes data outside Thailand, the notice shall say so and the team shall record the s.28 basis used (UNVERIFIED which basis applies; ask the advisor or university legal office).
  - `PRV-03` A user shall be able to request a copy of, or deletion of, their data from the settings page; the team shall fulfil either within 30 days. (Access: 30 days is the PDPA s.30 limit [V]. Erasure: the PDPC AI guideline refers to a 90-day period for s.33 requests [V guideline]; 30 days is our stricter proposal.)
  - `PRV-06` Data about bill-split participants and group members who are not users shall be limited to display name and amount owed, and shall be deleted when the split is settled or 30 days after it was created, whichever is earlier. (They are PDPA data subjects who never agreed to our terms; 30 days is a proposal.)
  - `PRV-07` The user shall be able to withdraw consent and to leave the service (unlink from LINE) from the settings page. On withdrawal the system shall stop processing that user's data and delete all LINE user information, including LINE user IDs, except audit records kept only for handling inquiries (LINE User Data Policy s.3.5.2.2), which follow `DR-04` and `TR-03`. (PDPA s.19; LINE User Data Policy s.3.2.10, s.3.5.2) [V].
  - `PRV-05` Chat logs shall not be used to train or fine-tune any model unless the user opts in separately; using the assistant shall never depend on that opt-in (PDPC AI guideline: no bundled consent) [V].
  - `PRV-04` The assistant shall not store images of ID cards or health documents beyond the task that needed them (sensitive data, s.26).
- **Verify.** Secret-scanning in CI; log inspection; a deletion-request test; a short threat model (STRIDE or OWASP LLM list) reviewed once per phase.

#### 1.3.9 Legal, regulatory and platform-policy compliance (`LEG`)

- **Definition.** Rules the product must obey because a law, regulator, or platform owner says so. Unlike quality goals, these are pass/fail and not negotiable.
- **Why it matters.** LINE can suspend a channel without notice if it believes we breach the Developers Agreement (§5.6) [V]. LINE's rules apply even to a student prototype.
- **Source.** PDPA [V]; Electronic Transactions Act s.15 para 2(2), s.16, s.17 [V, ETDA translation] and s.13/2, s.17/1 [V, ETA No. 3 translation]; PDPC AI guideline 2026 (advisory) [V]; draft Thai AI Act (not enacted) [S]; Computer-Related Crime Act s.26 [S]; LINE Developers Agreement, User Data Policy, Official Account terms, LINE MINI App Policy [V list; OA terms are Japanese-language pages for Japan, Thai OA terms UNVERIFIED]; Google API Services User Data Policy (UNVERIFIED, not opened); university research-ethics rules for user testing (UNVERIFIED).
- **Examples.**
  - `LEG-01` The service shall publish a privacy policy that is viewable at any time, before and after a user starts using it (LINE User Data Policy s.2.2, 2.4; PDPA s.23).
  - `LEG-02` (compliance row) LINE User Data Policy s.3.3: no proxy sending or posting via the user's account against their wishes, details shown first. Applies directly to any LIFF send/share feature; applied by analogy to bot-sent messages. **Satisfied by `AI-HITL-01`.**
  - `LEG-03` The system shall not hold or move money. PromptPay QR codes shall always pay the user's or the business's own account. (Keeps us outside payment-service licensing; see `2026-11-08-agent-legal-economics.md`.)
  - `LEG-04` Before any pilot with real users, the team shall obtain the approvals the university requires for research with human participants. (Process UNVERIFIED.)
  - `LEG-05` Pilot testers shall be aged 20 or over, or married in accordance with the Civil and Commercial Code (sui juris), unless written consent from the holder of parental responsibility is also obtained. Rationale: Thai majority is reached on completing 20 years of age (Civil and Commercial Code s.19) or on lawful marriage (s.20, marriage under s.1448) [S]. First-year students are often 18-19, so they are minors; PDPA s.20(1) then requires parental consent as well as the minor's where the consent is not an act the minor may do alone [V, PDPA translation]. Simplest route for the pilot: recruit testers aged 20 or over.
  - **Electronic Transactions Act: why confirm-before-acting and Edit/undo are legal requirements, not just good design.** Under ETA s.15 para 2(2), a message sent by an information system programmed to operate automatically in advance for the originator is deemed the originator's, and under s.17 (see also s.16) the receiver (a business, a friend) may treat it as what the originator intended and act on it [V, ETDA translation]. So a request card or bill-split message our assistant sends is, legally, *the user's* message. If the AI got the date or amount wrong, the user is bound unless the receiver should have noticed. ETA s.17/1 adds that a person who makes an input error through an automated system that gives no channel to correct it may withdraw the erroneous part [V]. A system that always shows a correctable confirmation keeps the user in control and removes that dispute.
  - `LEG-06` (compliance row) ETA s.15 para 2(2) and s.17: messages the assistant sends are attributed to the user and may be relied on by the receiver, so the user must see and confirm the exact content first. **Satisfied by `AI-HITL-01`** (with `FR-ACT-01` for card content and `SEC-03` for the record).
  - `LEG-07` Every confirmation card shall offer **Edit** and **Cancel**, and for 10 minutes after a confirmed action that the receiver has not yet answered, the user shall be able to **withdraw** it: the system retracts the request card (marking it "withdrawn by sender" on the business answer page) or deletes the created calendar event, and tells the user what could and could not be undone. (ETA s.17/1 input-error channel; the 10-minute window is a proposal.)
  - `LEG-08` (compliance row) PDPC AI guideline 2026 (advisory [V]): tell users from the start that they are dealing with an automated system. **Satisfied by `AI-EXP-01`.**
  - `LEG-09` The team shall record in the SRS that the draft Thai AI Act is not law as of 9 Nov 2026, and shall re-check its status at each phase gate. If enacted, check whether the reported carve-out for university research with ethics approval applies to our pilot. (Draft AI Act [S].)
  - Paired AI requirement: `AI-HITL-02` If the user edits any field on a confirmation card, the system shall rebuild the card from the edited values and require a fresh Confirm; it shall never execute the earlier, unedited version.
- **Verify.** Compliance checklist reviewed by the team and advisor before the pilot; inspection of the privacy page; walkthrough of every outbound-message path.

#### 1.3.10 Constraints (`CON`)

- **Definition.** Fixed limits on *how* we build, set from outside the design: time, people, money, platform and technology choices.
- **Why it matters.** Our biggest constraint is effort: 3 students × 3-4 hours a week ≈ 9-12 hours a week in total, roughly 230-300 person-hours from mid-November 2026 to the end of April 2027 (estimate, ignoring exam breaks). Every "Must" requirement has to fit inside that.
- **Source.** 29148 SRS 9.6.16 design constraints and 9.3.19/9.4.19 project constraints [V]; Volere §3 mandated constraints [V].
- **Examples.**
  - `CON-TIME-01` The architecture and overall design shall be finalised by 22 November 2026; the prototype shall be complete by 30 April 2027.
  - `CON-TEAM-01` The Must-have scope shall be sized to no more than about 120-160 person-hours of build and test effort, about 50-60% of the 230-300 total (estimate). The rest covers the proposal, documents, meetings, the pilot and the final report.
  - `CON-BUD-01` Running costs (LLM, hosting, LINE OA plan) during the pilot shall not exceed ฿⟨team to set⟩ per month.
  - `CON-PLAT-01` The user channel shall be LINE (Messaging API + LIFF); no separate mobile app.
- **Verify.** Inspection; monthly cost report; timesheet or sprint burn-down.

#### 1.3.11 Transition and deployment requirements (`TR`)

- **Definition.** Temporary capabilities needed to move from "no system" to "system in use": environments, onboarding of pilot users, data seeding, account linking, release steps. BABOK says they disappear after cutover [S].
- **Why it matters.** The pilot is a cutover. Calendar linking needs Google consent and is capped at 100 users until verified [V]; LINE OA needs setup; test users need a way in and out.
- **Source.** BABOK transition requirements [S]; Volere §22 migration to the new product [V]; 29148 SyRS 9.5.16 life-cycle sustainment [V].
- **Examples.**
  - `TR-01` There shall be separate development and pilot LINE channels, so tests never message pilot users.
  - `TR-02` Pilot onboarding shall take under 3 minutes: add friend → consent → optional calendar link.
  - `TR-03` At the end of the pilot the team shall delete or anonymise pilot data within 30 days and tell users how. Precedence: audit logs (`DR-04`) and anonymised evaluation data are kept until the evaluation report is submitted, then deleted; every shorter limit (`DR-04` chat text, `DR-07`, `PRV-06`) still applies during and after the pilot.
- **Verify.** Deployment checklist run before the pilot; timed onboarding with 3 users.

#### 1.3.12 Operational and support requirements (`OPS`)

- **Definition.** What is needed to keep the system running and to help users while it runs: monitoring, logging, alerts, backups, incident handling, user support.
- **Why it matters.** With 3-4 hours a week each, nobody is on call. The system has to detect and report its own failures. PDPA's 72-hour breach notice also needs someone to notice the breach.
- **Source.** 29148 SyRS 9.5.9 system operations [V]; Volere §13 operational and §14 maintainability and support [V]; PDPA s.37(4) [V].
- **Examples.**
  - `OPS-01` Failed webhook handling, failed tool calls and LLM errors shall raise an alert to the team's chat within 5 minutes.
  - `OPS-02` The database shall be backed up daily and a restore shall be tested once before the pilot.
  - `OPS-03` Users shall be able to type "help" or "contact team" to reach a human; the team shall answer within 2 working days during the pilot.
  - `OPS-04` The team shall keep a one-page incident procedure, including the PDPA 72-hour breach notice step.
- **Verify.** Alert drill (break a dependency on purpose); restore test; inspection of the incident page.

#### 1.3.13 Usability, localisation and accessibility (`UX`, `L10N`, `A11Y`)

- **Definition.** Usability: users can reach goals effectively, efficiently and with satisfaction (ISO 25010 interaction capability). Localisation: language, date, number and cultural conventions. Accessibility: people with disabilities can use the product (WCAG 2.2; ISO 25010 inclusivity and user assistance).
- **Why it matters.** Thai users write mixed Thai-English, use Buddhist-era years (2569 = 2026), Thai number words, and polite particles. A wrong year on a calendar event is a real error. LIFF pages are web pages, so WCAG applies to them directly. Chat bubbles are rendered by LINE, so we control only content and button labels there.
- **Source.** ISO 25010:2023 interaction capability [S]; Volere §10 look and feel, §11 usability and humanity, §16 cultural [V]; WCAG 2.2 [V].
- **Examples.**
  - `L10N-01` The assistant shall reply in the language of the user's last message (Thai or English), and the user can fix the language in settings.
  - `L10N-02` The assistant shall accept Buddhist-era and Common-era years and show dates in the confirmation card in both forms (e.g. "จ. 14 ธ.ค. 2569 / Mon 14 Dec 2026").
  - `A11Y-01` All LIFF pages shall meet WCAG 2.2 Level AA, including 2.5.8 Target Size (Minimum) and 3.3.8 Accessible Authentication (Minimum).
  - `UX-01` After the assistant's first reply in a task, every following step (choose an option, confirm, edit a field, cancel) shall be possible with quick-reply buttons alone, without typing.
- **Verify.** Usability test with 5 users per round (task success, time, SUS questionnaire); automated checker (e.g. axe) plus manual keyboard and screen-reader check on LIFF pages; L10N test cases in the evaluation set.

#### 1.3.14 Documentation and academic deliverable requirements (`DOC`)

- **Definition.** Documents and artefacts the project must produce, for users, for maintainers, and for the course. These are *project* requirements, not product requirements.
- **Why it matters.** The grade depends on them. Writing them down as requirements puts them in the plan and the traceability matrix.
- **Source.** Volere §25 user documentation and training, §21 tasks [V]; 29148 project constraints [V]; course rules (not seen by me; UNVERIFIED).
- **Examples.**
  - `DOC-01` By 22 November 2026: proposal with problem, scope, SRS v0.9 (all Must requirements with acceptance criteria), architecture diagram, data model, risk list, and plan.
  - `DOC-02` By 30 April 2027: SRS v1.0 baselined, design document, test and evaluation report (including AI evaluation results per action), user guide in Thai, README for running the system, and demo video.
  - `DOC-03` Every architecture decision shall be recorded as a short decision record (context, decision, consequences).
- **Verify.** Checklist against the course's deliverable list; advisor review.

#### 1.3.15 Acceptance criteria and verification requirements (`VER`)

- **Definition.** Two things. (a) Each requirement's **acceptance criterion**: the measurable condition that shows it is met (Volere calls it a fit criterion). (b) Requirements on **how** we verify: methods, test levels, evaluation protocol, and the pilot that measures quality in use.
- **Why it matters.** Without (a), a requirement cannot be marked done. Without (b), AI quality claims in the final report have no evidence.
- **Source.** 29148 SRS 9.6.19 verification and SyRS 9.5.18 [V]; 29148 6.5.2 requirements activities in verification [V heading]; Volere fit criterion [V]; ISO/IEC 25019 quality in use [V name only].
- **Verification methods** (the usual four, as used in systems engineering and 29148 [S]): **Inspection** (look at it), **Analysis** (calculate or model), **Demonstration** (show it working), **Test** (run with defined inputs and expected outputs). Add **Evaluation** for AI behaviour on a test set and **User study** for pilot measures.
- **Examples.**
  - `VER-01` Every Must requirement shall have an acceptance criterion before the proposal is submitted, and a linked test or evaluation case before the Phase 1 gate. Every Should requirement shall have a linked test before the prototype review.
  - `VER-02` The AI evaluation set shall be split into a development part and a held-out part; reported numbers shall come from the held-out part only.
  - `VER-03` The pilot shall measure, per action: task success, time to complete, number of corrections, and user satisfaction, with at least 15 participants. (Proposal.)
- **Verify.** Traceability matrix inspection (no Must without a test); CI test report; evaluation report.

#### 1.3.16 Assumptions, dependencies and out of scope (`ASM`, `DEP`, `OOS`)

- **Definition.** Assumptions: things we believe are true but do not control. Dependencies: outside services or decisions we rely on. Out of scope: things we have decided not to do.
- **Source.** 29148 SRS 9.6.8 assumptions and dependencies [V]; Volere §5 relevant facts and assumptions and §26 waiting room [V].
- **Examples.** `ASM-01` Businesses will answer a request card without signing up (untested). `DEP-02` LINE keeps the Messaging API pricing model through April 2027. `OOS-01` Phone calls by AI voice, SMS, and holding money are out of scope.
- **Verify.** Review at each phase gate; each assumption has an owner and a test date.

---

## 2. Writing good requirements: checklist, IDs, attributes, traceability

### 2.1 Quality checklist for each requirement

ISO/IEC/IEEE 29148:2018 clause 5.2.5 lists characteristics of an individual requirement, and 5.2.6 those of a set [V clause titles; the term list below is S, from secondary summaries of the standard, because the clause text is paid].

**Each requirement must be:**

| Characteristic | Question to ask | Bad → better |
|---|---|---|
| Necessary | If we delete it, does a stakeholder or a law miss it? Does it trace to a `BR` or `LEG`? | — |
| Appropriate | Is it at the right level (not a design choice dressed as a requirement)? | "Use PostgreSQL" → move to `CON` or a design decision, unless it is truly mandated |
| Unambiguous | Can it be read only one way? | "Reply quickly" → "first reply within 5 s for 95% of messages" |
| Complete | Does it say who, what, when, under which condition? | — |
| Singular | One requirement, one statement. No "and/or". | Split "create and share the event" into two |
| Feasible | Can 3 students build it in time with known technology? | — |
| Verifiable | Is there an acceptance criterion and a method? | "Understands Thai" → evaluation-set threshold |
| Correct | Does it state the real need and real facts (e.g. LINE limits)? | — |
| Conforming | Does it follow our template, ID scheme and wording rules? | — |

**The set of requirements must be:** complete (no missing category; use Section 1.2), consistent (no two requirements conflict, e.g. a 30-day log deletion vs a 90-day log retention rule), feasible as a whole (fits `CON-TEAM-01`), comprehensible (a newcomer can read it), and able to be validated (we can show stakeholders it meets their needs) [S].

**Wording rules** (29148 5.2.7 "requirement language criteria" [V heading]; detail [S]):
- Use **shall** for binding requirements, **should** for goals, **will** for facts or statements about others, **may** for options.
- Avoid vague words: *user-friendly, fast, easy, robust, adequate, as appropriate, etc., and/or, including but not limited to, minimise, maximise, support* (without saying how).
- Use active voice with a clear subject: "The system shall…", "The user shall be able to…".
- A useful sentence pattern is EARS (Easy Approach to Requirements Syntax) [S]: *While ⟨state⟩, when ⟨trigger⟩, the ⟨system⟩ shall ⟨response⟩.* Example: "When the user taps Confirm on a calendar card, the system shall create the event and reply with its link."

### 2.2 Requirement ID scheme

Format: `PREFIX-AREA-NNN`, for example `FR-BILL-003`.

- **PREFIX** = category from Section 1.2 (`BR, STK, US, UC, FR, QR-xx, IF, DR, AI, SEC, PRV, LEG, CON, TR, OPS, UX, L10N, A11Y, DOC, VER, ASM, DEP, OOS`).
- **AREA** (optional, for FR, IF, AI, CON) = short feature or subtype code, e.g. `REM` reminders, `CAL` calendar, `POLL` group poll, `BILL` bill split, `DOC` document reading, `REQ` request cards, `ACT` cross-cutting action handling; `LINE`, `LIFF`, `LLM`, `GCAL`, `PPQR` for interfaces; `BEH, HITL, EVAL, HAL, PI, EXP, SAFE, COST, SUP` for AI.
- **NNN** = number, never reused. A deleted requirement keeps its ID with status *Deleted*.
- Test cases use `TC-` plus the requirement ID they mainly cover (e.g. `TC-FR-BILL-003-a`). Evaluation items use `EV-<action>-NNNN`. Design decisions use `ADR-NNN`.
- Put requirement IDs in commit messages and test names ("FR-BILL-003: round remainder to payer") so traces can be found by search.

### 2.3 Attributes every requirement carries

29148 clause 5.2.8 lists requirement attributes such as identification, owner, priority, risk, rationale, difficulty, type, source, stability, verification method and status [V clause title; list S]. Scaled for us:

| Attribute | Values / notes | Required? |
|---|---|---|
| ID | Section 2.2 | Yes |
| Title | ≤ 8 words | Yes |
| Statement | The "shall" sentence | Yes |
| Type | Category from Section 1.2 | Yes |
| Priority | **MoSCoW**: Must (prototype fails without it), Should, Could, Won't (this time) | Yes |
| Source | Stakeholder, interview note, law section, LINE doc URL, or member proposal | Yes |
| Rationale | One sentence: why it exists | Yes |
| Acceptance criteria | Measurable condition (Given/When/Then is fine) | Yes |
| Verification method | Inspection / Analysis / Demonstration / Test / Evaluation / User study | Yes |
| Status | Proposed → Agreed → Implemented → Verified (or Deleted / Deferred) | Yes |
| Trace links | Up: `BR`/`STK`/`LEG`. Down: design element (`ADR`, component), test or eval case IDs | Yes |
| Owner | One team member | Yes |
| Risk | Low / Medium / High (technical or compliance risk) | Optional |
| Release | Proposal (Nov 2026), Phase 1, Phase 2, Prototype (Apr 2027) | Optional |
| Version / last changed | Date | Optional (Git history covers it) |

### 2.4 Traceability, scaled for 3 students

Goal: answer two questions at any time. *"Why do we have this?"* (trace up) and *"How do we know it works?"* (trace down).

Chain: **Business goal (`BR`) → stakeholder need (`STK`/`US`/`UC`) → requirement (`FR`, `QR`, `AI`, …) → design element (`ADR`, component, API) → test or evaluation case (`TC`, `EV`) → result (CI run, evaluation report, pilot data).**

Lightweight approach (about 30 minutes a week for the whole team):

1. **One requirements table** as the single source of truth: either a Markdown file `docs/requirements/srs.md` with one table per category, or one shared spreadsheet with one row per requirement and the attributes in Section 2.3 as columns. Pick one; do not keep both.
2. **Trace columns, not a separate matrix.** Each row has "Traces up" and "Verified by" columns. The traceability matrix is then just a filter or pivot of that table.
3. **IDs in code and tests.** Test names and commit messages carry requirement IDs. A small script (later) can list requirement IDs with no matching test.
4. **Three checks at each phase gate:** (a) every `FR`/`AI`/`QR` row traces up to a `BR`, `STK` or `LEG`; (b) every Must row has at least one test or evaluation case; (c) every `BR` has at least one requirement under it. Anything failing (a) is a candidate to cut.
5. **Change control:** a requirement change after "Agreed" is a pull request that edits the row and says why; one other member approves.

---

## 3. Recommended SRS structure

Based on the ISO/IEC/IEEE 29148:2018 SRS outline (cl. 9.6) [V], with the IEEE 830 three-part shape [S], plus an AI section and Volere-style constraints and fit criteria. Two maturity levels: **v0.9 for the proposal (22 Nov 2026)** and **v1.0 baselined for the prototype (30 Apr 2027)**.

| Section | Content | v0.9 by 22 Nov 2026 (proposal) | v1.0 by 30 Apr 2027 (prototype) |
|---|---|---|---|
| **1 Introduction** | 1.1 Purpose; 1.2 Scope (in and out); 1.3 Definitions and acronyms (LIFF, OA, PDPA, LLM, PromptPay…); 1.4 References; 1.5 Business requirements (`BR`) | Full | Updated |
| **2 Overall description** | 2.1 Product perspective (context diagram: user, LINE, our server, LLM, calendar, banks via QR, businesses); 2.2 Product functions (list of supported actions); 2.3 Stakeholders and user characteristics (personas); 2.4 Operating environment; 2.5 Constraints (`CON`); 2.6 Assumptions, dependencies, out of scope (`ASM`, `DEP`, `OOS`) | Full | Updated |
| **3 Specific requirements** | | | |
| 3.1 External interfaces (`IF`) | LINE Messaging API, LIFF, LLM, calendar, PromptPay QR, search API; UI overview (rich menu, cards, LIFF pages) | Interfaces named only | Full, with contract tests |
| 3.2 Functional requirements (`FR`), grouped by action | For each action: user story or use case, then FRs | Must use cases only | All actions |
| 3.3 AI requirements (`AI`) | Behaviour scope, human confirmation, evaluation sets and thresholds, confabulation handling, prompt-injection defence, transparency, cost, model supply chain | List of requirements; thresholds "TBD after spike" | Thresholds confirmed with first evaluation data |
| 3.4 Quality requirements (`QR`) by ISO 25010:2023 | One subsection per characteristic; 3.4.4 holds usability, localisation and accessibility (`UX`, `L10N`, `A11Y`) | One line per characteristic, or "N/A" | Full |
| 3.5 Data requirements (`DR`) | Data model (ER diagram), data dictionary, retention, data quality | Entity list + retention table | Full dictionary |
| 3.6 Security and privacy (`SEC`, `PRV`) | Threat model summary; controls; PDPA rights handling | Checklist | Full, with test evidence |
| 3.7 Compliance (`LEG`) | PDPA, LINE policies, ETA, ethics approval | Checklist | Signed-off checklist |
| 3.8 Transition and deployment (`TR`) | Environments, onboarding, pilot start and end | Moved to v1.0 | Full |
| 3.9 Operations and support (`OPS`) | Monitoring, alerts, backups, incident and breach procedure, user support | Moved to v1.0 | Full |
| **4 Verification** (`VER`) | Methods per requirement; test strategy; AI evaluation protocol; pilot design and measures | Strategy only | Results summary and links |
| **5 Traceability** | Filter of the requirements table: `BR` → requirements → design → tests | Moved to v1.0 (acceptance criteria only at v0.9) | Fully traced up and down |
| **Appendices** | A. Requirement attribute table; B. Evaluation-set description; C. Glossary (Thai–English); D. Open issues; E. Change log | A, D | All |

What **not** to do at proposal stage: do not write every Should and Could in full; do not specify user interface pixels; do not invent precise numbers the team cannot defend. Mark them "TBD after spike" with a date.

Where the other project documents fit: `DOC` requirements and the schedule belong in the project plan (`docs/planning/`), not the SRS; design decisions go in the design document and ADRs; the SRS links to them.

---

## 4. Open issues and items I could not verify

1. **ISO/IEC/IEEE 29148 clause text** (exact wording of characteristics 5.2.5-5.2.6, attribute list 5.2.8, verification methods) is behind a paywall. I verified the table of contents and scope from the official preview; the term lists come from secondary sources [S].
2. **ISO/IEC 25059** characteristic placement is from secondary sources [S]. The 2023 edition was written against ISO 25010:2011 (where "usability" still existed); secondary sources map it to the 2023 names. A second edition is in progress; its publication status is UNVERIFIED.
3. **ISO/IEC 25010:2023** facts come from the FDIS preview foreword [V]. The published text may differ slightly in wording; the characteristic names match the secondary sources.
4. **BABOK v3** official text is behind IIBA login; classification is from consistent study guides [S].
5. **IEEE 830 section layout** is from memory and secondary sources [S]; supersession by 29148 is confirmed by the IEEE listing.
6. **PDPA** quotes are from an unofficial English translation [V of the translation]. The Thai Royal Gazette text is authoritative. Which s.28 basis covers sending prompts to a foreign LLM provider is UNVERIFIED and needs advice.
7. **Age of majority and ethics process.** Majority at 20 years (Civil and Commercial Code s.19) or by lawful marriage (s.20) is [S], from consistent English translations, e.g. https://www.thailawonline.com/thai-civil-code/section-19/ (Thai original not opened). A proposal to lower it to 18 appears in Thai academic writing; no enactment found. Impact: testers aged 18-19 need parental consent as well as their own (see `LEG-05`). The university ethics process is still UNVERIFIED; ask the advisor.
8. **Computer-Related Crime Act s.26** (90-day traffic-data retention) applicability to a student prototype: UNVERIFIED.
9. **Electronic Transactions Act.** Section numbers are now taken from ETDA's English translation of ETA B.E. 2544 [V], which is for information only; the Thai text is authoritative. Correct mapping: s.13 offer and acceptance; **s.15 para 2(2)** a message from a system programmed to operate automatically for the originator is the originator's; **s.16** when the addressee may regard a message as the originator's; **s.17** the addressee may treat it as what the originator intended. s.13/2 and s.17/1 come from the ETA No. 3 translation on MDES [V]. Whether s.17/1 (input error in *another party's* automated system) applies when our system causes an error for our own user is an interpretation question; we use it as the design basis, not as a claim of legal coverage. s.7-9 are [S]. The status of the 2026 draft ETA revision is UNVERIFIED.
10. **LINE Official Account Terms for Thailand**: the developer site links only Japanese-language OA terms; Thai OA terms UNVERIFIED. No LINE rule specific to AI chatbots was found in the Developers Agreement.
11. **Thai QR / PromptPay tag mapping**: sources conflict; use the Bank of Thailand specification (not opened) before coding `FR-BILL`.
12. **Google Calendar scope classification** (sensitive or not) UNVERIFIED; the 100-new-user cap for unverified apps is [V].
13. **Thai AI-specific law.** The draft AI Act (ETDA; revised draft 2 July 2026; hearing closed 14 Aug 2026) is **not enacted** as of 9 Nov 2026 [S, three law-firm or consultancy sources]. I did not open the draft text on the Council of State portal. Its duties, fines (reported ฿1-5 million) and the education/R&D carve-outs come from secondary sources and may change before enactment. Sources differ on whether the draft has a general chatbot-disclosure duty; the ones I read report none.
14. All numeric targets (5 s, 85-90%, 30 days, 15 participants, budgets) are **proposals** for the team to agree, not facts.
15. **PDPC AI guideline.** I verified the official item on pdpc.or.th (posted 7 Oct 2026) and read key passages of the Thai PDF: non-binding status, human-in-the-loop, chatbot disclosure, no bundled consent, the 90-day reference for s.33 erasure. The reported Royal Gazette publication (2 Nov 2026, vol. 143, special issue 261 ง) is UNVERIFIED. The PDF cover still carries the English line "Draft of Guidelines", so check that the team cites the final version. The teammate's 14 Oct 2026 article is the Tilleke & Gibbins piece cited above. One detail in it (a 90-day erasure window) matches the guideline text I read.
16. **Retention conflict: Computer-Related Crime Act s.26 vs `DR-04`.** s.26 requires service providers to keep computer traffic data for at least 90 days [S]; `DR-04` deletes chat text after 30 days. Traffic data (source, destination, time) is not message content, so one way out is to keep minimal traffic metadata for 90 days and content for 30. Whether the team counts as a "service provider" is UNVERIFIED; get advice before the pilot.
