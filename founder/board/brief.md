# Board brief: f-abdul (working name)

Prepared 17 November 2026 from the project's own documentation. Everything below is the project's stated position or research; items marked "untested" or "estimate" have no real-world evidence behind them. Currency is Thai baht (฿).

## The question for the board

Is this idea valid as a business? If not yet, what would have to be added or changed for it to become one?

## The idea

- What it is: an AI personal assistant inside LINE (the dominant chat app in Thailand). A user forwards a bill, document or appointment message; the assistant extracts due date and amount, asks about anything uncertain, saves a tracked obligation, reminds the user through LINE and writes it to their Google Calendar, and records completion.
- Who it is for: office workers and other individual adults in Thailand, Bangkok first, managing their own bills, rent, renewals and appointments.
- Pitch line: "Send it once. Know what needs doing, get help doing it, and keep track of what is actually finished."
- What it sells: a monthly subscription. Price hypotheses (untested): free tier with a small allowance; Personal at ฿79 or ฿99/month; a possible Household tier at ฿149–199/month.
- Where and how: online only, through a LINE Official Account. No acquisition channel has been tested. Candidates named: communities of working adults, renters and shift workers; user referrals; later, employer-funded benefit.

## Current scope (decided 15–17 November 2026)

- Current, confirmed scope: calendar-connected reminders on top of a base obligation system (LINE onboarding, text capture with AI-suggested fields, validated dates and amounts incl. Thai/Buddhist-era dates, reminders, snooze, completion recording, export/deletion).
- Proposed, not decided: a spending-and-bills assistant. Connect Gmail (or forward receipts), import transaction emails, turn them into editable records, categorize, set category budgets, weekly spending summary, bill and subscription tracking. Claimed benefit: removes the effort of manual record keeping. Known limits: many Thai bank alerts arrive through LINE or bank apps, not email, so email coverage is unmeasured; Gmail's read-only scope is restricted and requires Google verification for public use.
- Other candidates after validation: expense reimbursement tracking, follow-up tracking, daily planning, purchase/refund tracking, meeting preparation.
- Phase 2: booking (restaurants, salons, sports courts). Major Bangkok booking platforms need private-API partnerships the team cannot get without a user base. Non-API routes identified for later: human-assisted concierge, pre-filled links to merchants' hosted booking pages, messaging a merchant's LINE account, building booking for small merchants directly.
- Deferred: family sharing of individual obligations. Three selectable assistant personalities (Professional, Companion, Coach) are requested but provisional.

## What the team claims as differentiation

- Trustworthy completion: "user-reported done" and "externally verified done" are kept distinct; uncertain outcomes are never shown as confirmed.
- Source-grounded extraction: the AI never silently invents a date or amount; uncertain fields require user confirmation; the source text is kept.
- Proposed: low-effort capture of bills and spending.
- The team's own rating of present differentiation: 4/10. Its own overall rating: 6.5/10 today, 7.5/10 potential.

## Competitors (from public pages, snapshot 6 November 2026; reliability and commercial success of these products are unknown)

| Competitor | Advertised | Public pricing observed |
|---|---|---|
| Abdul | LINE assistant: personal and group reminders, recurring obligations, stored information, Google Calendar, family/team use | Free; annual plans ฿990, ฿1,490, ฿4,900 |
| Lamai | Tasks, group coordination, Google/Microsoft calendars, files, email assistance, voice input, phone-call reminders | Free; ฿169, ฿349, ฿890/month |
| Cole | LINE assistance, reminders, email/calendar actions, research, browser tasks, Thai-language phone calls on the user's behalf | Paid plans from US$19/workspace/month |

Substitutes: phone calendars, bank notifications, biller apps (e.g. the electricity authority's own app with bill notifications), notes, family chats.

The team's view of Abdul's moat (inference): brand recognition, habit, accumulated personal context, group distribution; not proprietary technology.

## Economics (all illustrative, from the research brief; nothing measured)

- At ฿99/month incl. 7% VAT, net revenue is about ฿92.52 per paying user.
- Variable cost per paying user (model usage, messaging, storage, payment fees, support) is unknown; scenarios of ฿30 / ฿60 / ฿80 leave ฿62.52 / ฿32.52 / ฿12.52 contribution.
- LINE messaging: Thai Pro plan ฿1,780/month for 35,000 messages, extra messages ฿0.06 each; push messages count, replies do not.
- Illustrative break-even: with ฿62.52 contribution and ฿300,000/month fixed cost, about 4,800 paying users before acquisition cost.
- Human work is expensive relative to price: two ten-minute interventions at ฿150/hour cost ฿50.
- Customer acquisition cost, conversion from free to paid, and retention: all unknown.

## Team, time and status

- Three university students, each 3–4 hours/week. Senior project. Proposal due 22 November 2026; evaluated prototype due 30 April 2027.
- No code exists. No named roles. No approved budget. No user interviews recorded. No paying or waiting users.
- Planned April deliverable: a small evaluated prototype with a supervised evaluation on invited adult testers using low-sensitivity data. It is explicitly not a paid market-validation pilot.
- Effort: the committed scope (base system plus calendar connection) is estimated at 113–160 team-hours against roughly 122–198 usable hours depending on holidays and exams. The proposed spending assistant is estimated at a further 65–110 hours and does not fit at the midpoint in any capacity scenario.
- Privacy: Thailand's PDPA applies; the product would handle bills, financial records and (if built) email content. No legal review has been done.

## Risks the team has already listed

Low willingness to pay; feature parity with existing assistants; setup burden; wrong deadlines or false confirmation; notification fatigue; support cost exceeding margin; privacy or credential incident; dependence on LINE and on model providers.
