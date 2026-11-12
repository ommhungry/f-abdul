# Advisor Briefing — Thailand LINE AI Personal Assistant

**Date:** 12 November 2026 · **Status:** Draft for advisor review, pre-proposal (submission 22 Nov 2026) · **Full detail:** [architecture brief](../research/business-and-architecture-brief.md), [charter](../governance/project-charter.md), [database design](../design/database-design.md)

## 1. Project idea

A LINE-based AI assistant for working adults in Thailand (Bangkok-first) that turns a forwarded bill, document, or appointment message into a tracked obligation — due date, amount, responsible person, completion status — and follows it through to completion. Primary user: an individual managing their own bills, rent, and renewals. Secondary: the same person selectively sharing specific obligations with family, without exposing their full account.

**Pitch:** *Send it once. Know what needs doing, get help doing it, and keep track of what is actually finished.*

**Positioning vs. competitors (Abdul, Lamai, Cole):** reminders and calendar sync already exist in the market. Our differentiation is **capture-to-completion trustworthiness** — distinguishing *user-reported* from *externally verified* completion, source-grounded extraction (no silently invented dates/amounts), and resource-level family sharing with revocation — not any single integration.

## 2. Booking stays in scope — via non-API implementation paths

The brief's booking-feasibility research (§6) found that major Bangkok booking platforms (Hungry Hub, TableCheck, GoWabi) require a private-API partner agreement, which a pre-revenue student team cannot secure before the pilot. Rather than dropping booking, we route around the blocked path: none of the alternatives below require third-party API approval.

| Alternative | How it works | API/partner approval needed? | Build cost |
|---|---|---|---|
| **Concierge-assisted booking** | Automated request/offer/approval flow; a teammate places the actual call/LINE message to the merchant and logs the outcome | None | Low — already the brief's own recommended pilot design (§6); must be disclosed as human-assisted per charter's prototype boundary |
| **Direct merchant self-host** | Recruit small independent merchants with no existing booking platform (salons, sports courts still using a paper ledger or LINE chat); we build their booking system | None — no third party in the loop | Medium; doubles as a durable merchant-access moat (brief §4) |
| **Hosted-handoff deep link** | Pre-filled link into a merchant's existing hosted booking page (e.g. TableCheck), opened in a LIFF webview; user completes the final tap | None — linking to a public hosted page | Low |
| **LINE OA-to-OA relay** | Assistant sends the structured request as an ordinary customer message to the merchant's own LINE Official Account and parses the reply, pending user review | None — standard LINE messaging | Low–Medium |
| **Affiliate referral + evidence reconciliation** | Referral/discovery via an affiliate program (e.g. GoWabi); booking write happens on their site; confirmation evidence comes from the forwarded receipt | Affiliate signup only, not a write-API | Low |

**Recommended sequencing:** run *concierge-assisted + hosted-handoff + LINE OA relay* for the pilot (near-zero build cost, no API blocker) to prove booking demand and volume; treat *direct merchant self-host* as the medium-term differentiation play for one or two flagship categories (salons/sports courts); use the resulting volume and evidence to re-approach Hungry Hub/TableCheck for a private-API partnership later, from proof rather than a cold ask.

Google Calendar push (app-created scope) remains worth adding as a low-risk convenience feature in parallel — but it is feature-parity with Lamai/Abdul, not a differentiator, so it should not replace booking in the pitch.

**Recommendation to advisor:** keep booking (F08) in baseline, scoped to the non-API alternatives above for the pilot; approve Calendar push as an additional minor feature; confirm the pitch continues to lead with capture/evidence/sharing quality plus demonstrated booking completion, not any single connector.

## 3. Features and functionality (current baseline)

| ID | Feature | Status |
|---|---|---|
| F01 | LINE onboarding, verified identity, account recovery | Required baseline |
| F02 | Text capture with AI-suggested fields; source text retained; uncertain fields require confirmation | Required baseline (image/OCR extension conditional) |
| F03 | Validated dates/amounts, recurring reminders, snooze, completion recording | Required baseline |
| F04 | Upcoming-obligations view; user-reported vs. externally verified status kept distinct | Required baseline |
| F05 | Selective family sharing — invite, accept, assign, revoke a single obligation | Required baseline |
| F06 | Authorization, audit, privacy controls, export/deletion, cost monitoring (prototype scale) | Required baseline |
| F07 | Google Calendar push (app-created calendar scope) | Conditional — low-risk parity feature |
| F08 | Booking (one pilot category) | **Retained in baseline** — via concierge-assisted / hosted-handoff / LINE OA relay (§2), not a private-API integration |
| F09 | Tax-document checklist | Conditional, capacity-permitting |
| F10 | Voice input, Gmail ingestion, telephone booking | Deferred by default |

## 4. Requirements that shape the build

- **Authorization:** every access to shared data is resource-specific and server-enforced; household membership alone grants nothing.
- **Evidence integrity:** the system must never report an uncertain outcome as confirmed, nor invent a missing field.
- **Idempotency:** every state-changing action (reminder, calendar push, future booking) uses an idempotency key — duplicates must not create duplicate obligations or duplicate calendar events.
- **Locale:** Thai/English mixed input, Buddhist/Gregorian date normalization, `Asia/Bangkok` as default timezone, explicit per-user override.
- **Privacy (PDPA):** lawful basis per processing purpose, minimization, retention by data class, deletion that propagates to derived data (OCR text, indexes) and expires out of backups.
- **Capacity constraint:** 3 people × 3–4 h/week. Every requirement above is chosen to be enforceable by infrastructure (database constraints, managed OAuth scopes) rather than by hand-written application discipline, because the team has no spare review capacity for the latter.

## 5. Database design and recommendation

Full schema and rationale: [docs/design/database-design.md](../design/database-design.md).

- **Model:** a single relational schema (users, sharing_grant, obligation, source_document/extracted_claim, action_request/approval, connector_grant, notification, audit_event) with `jsonb` columns absorbing semi-structured fields (extracted-claim locations, audit metadata) — no separate document store needed.
- **Why relational, not NoSQL:** the hard requirements above (resource-level sharing, atomic state transitions, non-mergeable status states, money correctness) map onto Postgres features — foreign keys, transactions, `check` constraints, Row-Level Security — that a document database would require us to hand-rebuild and separately test.
- **Row-Level Security:** sharing/revocation (§4 above) is enforced as a database policy, not an application convention — a revoked grant loses access on the very next query.
- **Provider:** **Supabase (managed Postgres, Singapore region)** for the prototype — zero-ops, free/low tier fits the unfunded pilot, built-in Auth/Storage/RLS tooling matches our needs directly. **AWS Aurora PostgreSQL (`ap-southeast-1`)** is the named graduation target if the project becomes a funded commercial pilot — same SQL dialect, so this is a scale-up, not a rewrite.
- Calendar push (F07) adds no new table — it reuses `connector_grant`, already in the schema, for the OAuth token reference.
- Booking's non-API alternatives (§2) need no schema redesign either: `merchant.integration_type` already enumerates `direct_api` / `hosted_handoff` / `telephone` / `manual`; add `line_oa_relay` as a fifth value. `booking.status` already models the full request → offer → approval → confirmed/uncertain → completed/cancelled lifecycle regardless of which integration type fulfills it.

## 6. Other items for advisor input

- **Timeline:** proposal due 22 Nov 2026 (architecture/design finalized, no working implementation required); evaluated prototype due 30 Apr 2027.
- **Decision needed from advisor:** sign off on keeping booking in baseline via the non-API alternatives in §2 (specifically: disclosing concierge-assisted booking as human-assisted, not assistant-executed), and on which single pilot category/merchant set (salons vs. sports courts) to recruit first.
- **Open risk carried from the brief:** feature parity with existing reminder apps remains rated 4/10 differentiation; the team's mitigation is execution quality (capture accuracy, evidence rules, sharing UX), which still needs to be demonstrated, not assumed.
- **Not yet decided:** named role assignments, approved budget lines, and PDPA counsel review — all still open per the charter (§4, §10).
