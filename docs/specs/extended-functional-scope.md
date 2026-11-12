# Extended functional scope

| Field | Value |
|---|---|
| Author | vj |
| Version | 0.1 draft, 12 November 2026 |
| Status | For team review. Not approved. Nothing here changes the baseline until the team decides |
| Builds on | Combined proposal v0.2 and SRS v0.9 (branch `jui`), charter v0.3 (branch `charter-timeline-v0.3`) |
| Team | 3 students, 3-4 hours a week each |

Evidence tags follow the team's convention. `[V]` means a primary source was opened. `[S]` means a secondary source or search result. `[I]` means our inference. `UNVERIFIED` means not checked. Hour figures are my **estimates**. The web facts in section 4 come from search results only. I did not open the primary pages, so none of them is `[V]`.

## 1. Purpose

This document lists every functionality of the system in one place:

1. the baseline in the combined proposal (section 3), and
2. four additions I propose: a personal file vault, calendar connectors, Gmail connectors, and a morning brief (section 4).

It says what is feasible inside LINE, what each addition would cost, and which parts clash with the current plan. Section 7 lists the decisions the team needs to make.

## 2. Rules that apply to every function

These come from the combined proposal and are kept for the additions.

- **The AI interprets and code decides.** The model classifies and fills a schema. It cannot send, book, share, delete or mark anything done (`AI-BEH-01`).
- **Nothing leaves the system without the user's confirmed tap.** Code checks the tap: right user, right task, right content version, not expired, used once (`AI-HITL-01`).
- **We only show a state we can observe.** A request that was opened or sent is never shown as a booking.
- **Text from businesses, emails, web pages and files is data, never instructions** (`AI-PI-01`).
- **The AI is disclosed** in the welcome message, the privacy notice and every card (`AI-EXP-01`).
- **Testers are 20 or over, or married** (`LEG-05`).
- **Sensitive personal documents are out of scope** for the prototype (charter section 11). This matters for the vault.

## 3. Baseline functions (from the combined proposal)

### 3.1 Must - the April deliverable

| ID | Function | What it does |
|---|---|---|
| F1 | Onboarding | Welcome with AI disclosure, privacy notice, acceptance, age statement |
| F2 | Capture an obligation | Thai, English or mixed text; Buddhist-era and Thai relative dates; source text kept |
| F3 | Confirm, edit, cancel | Card with Confirm, Edit (one field at a time in chat) and Cancel; approval bound to the exact content |
| F4 | Reminders | One-off reminders with snooze, done and quiet hours |
| F5 | Completion with evidence | User-reported, or slip or screenshot attached |
| F6 | Compose a request to a business | Service, 2-3 time options, user nickname, AI disclosure |
| F7 | Route the request | Channel router picks LINE handoff or copy-and-send from the venue table (10-15 hand-checked venues); asks "Did you send it?" |
| F8 | Follow up | Follow-up inside opening hours, record the answer, expire, reminder for the agreed time |
| F9 | Withdraw | Within 10 minutes while unanswered |
| F10 | One lifecycle | Honest states: Captured, Confirmed, Handoff opened, Request sent, User-confirmed, Withdrawn, Expired, Done (user-reported), Done (evidence), Uncertain |
| F11 | Data export and delete | By chat command; the team runs the script within 30 days |
| - | Safety and operations | Signature check on webhooks, no duplicate tasks from redelivered events, separate development and pilot channels, alerts, logs without personal content, isolation tests, backup restore drill |

### 3.2 Should (in this order)

| ID | Function |
|---|---|
| S0 | Reply page: a business answers with one tap on a web page, no sign-up. Promoted to Must if the calendar allows |
| S3 | Recurrence: monthly first, then weekly and daily |
| S4 | "Upcoming in the next N days" list as a chat message |
| S1 | Share one item with one invited person: read status, mark done, revoke |
| S2 | Operator console: a team member phones a phone-only shop and records the outcome |

### 3.3 Could

C1 photo of one bill format read by a vision model; C2 bill split with a PromptPay QR; C3 `.ics` file for a confirmed booking; C4 share-target picker; C5 LIFF pages; C6 second LLM compared on the held-out set.

### 3.4 Won't (current plan)

Direct booking (Level 3), partner APIs, browser agents, email, AI phone calls, group polls in LINE groups, **Google Calendar or Gmail OAuth**, payments or subscriptions, venue recommendations from Google or Wongnai data, escalation-until-acknowledged reminders, voice notes, วันพระ recurrence, medical requests, testers under 20, merchant dashboard.

## 4. Proposed additions

Priorities in this section are my **proposals** for the team to accept or change.

### A1. Personal vault: keep files and facts, pull them back later

**What the user sees**
- The user sends a file or photo in the LINE chat and says what it is ("car insurance, expires March").
- The bot saves it, asks one confirming question, and files it under a name and tags.
- Later the user asks "send my insurance policy" or "when does my car insurance expire?". The bot returns the file or the answer, with the source file named.
- The user can list, rename, and delete items. Deleting removes the stored copy.

**Functions**
| ID | Function |
|---|---|
| A1.1 | Receive a file, image, video or audio message and copy it to our storage immediately |
| A1.2 | Ask the user to confirm the name, tags and any date to track (a confirmation card, as in F3) |
| A1.3 | Find an item by name, tag or date |
| A1.4 | Return the item to the user |
| A1.5 | Answer a question from the item's text, quoting the source (needs text extraction or OCR) |
| A1.6 | Optional: turn a date in a stored item into a reminder (a confirmed obligation) |
| A1.7 | List, rename and delete items; deleting removes the stored copy |
| A1.8 | Export everything the user stored (extends F11) |

**Is it feasible in LINE?** Yes for receiving. Partly for returning files.
- **Receiving.** LINE's overview says a bot can get images, videos, audio and files that users send `[S, developers.line.biz overview, 12 Nov 2026]`. The webhook sends a message ID, and the bot downloads the content with a separate call to `api-data.line.me` `[S]`. The SDK comment lists image, video and audio only, so confirm that files are included `UNVERIFIED`.
- **Content disappears.** LINE deletes user-sent content after some time `[S]`. The period was not found `UNVERIFIED`. The bot must copy the file at once.
- **Returning.** I found no outbound "file" message type `[S]`, `UNVERIFIED`. Images, video and audio can be sent from an HTTPS URL. A PDF or Office file would probably go as a short-lived link. Test this in the P1 spike.
- **Browsing.** A LIFF page (C5) is the natural place to list and search items. In chat only, listing is limited to a few quick-reply buttons.

**Constraints and risks**
- **Sensitive documents.** The charter excludes sensitive personal documents for the prototype. Users will still try to store ID cards and bank papers. Add a plain warning at upload, a blocklist for obvious ID formats, and deletion on request.
- **PDPA.** Stored files are personal data. Record the basis, retention and deletion rules (`PRV-01`, `PRV-03`, `DR-04`). Extracted text sent to the LLM goes abroad (PDPA s.28), so the data transfer record in `PRV-02` applies.
- **Prompt injection.** A file can contain instructions. Treat file text as data (`AI-PI-01`).
- **Storage and cost.** Object storage with encryption at rest and per-user isolation. Set a per-user quota (for example 50 MB, my guess). OCR and model calls add per-use cost.
- **Wrong answers.** Q&A over stored documents can hallucinate. Always quote the source passage and name the file (`AI-HAL-01`).

**Effort (estimate):** A1.1-A1.4 and A1.7-A1.8: 20-30 h. Add A1.5-A1.6: 15-25 h more.
**Proposed priority:** A1.1-A1.4, A1.7, A1.8 as **Should**. A1.5 and A1.6 as **Could**.

### A2. Calendar connectors

**What the user sees**
- The user links their Google Calendar once.
- The morning brief and the "upcoming" list show their events next to their obligations.
- When a request is confirmed, the bot offers to add the booking to their calendar.
- Before proposing time options for a request, the bot avoids times that clash.

**Functions**
| ID | Function |
|---|---|
| A2.1 | Link and unlink a Google Calendar (user consent, revocable at any time) |
| A2.2 | Read events for the brief and the "upcoming" list |
| A2.3 | Suggest request time options that avoid existing events |
| A2.4 | Add a confirmed booking or obligation to the calendar after a Confirm tap |
| A2.5 | Calendar feed without a login: a private subscription link the user adds to Google or Apple Calendar, which shows our reminders and bookings (read-only for them) |

**Is it feasible in LINE?** Yes. The LINE side is just a link flow.
- **Linking.** The user opens a web page from a button in the chat and signs in with Google. This needs a LIFF page or a plain web page.
- **Google rules.** Calendar scopes are "sensitive", which needs Google app verification before general release `[S]`. In testing mode an external app is limited to a list of named test users (up to 100) and shows an "unverified app" warning `[S, UNVERIFIED for refresh-token expiry]`. A university pilot with 15-20 named testers fits testing mode. Check whether refresh tokens expire after 7 days in that mode, because that would force testers to relink weekly `UNVERIFIED`.
- **Lower-risk options.** `.ics` file for a booking (C3) and the subscription feed (A2.5) need no Google approval.

**Constraints and risks**
- Google Calendar OAuth is in the Won't list. Moving it needs a team decision.
- Calendar contents are personal data. Read only the fields needed (time, title, busy or free). Prefer free/busy for A2.3.
- Store tokens encrypted. Revoke on unlink or delete.
- Apple Calendar, Outlook and other providers are out of scope.

**Effort (estimate):** A2.1-A2.2: 10-16 h. A2.3: 3-5 h. A2.4: 4-6 h. A2.5: 5-8 h.
**Proposed priority:** A2.5 and C3 first (**Could**, no approval needed). A2.1-A2.4 as **Could**, testing mode only.

### A3. Gmail connectors

**What the user sees**
- Bills, booking confirmations and renewals in the user's email become proposed obligations in LINE: "Looks like your electricity bill of 1,240 baht is due 20 Nov. Track it?". The user confirms with a tap.
- No email is acted on without that tap.

**Functions**
| ID | Function |
|---|---|
| A3.1 | Forward-to-bot: the user forwards an email to a private address; we read it and propose an obligation |
| A3.2 | Gmail link: the user links Gmail read-only; we scan messages with a user-chosen label |
| A3.3 | Extract a due date, amount and sender from an email into the same confirmation card as F3 |
| A3.4 | Link and unlink; delete all stored email data on request |

**Is it feasible?** Not as a general feature. A narrow version is.
- **Google's rules are heavy.** Gmail read scopes are "restricted". An app that stores or passes restricted-scope data through its own server needs a security assessment (CASA) and an annual re-assessment `[S, Google Cloud support, nango.dev, 12 Nov 2026]`. Sources disagree on which Gmail scopes are restricted, so check the current scope reference `UNVERIFIED`. This is out of reach for a three-student project that ends in April.
- **A pilot version may be possible.** In testing mode with named test users, an unverified app can use restricted scopes `[S, UNVERIFIED]`. Confirm before relying on it.
- **Forward-to-bot (A3.1) avoids the restricted scope.** It needs an inbound email address and a parser, but no Google review.

**Constraints and risks**
- **Privacy.** Email is the most sensitive data in this document. Read only labelled messages, never the whole inbox. Show the user exactly what was read.
- **Prompt injection.** Emails are written by third parties. A malicious email can try to steer the model. The rule that the model cannot act, and that every action needs a tap, is the defence. Add emails to the 30-case injection test set.
- **LLM transfer abroad.** Email text sent to the model provider is a PDPA s.28 transfer. Record the basis and use no-training terms (`AI-SUP-02`).
- **Wrong extraction.** A wrong amount or date shown as fact causes real harm. Always show the extracted values with the source sentence and ask before saving (`AI-HAL-01`).
- Email is in the Won't list, and Gmail OAuth is in the Won't list. Moving either needs a team decision.

**Effort (estimate):** A3.1 + A3.3: 10-16 h. A3.2: 14-24 h more, including the privacy work. A3.4: 3-5 h.
**Proposed priority:** A3.1, A3.3, A3.4 as **Could**. A3.2 as **future work** unless the team accepts testing-mode-only.

### A4. Morning brief

**What the user sees**
- Every morning at a time they choose, one message: today's reminders, obligations due soon, appointments (from the calendar, if linked), requests still waiting for an answer, and, if A3 is built, bills found in email.
- The user can also ask for it at any time with a rich-menu button or a keyword.

**Functions**
| ID | Function |
|---|---|
| A4.1 | On-demand brief from a rich-menu button or keyword, built from existing data |
| A4.2 | Scheduled brief at a user-chosen time, on weekdays by default |
| A4.3 | Include calendar events (needs A2) |
| A4.4 | Include flagged emails (needs A3) |
| A4.5 | Quiet hours, pause, and one-tap opt-out |

**Is it feasible in LINE?** Yes, but the message quota is the limit.
- **Push cost.** A scheduled brief is a push message. The free plan allows 300 messages a month `[V, judge-vj spot-check 10, 9 Nov 2026]`. One brief per weekday is about 22 messages a month per user. With 15-20 testers that is 330-440 messages, over the quota before any reminders or follow-ups.
- **On-demand is free.** A brief the user asks for is sent as a reply, which has no push cost. LINE will charge for reply messages above 25,000 a month from February 2027 `[V, judge-vj spot-check 9]`, which we won't reach.
- **Ways to stay inside the quota:** make the scheduled brief opt-in, limit it to weekdays, send it only when there is something to say, count pushes per tester (`OPS-06`), or buy a paid plan. A paid plan is a team decision (TD5).
- **Content is mostly existing data.** A4.1-A4.2 reuse reminders, obligations and request states. They add only a scheduler, a formatter and a Flex message.

**Constraints and risks**
- Do not let the brief become another source of stress. Keep it short and let the user turn it off.
- A brief that mentions a request must show its honest state (for example "sent, no answer yet"), not "booked".
- The model may write the brief text, but code must supply the facts, so no item appears that is not in the data.

**Effort (estimate):** A4.1-A4.2 with A4.5: 5-8 h. A4.3: 2-3 h (after A2). A4.4: 2-3 h (after A3).
**Proposed priority:** A4.1 and A4.2 as **Should**, immediately after S4, because they reuse the same data.

## 5. Feasibility summary

| Addition | Feasible inside LINE? | Main limit | Effort (est.) | Proposed priority |
|---|---|---|---|---|
| A1 Vault: store and return | Yes, with caveats | LINE deletes content after a while; PDFs probably go as links; sensitive documents | 20-30 h | Should |
| A1 Vault: question answering | Yes | Hallucination, OCR cost, LLM transfer abroad | 15-25 h | Could |
| A2 Calendar connectors | Yes | Google verification; testing mode only; refresh-token expiry unverified | 22-35 h | Could |
| A2 Calendar feed / `.ics` | Yes | None significant | 5-8 h (+C3) | Could |
| A3 Gmail via forwarding | Yes | Inbound email handling; privacy; injection | 10-16 h | Could |
| A3 Gmail OAuth | Not for production | Restricted scope, CASA, annual re-assessment | 14-24 h | Future work |
| A4 Morning brief, on demand | Yes | None significant | 5-8 h | Should |
| A4 Morning brief, scheduled | Yes | Free push quota of 300 a month | included | Should, opt-in |

## 6. Effect on hours

From the combined proposal: Must is 111-157 h (midpoint 134 h). In the worst-case calendar (scenario C) at 3.5 h a week, the margin is 7.75 h. All Should items together add 39 h, and all Could items add 35 h `[V, combined proposal section 6.2-6.3]`.

My additions total about 55-100 h (sum of the estimates above). They **do not fit** beside the current Should and Could list. Adding even A4 and the A1 basics (25-38 h) would need the team to drop other Should items (for example S1 sharing, 15 h, and S2 operator console, 8 h) or move to a better calendar scenario.

These features also pull the product toward Abdul and general AI assistants, where competition is strongest. The combined proposal's difference from Abdul is that it acts and tracks outcomes. The team should decide whether retention features are worth the trade.

## 7. Decisions needed from the team

| # | Decision | Options | My recommendation |
|---|---|---|---|
| X1 | Which additions enter the scope? | None; A4 only; A4 + A1 basics; all | A4 + A1 basics, if the team swaps out S1 and S2 |
| X2 | Move Google Calendar OAuth out of Won't? | Keep Won't; testing mode only | Keep Won't for now; do A2.5 and C3 first |
| X3 | Move email handling out of Won't? | Keep Won't; forward-to-bot only | Forward-to-bot only, as Could |
| X4 | Paid LINE plan for the brief? | Free plan with opt-in and caps; paid plan | Free plan with opt-in, decide at TD5 |
| X5 | Vault rules | Block sensitive documents; allow with warnings | Warn and block obvious ID formats |
| X6 | Do the P1 spike checks first | Test file receive, file return, content expiry on testers' phones | Yes, before costing A1 |

## 8. Things to verify before committing

1. How long LINE keeps user-sent content, and whether inbound `file` messages download like images.
2. How a bot can return a PDF or Office file to a user in a chat (probably a link).
3. Whether Google refresh tokens for apps in testing mode expire after 7 days.
4. Which Gmail scopes are restricted, and whether testing-mode use with named test users is allowed.
5. PDPA position on storing user files and sending extracted text to a model abroad.
6. Real push counts per tester, using the OPS-06 counter.

## 9. Sources (search results, not opened pages)

- LINE Messaging API overview: https://developers.line.biz/en/docs/messaging-api/overview/
- LINE Messaging API reference: https://developers.line.biz/en/reference/messaging-api/
- Google restricted scope verification: https://developers.google.com/identity/protocols/oauth2/production-readiness/restricted-scope-verification
- Google Cloud security assessment: https://support.google.com/cloud/answer/13465431
- Nango on Google security review: https://nango.dev/docs/api-integrations/google-shared/google-security-review
- Combined proposal, SRS v0.9 and judge-vj: branch `jui` of ommhungry/f-abdul
