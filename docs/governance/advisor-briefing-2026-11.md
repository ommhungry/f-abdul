# Advisor Briefing — Thailand LINE AI Personal Assistant

**Date:** 12 November 2026, updated 17 November 2026 · **Status:** Draft for advisor review, pre-proposal (submission 22 Nov 2026) · **Full detail:** [proposed features](../planning/proposed-features.md), [AI implementation](../design/ai-implementation.md), [timeline](../planning/timeline-and-development-phases.md), [database design](../design/database-design.md), [architecture brief](../research/business-and-architecture-brief.md)

> **Update (17 Nov 2026).** The 12 November version of this briefing proposed keeping booking in the baseline through non-API routes. The project owner's 15 November direction supersedes that: **calendar-connected reminders are the current scope, the proposed spending-and-bills assistant was dropped on 17 November, and booking moves to phase 2.** Sections 2, 3 and 6 are rewritten accordingly. The non-API booking routes are kept as phase 2 options.

## 1. Project idea

A LINE-based AI assistant for office workers and other individuals in Thailand managing everyday obligations (Bangkok-first) that turns a forwarded bill, document, or appointment message into a tracked obligation — due date, amount, responsible person, completion status — and follows it through to completion. Primary user: an individual managing their own bills, rent, and renewals. Family sharing (one person selectively sharing specific obligations with family, without exposing their full account) is deferred and not a priority for the April prototype.

**Pitch:** *Send it once. Know what needs doing, get help doing it, and keep track of what is actually finished.*

**Positioning vs. competitors (Abdul, Lamai, Cole):** reminders and calendar sync already exist in the market. Our differentiation is **capture-to-completion trustworthiness** — distinguishing *user-reported* from *externally verified* completion, and source-grounded extraction (no silently invented dates/amounts) — not any single integration.

## 2. Booking moves to phase 2 — and how it could be done without a private API

The brief's booking-feasibility research (§6) found that major Bangkok booking platforms (Hungry Hub, TableCheck, GoWabi) require a private-API partner agreement, which depends on partnerships and evidence of a user base that a pre-revenue student team cannot show before the pilot. The project owner therefore moved booking to **phase 2** on 15 November.

The 12 November analysis of non-API routes still stands and is kept as the phase 2 starting point. None of these needs third-party API approval:

| Alternative | How it works | Build cost |
|---|---|---|
| **Concierge-assisted booking** | Automated request/offer/approval flow; a teammate places the actual call/LINE message to the merchant and logs the outcome. Must be disclosed as human-assisted | Low |
| **Direct merchant self-host** | Recruit small independent merchants with no existing booking platform and build their booking system. Doubles as a merchant-access moat (brief §4) | Medium |
| **Hosted-handoff deep link** | Pre-filled link into a merchant's existing hosted booking page, opened in a LIFF webview; the user completes the final tap | Low |
| **LINE OA-to-OA relay** | The assistant sends the structured request as an ordinary customer message to the merchant's own LINE Official Account and parses the reply, pending user review | Low–Medium |
| **Affiliate referral + evidence reconciliation** | Referral through an affiliate program (e.g. GoWabi); the booking happens on their site; confirmation evidence comes from the forwarded receipt | Low |

**Suggested phase 2 sequencing:** concierge-assisted, hosted-handoff and LINE OA relay first, to prove booking demand and volume; direct merchant self-host as the medium-term play for one or two categories (salons, sports courts); then use the resulting volume to re-approach Hungry Hub or TableCheck for a private-API partnership.

Google Calendar push is no longer a conditional extra: calendar-connected reminders are the current scope (PF-01).

## 3. Features and functionality (current)

Source: [Proposed features](../planning/proposed-features.md). Earlier F-IDs map to the PF IDs below in that document.

| Status | Features |
|---|---|
| **Current scope (user-confirmed)** | PF-01 Calendar-connected reminders, on top of the base obligation system: LINE onboarding and verified identity, text capture with AI-suggested fields and source text retained, validated dates and amounts, recurring reminders, snooze and completion recording, user-reported vs. externally verified status kept distinct, privacy controls, export/deletion |
| **Dropped 17 Nov (was proposed pilot core)** | PF-02 Receipt and transaction capture (Gmail), PF-03 Spending categorization, PF-04 Budget tracking, PF-05 Weekly spending summary, PF-07 Manual expense capture |
| **Recommended early extension** | PF-06 Bill and subscription tracking |
| **Candidates after validation** | PF-08 Expense reimbursement, PF-09 Follow-up tracking, PF-10 Daily planning, PF-11 Purchase and refund tracking, PF-12 Meeting preparation |
| **Phase 2** | PF-13 Booking engine (see §2) |
| **User-requested, details provisional** | PF-14 Three selectable assistant personalities |
| **Deferred** | Selective family sharing (earlier F05): in the 12 Nov baseline, but not a priority as of 17 Nov. Out of the April scope |
| **Deferred / excluded** | Voice input, telephone booking, tax-document checklist, autonomous banking or tax filing |

**The spending pilot was dropped on 17 November.** It was a proposal with an estimate of 65–110 team-hours, which does not fit the remaining capacity alongside the base plan under the planning-case scenario (timeline §6). A competitor check also found the space already served (YES BOSS!, MAKE by KBank, MeowJot).

## 4. Requirements that shape the build

- **Authorization:** every access to shared data is resource-specific and server-enforced; household membership alone grants nothing.
- **Evidence integrity:** the system must never report an uncertain outcome as confirmed, nor invent a missing field.
- **Idempotency:** every state-changing action (reminder, calendar write, receipt import, future booking) uses an idempotency key — duplicates must not create duplicate obligations or duplicate calendar events.
- **Locale:** Thai/English mixed input, Buddhist/Gregorian date normalization, `Asia/Bangkok` as default timezone, explicit per-user override.
- **Privacy (PDPA):** lawful basis per processing purpose, minimization, retention by data class, deletion that propagates to derived data (OCR text, indexes) and expires out of backups.
- **Capacity constraint:** 3 people × 3–4 h/week. Every requirement above is chosen to be enforceable by infrastructure (database constraints, managed OAuth scopes) rather than by hand-written application discipline, because the team has no spare review capacity for the latter.

## 5. Database design and recommendation

Full schema and rationale: [docs/design/database-design.md](../design/database-design.md).

- **Model:** a single relational schema (users, sharing_grant, obligation, source_document/extracted_claim, action_request/approval, connector_grant, notification, audit_event) with `jsonb` columns absorbing semi-structured fields (extracted-claim locations, audit metadata) — no separate document store needed.
- **Why relational, not NoSQL:** the hard requirements above (resource-level sharing, atomic state transitions, non-mergeable status states, money correctness) map onto Postgres features — foreign keys, transactions, `check` constraints, Row-Level Security — that a document database would require us to hand-rebuild and separately test.
- **Row-Level Security:** sharing/revocation (§4 above) is enforced as a database policy, not an application convention — a revoked grant loses access on the very next query.
- **Provider:** **Supabase (managed Postgres, Singapore region)** for the prototype — zero-ops, free/low tier fits the unfunded pilot, built-in Auth/Storage/RLS tooling matches our needs directly. **AWS Aurora PostgreSQL (`ap-southeast-1`)** is the named graduation target if the project becomes a funded commercial pilot — same SQL dialect, so this is a scale-up, not a rewrite.
- Calendar-connected reminders (PF-01) add no new table — they reuse `connector_grant`, already in the schema, for the OAuth token reference. The proposed spending pilot (PF-02 to PF-05) **does** need new tables (transactions, categories, budgets, import status and links between related records); these are not yet designed (database design §7).
- For phase 2 booking, the non-API alternatives (§2) need no schema redesign: `merchant.integration_type` already enumerates `direct_api` / `hosted_handoff` / `telephone` / `manual`; add `line_oa_relay` as a fifth value. `booking.status` already models the full request → offer → approval → confirmed/uncertain → completed/cancelled lifecycle regardless of which integration type fulfills it.

## 6. Other items for advisor input

- **Timeline:** proposal due 22 Nov 2026 (architecture/design finalized, no working implementation required); evaluated prototype due 30 Apr 2027. See the [timeline](../planning/timeline-and-development-phases.md) for phases, gates and open decisions.
- **Decision needed from advisor:** confirm the current scope (calendar-connected reminders) and advise on which differentiating feature to add. A competitor check on 17 November found seven LINE assistants advertising the same core, so the proposal needs one thing they do not offer.
- **Phase 2 booking:** when the time comes, confirm that concierge-assisted booking will be disclosed as human-assisted, not assistant-executed, and choose the first pilot category (salons vs. sports courts).
- **Open risk carried from the brief:** feature parity with existing reminder apps remains rated 4/10 differentiation; the team's mitigation is execution quality (capture accuracy, evidence rules), which still needs to be demonstrated, not assumed.
- **Not yet decided:** named role assignments, approved budget lines (hosting, model API, LINE plan), exact AI models, and PDPA counsel review.
