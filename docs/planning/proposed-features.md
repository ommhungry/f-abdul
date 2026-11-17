# f-abdul proposed features

Prepared on 15 November 2026 for Senior Project.

Related design document — [AI implementation, routing, and personalities](../design/ai-implementation.md).

## Status and current direction

This document records the feature discussion and recommendations for team review. The user confirmed that the current functional scope consists of reminders connected to calendars. Booking moves to phase 2 because access to the private APIs of the booking platforms being considered depends on partnerships and evidence of a user base. Other features below are proposals awaiting prioritization and validation.

The working audience is office workers and individuals managing everyday obligations in Thailand. The proposed interface is LINE. The previously recorded capacity is three students contributing 3–4 hours each per week. Proposal submission is 22 November 2026 and project completion is 30 April 2027. These capacity and dates come from the earlier project context and should be checked when the team approves a revised plan.

## Proposed product direction

Extend calendar reminders with a spending-and-bills assistant that reduces manual record keeping. Collect supported transaction emails or receipts, turn them into editable records, and connect those records to budgets, upcoming bills, and follow-up actions.

The strongest proposed benefit is reducing the effort of maintaining useful financial records. Automatic capture, timely summaries, and reminders could encourage repeat use. User interviews and a limited pilot must establish demand and willingness to connect email.

The recommended first package combines the existing reminders, bill and subscription tracking, and a limited spending summary. Expense reimbursement tracking is the next candidate if office-worker interviews show frequent claims.

## Feature register

| ID | Feature | Proposed behavior | Status |
| --- | --- | --- | --- |
| PF-01 | Calendar-connected reminders | Surface upcoming obligations and send reminders through LINE | User-confirmed current scope |
| PF-02 | Receipt and transaction capture | Import supported emails, extract transaction details, preserve a source reference, and allow corrections | Proposed pilot core |
| PF-03 | Spending categorization | Assign an editable category and remember user corrections where appropriate | Proposed pilot core |
| PF-04 | Budget tracking | Compare recorded spending with user-set category budgets and show remaining allowance | Proposed pilot core |
| PF-05 | Weekly spending summary | Report totals, changes, remaining budgets, and gaps in available data | Proposed pilot core |
| PF-06 | Bill and subscription tracking | Record amount, due date, renewal terms, and user-confirmed cancellation deadlines | Recommended early extension |
| PF-07 | Manual expense capture | Accept a LINE message such as “Lunch 80 baht” for purchases missing from connected sources | Proposed pilot support |
| PF-08 | Expense reimbursement | Collect work receipts and track claim preparation, submission, and reimbursement | Candidate after validation |
| PF-09 | Follow-up tracking | Track unresolved requests, replies, refunds, and applications with scheduled follow-ups | Candidate |
| PF-10 | Daily planning | Suggest calendar slots for user-specified tasks and confirm changes before writing events | Candidate |
| PF-11 | Purchase and refund tracking | Associate purchases with receipts, stated return deadlines, and refund status | Candidate |
| PF-12 | Meeting preparation | Retrieve relevant notes and unfinished actions before a meeting | Candidate with broader integration needs |
| PF-13 | Booking engine | Execute bookings through supported channels after integration and partnership feasibility are established | User-directed phase 2 |
| PF-14 | Assistant personality selection | Choose among three personalities during onboarding, with consistent style across model changes and an option to switch later | User-requested direction, character details provisional |

### Mapping from earlier feature IDs (added 17 November)

The PF identifiers are the current ones. The earlier F-IDs (7 November kickoff and 12 November advisor briefing) map as follows. The M/S/C and SRS identifiers belong to material on the unmerged `jui` and `vj` branches and are superseded.

| Earlier ID | Earlier feature | Now |
| --- | --- | --- |
| F01–F04, F06 | Onboarding and identity; text capture; validated dates and recurring reminders; upcoming obligations and evidence-aware status; access control, audit, deletion and export | The base reminder system under PF-01. Not separate PF items; sized as the foundation packages in the [timeline](timeline-and-development-phases.md) |
| F05 | Selective family sharing | Not in the 15 November register. **Deferred, not a priority (17 November)**. The [database design](../design/database-design.md) keeps the sharing table for later |
| F07 | Google Calendar push | PF-01, now current scope |
| F08 | Booking | PF-13, phase 2. The non-API routes in the advisor briefing (concierge-assisted, hosted handoff, LINE OA relay) stay as phase 2 options |
| F09 | Tax-document checklist | Not carried forward; no current owner |
| F10 | Voice, Gmail ingestion, telephone booking | Gmail ingestion is now PF-02, a proposed pilot core. Voice and telephone booking remain deferred |
| F11 | Autonomous banking and tax filing, unrestricted group reading, universal booking | Still excluded |

Note that the unmerged combined proposal listed Google Calendar and Gmail OAuth as out of scope. The 15 November direction reverses that: both are now in or proposed, and the Google verification dependencies below apply.

## Spending assistant pilot

### Input and output

Start with two or three specific receipt or transaction-email formats chosen from actual pilot users. Extract merchant, transaction date, amount, currency, transaction type, and a source reference. Record uncertainty and give users an easy way to correct or exclude a record.

Use a small set of editable spending categories. Let the user choose a monthly budget for each category. Compute totals and remaining allowances in application code. Use the LLM to interpret input, suggest categories, and explain verified calculations.

For example, with complete data for the selected category, the assistant could report that ฿3,600 of a ฿5,000 dining budget has been spent, leaving ฿1,400 over the next two weeks, or an average allowance of ฿700 per week. Label summaries with their period and connected-source coverage.

A payday-based available-money forecast additionally needs the user’s available money, other spending, and outstanding commitments. It should remain a later extension until those inputs are dependable.

### Suggested interaction

1. The user connects a supported email account or supplies selected receipts.
2. The system imports relevant records and identifies duplicate or uncertain entries.
3. The user reviews corrections and sets category budgets.
4. The assistant sends a weekly summary and relevant bill reminders.
5. The user can ask about spending, adjust a budget, or add a missing expense through LINE.

### Reconciliation requirements

A merchant receipt and a card notification can describe the same purchase. Credit-card bill payments can duplicate expenses already captured from purchases. Transfers, refunds, cancellations, and instalments also require explicit treatment.

The ledger should preserve source identifiers, import status, and links between related records. Repeated imports must be safe. Unknown transaction types should enter review rather than silently changing a spending total.

Recurring charges should be presented as suspected subscriptions until sufficient evidence or user confirmation establishes their recurrence and terms. A recorded bill becoming due and an expense actually being paid are separate events.

### Coverage assumptions

Email coverage must be measured per pilot user. Cash payments and transactions delivered only through another channel will be absent unless the user supplies them. SCB documents debit-card alerts through LINE SCB Connect. K PLUS documents financial-transaction alerts within its application. These examples establish that email availability cannot be assumed for every transaction. [SCB debit-card alerts](https://www.scb.co.th/en/personal-banking/cards/debit-cards/announcement/scbconnect-debit.html) · [K PLUS notifications](https://www.kasikornbank.com/en/personal/digital-banking/kplus/help/Pages/notifications.html)

The product should describe its totals as spending recorded from the selected sources. Broad claims about total spending or affordability require adequate coverage.

### Gmail integration

A Gmail MCP can expose email tools to the assistant. Its actual contribution depends on the provider’s authorization, background-processing, and account-isolation capabilities. A specific provider has not been selected.

The proposed implementation imports records on a schedule or through mailbox events, stores normalized transactions, and lets the assistant query those records. Conversation history alone is insufficient for persistent accounting.

Gmail’s read-only scope is restricted. Public use generally requires verification unless an exception applies. Handling restricted data on servers can require a security assessment. Development and testing exceptions exist, and Google’s external Testing mode generally expires relevant test-user authorization after seven days. These are integration and rollout dependencies to evaluate with the chosen provider. [Gmail scopes](https://developers.google.com/workspace/gmail/api/auth/scopes) · [Restricted-scope verification](https://developers.google.com/identity/protocols/oauth2/production-readiness/restricted-scope-verification) · [OAuth app audience](https://support.google.com/cloud/answer/15549945?hl=en)

For the pilot, scope email processing to relevant records, protect account credentials, enforce access per user, and provide disconnect and deletion handling. Define what is stored and what is sent to the model in the eventual implementation design.

## Value and evaluation

| Hypothesis | Proposed evaluation |
| --- | --- |
| Automatic capture reduces effort | Compare correction and recording time with the participant’s existing approach |
| Supported emails provide useful coverage | Compare captured purchases and amounts against a participant-checked reference |
| Extracted records are dependable | Measure accuracy separately for amount, date, merchant, currency, and transaction type |
| Totals remain correct after re-imports and refunds | Test duplicates, repeat imports, reversed purchases, and related notifications |
| Summaries influence decisions | Ask whether users changed a budget or action after a summary |
| The feature encourages continued use | Measure repeat use and reasons for returning during the pilot |
| Users accept the data access involved | Observe connection completion and record reasons for declining |

Start with approximately five target users and two weeks of redacted records. Obtain permission to use those records and preserve their relationships while redacting, so duplicate and refund tests remain meaningful. This is a proposed discovery exercise, separate from the eventual formal project evaluation.

If coverage is weak, concentrate the first release on identifiable bills, subscriptions, and reimbursements. Set quantitative acceptance thresholds after the initial dataset establishes the baseline.

## Effort estimate

These are preliminary engineering estimates for incremental work, assuming the LINE bot, database, and reminder scheduler already function. They exclude external verification waiting time and require revision after a technical spike.

| Work | Estimated team-hours |
| --- | --- |
| Gmail authorization, initial import, background updates, and reconnection | 15–25 |
| Supported extraction, categorization, and transaction reconciliation | 20–35 |
| Budget calculations, LINE summaries, and corrections | 15–25 |
| Data controls, evaluation dataset, and end-to-end testing | 15–25 |
| Total | 65–110 |

At 9–12 combined team-hours each week, this is approximately 6–12 weeks if the team devotes its available time to this work. Supporting many banks, complex statements, multiple currencies, or broad financial forecasting would increase the scope. The estimate should be incorporated into the overall project plan before approval.

## Other office-worker use cases

| Candidate | Example need | Relative effort | Recommendation |
| --- | --- | --- | --- |
| Bill and subscription tracker | What renews before payday, and when must I cancel? | Low–medium after receipt capture | Best early companion to spending summaries |
| Reimbursement assistant | Collect work receipts and show which claims remain unpaid | Medium | Prioritize if users submit frequent claims |
| Follow-up tracker | Remind me if this request is still unresolved on Friday | Low manually, medium with email detection | Reuses the reminder and completion mechanisms |
| Daily planning assistant | Find a free hour for this task before Thursday | Low–medium | Reuses calendar access |
| Purchase and refund tracker | Keep the receipt, return deadline, and refund status together | Medium | Reuses receipt capture and follow-up |
| Meeting preparation assistant | Show my notes and unfinished actions before the meeting | Medium–high | Requires dependable access to relevant work material |

Relative effort assumes an existing reminder system. Email-based candidates share the Gmail access dependencies above. Return and cancellation deadlines should come from recorded terms or user confirmation.

## Open decisions

- Which banks, cards, and merchants generate the pilot users’ transaction emails?
- What proportion of purchases and spending amounts do those emails capture?
- Which two or three formats form the initial supported set?
- What Gmail integration provides appropriate per-user access and background operation?
- Is the first financial use case category budgeting, recurring bills, or work reimbursement?
- What team capacity and budget are available after the existing reminder work?
- What accuracy and correction burden will make the feature acceptable to pilot users?
- Which exact models, hosting configuration, and data-handling terms should implement the endorsed API-based routing approach?

The project owner endorsed API-based inference, routing between small and stronger models, and three selectable personalities in the follow-up discussion. The [AI implementation document](../design/ai-implementation.md) records this direction, hosting alternatives, response-generation rules, and evaluation approach. Exact providers, model versions, and character identities require validation.
