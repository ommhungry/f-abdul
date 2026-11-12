# Competitor analysis: Abdul and the rest, and how we differentiate

| Field | Value |
|---|---|
| Author | vj |
| Version | 0.1 draft, 12 November 2026 |
| Status | For team review. Not approved |
| Builds on | The team's research on branch `jui`: Abdul teardown (8 Nov), Abdul passive recon (9 Nov), market and competitors (8 Nov), combined proposal v0.2 |
| New work in this file | A feature-by-feature comparison, a parity check of our scope against Abdul, and a differentiation strategy |

**Evidence tags.** `[V]` means a primary source was opened by the team member who wrote the cited file. `[S]` means secondary. `[I]` means our inference. `UNVERIFIED` means not checked. I did not re-open the team's primary sources. I ran two new searches on 12 Nov 2026 for fresh news about Abdul and for the assistants "Lamai" and "Cole". They found nothing about either, so those two names stay as the team recorded them (`UNVERIFIED` by me).

## 1. Summary

1. **Abdul reminds. It does not act.** Its only outside actions are LINE messages and edits to the user's own Google Calendar `[V, teardown s.2]`. It cannot contact a business, book, or confirm that anything happened.
2. **Abdul is further along than our plan on everything except acting.** It has recurring reminders, group mode, an escalation ("can't miss") mode, memory with up to 100 GB of files, Google Calendar read and write, web search, image generation, live payments, and about 42,000 LINE friends `[V, teardown; recon]`.
3. **Three of the four features I proposed in `docs/specs/extended-functional-scope.md` are things Abdul already does** (file memory, calendar, morning brief). Building them as standalone features would make us a weaker copy of Abdul on its home ground. Section 5 explains how to keep them only where they support booking.
4. **Our real difference is the closed loop on a request to a business:** draft it, one-tap send, follow up, record an honest outcome, and remind for the agreed time. No Thai consumer assistant does this today `[S, market s.1]`.
5. **The edge is thin.** A single copy of our feature list is easy. The harder parts to copy are the venue channel data, the honest state model, and shops answering through our reply page. The biggest threats are LINE itself and Abdul adding booking `[S/I]`.

## 2. Who we compete with

| Group | Players | What they do | Threat to us |
|---|---|---|---|
| **A. Assistants in LINE** | Abdul; "Lamai" and "Cole" (named in omm's brief, not re-checked) | Reminders, notes, calendar. Cole advertises Thai phone calls on paid plans `[V, omm brief s.4, via combined proposal]` | **High** for Abdul: same channel, same users |
| **B. The platform owner** | LINE Thailand (ActEngine AI for merchants, Seller Agent); LY Corp Japan (Agent i, LINE AI Reservation) | AI agents for merchants. In Japan an AI phone agent takes restaurant bookings `[V/S, market s.2]` | **Highest.** Owns the channel, pricing and policy. The Reply API becomes paid above 25,000 messages a month from Feb 2027 `[V, judge-vj check 9]` |
| **C. General AI assistants** | ChatGPT, Google AI Mode | Restaurant search and booking through US and European partners (OpenTable, Resy, Booksy). Google's "Ask for me" phones businesses in the US `[S, market s.2]` | **Medium.** Works in Thai, but few Thai merchants are on their partners `[I]` |
| **D. Booking marketplaces** | Wongnai (Reservations), Hungry Hub, Chope/Grab Dine Out, GoWabi, Fresha, QueQ, HDmall Jib OS | The shop is on their system, so the booking is real and instant `[S]` | **Medium.** They have supply. They are not assistants and do not live in the user's chat |
| **E. Merchant chatbots** | Botnoi, Chinda, Zaapi, KaoJai, SleekFlow, Crescendo Lab; Aiello AI calls on TableCheck | Help the shop answer chats and bookings, priced at ฿499-3,300 a month `[S, market s.2]` | **Low now.** They serve the shop side. They could meet our reply page in the middle |
| **F. The user doing it themselves** | Walk-in, phone call, LINE chat with the shop | Free, familiar | **The real baseline.** We win only if it is clearly less effort |

## 3. Abdul in detail

**What it is.** A verified LINE Official Account with a web workspace and a LIFF mini-app, made by "nativelabs.studio". It started around 17-18 June 2026 as a broad "AI second brain" and by November had narrowed to "a reminder assistant in LINE", with group reminders as the paid driver `[V, recon s.2-3]`.

**Plans (yearly price ÷ 12 is what the page shows by default; monthly billing is higher, about ฿149 and ฿490 on the two top plans, estimated `[recon s.4.4]`):**

| Plan | Shown price a month | Main limits |
|---|---|---|
| Free | ฿0 | 15 reminders a month, 8 recurring, 1 GB, 2 drawings a week |
| วัยรุ่น | ฿83 | 100 reminders a month, 5 GB, Google Calendar |
| ร่างทอง | ฿125 | 3 groups, 200 reminders a month plus 200 per group, 100 GB |
| เถ้าแก่ | ฿409 | 10 groups, 500 per group a month |

**Technology (inferred from its privacy policy and headers):** Next.js on Cloudflare, Railway in Singapore, Backblaze B2 for files, OpenRouter with DeepSeek and Gemini models, Voyage AI embeddings, an intent classifier that decides if a group message is for Abdul `[V, privacy policy; recon s.2]`.

**Strengths we should respect**
- Mature Thai time parsing, including Buddhist-era dates, typos, and วันพระ.
- Recurring reminders, an escalation mode ("keeps messaging until you answer"), and group mode where only the payer needs a plan.
- Memory and file recall, and Google Calendar read and write.
- About 42,000 friends on 9 Nov 2026 `[V, recon]`. Friends are not active users.
- A free tier and PromptPay payment already live.

**Gaps (our opening) `[V/I, teardown s.6]`**
1. It takes no action in the outside world: it does not book, reserve, order or contact anyone.
2. It has no link to Thai merchant systems.
3. It has no closed loop: it cannot confirm an appointment happened or reschedule it with the business.
4. "Done" means whatever the user taps. It does not record evidence or an outcome.
5. The moat is weak: reminders and memory are easy to copy.

**What we do not know about Abdul:** its paid-user count, its quality in daily use, and its roadmap beyond "coming soon: daily recap, Google Drive, personality". There are no public complaints or reviews `[V, recon s.4.1, absence is not proof]`.

## 4. Feature comparison

"Plan" means our combined-proposal baseline, Must unless marked S (Should) or C (Could). "Extended" means the additions I proposed in the extended scope document.

| Capability | Abdul (today) | Our plan | LINE / LY Corp | ChatGPT / Google AI Mode | Booking marketplaces | Merchant chatbots |
|---|---|---|---|---|---|---|
| Lives in the user's LINE chat | **Yes** | **Yes** | Yes | No | No (apps) | Shop's OA only |
| One-off reminders, snooze, done | Yes | Yes | n/a | Limited | No | No |
| Recurring reminders | **Yes** | S3 (Should) | n/a | Limited | No | No |
| "Can't miss" escalation | **Yes** | Won't | n/a | No | No | No |
| Group mode | **Yes** (paid driver) | Won't | n/a | No | No | No |
| Thai dates and Buddhist era | **Yes** (mature) | Yes (to be tested, target ≥ 90%) | n/a | Works in Thai | n/a | Varies |
| Notes, files, search | **Yes** (1-100 GB) | Extended A1 | n/a | In app | No | No |
| Google Calendar | **Yes** (read and write) | Won't; Extended A2 | n/a | Via connectors | No | No |
| Daily briefing | Demo shown; "daily recap" listed as coming soon | S4 upcoming list; Extended A4 | n/a | Tasks | No | No |
| **Drafts a request to a business** | **No** | **Yes (M4)** | Merchant side only | Partly, via partners | n/a (shop is on the platform) | n/a |
| **Reaches a shop with no booking system** | **No** | **Yes** (LINE handoff, copy, call button; S2 operator) | No | US only, "Ask for me" `[S]` | No | No |
| **Follows up and records the answer** | **No** | **Yes (M4)** | No | Limited | n/a | n/a |
| **Honest state: sent vs answered vs booked** | **No** | **Yes (F10)** | No | No | Booking is binary | n/a |
| **Withdraw within 10 minutes** | **No** | **Yes (F9)** | No | No | Platform rules | n/a |
| **Business answers without signing up** | **No** | S0 reply page | n/a | No | Shop must be on platform | Shop must buy tool |
| Instant confirmed booking | No | **No** (a request, not a booking) | Japan only | Yes for partner venues | **Yes** | Sometimes |
| Pays for itself | Subscription | None in prototype | n/a | Subscription | Commission | ฿499+ a month |

Reading the table: we lose or tie on every row above the line "Drafts a request to a business". We win only on the rows below it, and we lose on instant confirmed booking.

## 5. What this means for our own scope

### 5.1 Parity versus differentiator

| Our item | Abdul has it? | Role |
|---|---|---|
| M2 obligations and one-off reminders | Yes | **Parity.** Needed as the base, but it is not why anyone switches |
| S3 recurrence, S4 upcoming list | Yes | Parity |
| Extended A1 personal vault | Yes (memory, 1-100 GB) | Parity. A weaker copy if built alone |
| Extended A2 calendar connectors | Yes (Google Calendar read and write) | Parity |
| Extended A4 morning brief | Partly (demo; "daily recap" coming) | Parity |
| Extended A3 Gmail | No | Differentiator on paper, but Google's restricted-scope review blocks it for production `[S]` |
| M4 request to a business, F8 follow-up, F9 Withdraw, F10 honest states | No | **Differentiator** |
| S0 reply page, S2 operator console | No | **Differentiator** (supply side) |
| Venue table with channel and confidence | No | **Moat candidate** |

### 5.2 Correction to my earlier proposal

In `docs/specs/extended-functional-scope.md` I proposed A4 (morning brief) and the basic vault (A1) as **Should**. This analysis shows they are Abdul parity. I now suggest:

- **Do not build A1, A2 or A4 as standalone features.** Abdul already offers them, and they cost 55-100 h that the plan does not have.
- **Build the small parts that make the booking loop better:**
  - **Calendar clash check** before proposing time options for a request (a free/busy read, or just the user's own confirmed bookings if there is no calendar link).
  - **A brief section called "Requests waiting for an answer"**, reusing S4's list. This is something Abdul cannot show because it has no requests.
  - **Booking confirmations kept as items**, such as a screenshot of the shop's reply attached to the task, which F5 already allows.
- **Keep Gmail forwarding (A3.1) as a Could.** It feeds the "obligation" side. Abdul has no email feature.

If the team still wants A1, A2 or A4, record the reason: expected user pull for the pilot, not differentiation.

## 6. Differentiation strategy

### 6.1 Positioning

> **Abdul reminds you to book a haircut. We get the haircut booked, and tell you honestly when it is confirmed.**

This is the combined proposal's "acts, not only reminds" `[combined proposal s.1]`, stated so a user and an examiner can compare it with Abdul in one sentence.

### 6.2 Five differentiators

| # | Differentiator | What Abdul and others do instead | Why it matters | How we prove it (evaluation) |
|---|---|---|---|---|
| D1 | **Request-to-business loop:** draft the message, one-tap send, follow-up, outcome | Abdul stops at the reminder. Marketplaces need the shop on their system | Reaches the long tail of Thai shops that take bookings by LINE chat or phone `[S, hdmall via combined proposal]` | E8 (unaided completion ≥ 70%), E11 (answered, declined, no answer, time to answer) |
| D2 | **Honest states and evidence:** "opened", "sent", "answered", "user-confirmed" are different words | Abdul: Done = user said so. Marketplaces: booked or not | Users trust the status. A false "booked" is the worst failure | E6: 0 tasks shown as sent, booked or done without the matching evidence |
| D3 | **Safe action gate:** the AI cannot send or book. Each action needs a content-bound tap, with Withdraw | Most AI assistants act on a prompt | Matches Thai input-error law and builds trust `[combined proposal s.7.2]` | E3: 0 actions without a valid tap; E4: 0 effects from 30 injection cases |
| D4 | **Venue channel table:** which shop takes bookings by LINE, phone, or page, how reliably, and when it was last checked | Nobody publishes this for Thai shops `[I]` | The data compounds with every attempt and drives the router. It is the closest thing to a moat | E12: share of attempts where the stored channel worked; score moves the right way |
| D5 | **Shop-side reply page (S0):** the shop answers with one tap and no sign-up | Merchant chatbots need the shop to buy a tool | Closes the loop without asking the user | E11 reply-page decision rule |

### 6.3 What we should not do

- **Do not fight Abdul on reminders, groups, memory or price.** It is ahead and cheaper to copy than to beat.
- **Do not promise instant confirmed booking.** Marketplaces and partner APIs win there, and we cannot match them with the access we have `[see the booking-options comparison]`.
- **Do not claim a market gap that the evidence does not support.** The team found no Thai survey showing that people want an assistant to book for them. This is "the biggest gap" in the market research `[S, market s.3]`.

### 6.4 Moat realism

An honest ranking of what is hard to copy:

| Asset | Copy difficulty | Notes |
|---|---|---|
| The feature list | Easy | Any competitor can add "draft a message to a shop" `[I]` |
| The honest state model and action gate | Medium | Needs discipline and tests, not secrets |
| Venue channel table with verified outcomes | Medium-high, grows with use | Needs real attempts. With 10-15 venues in the prototype it is only a proof of concept |
| Shops that answer through our page | High, but needs many shops | Cold start on both sides `[market s.8]` |
| Channel ownership | Not ours | LINE can change terms and pricing at any time |

For a senior project, a full moat is not required. The proposal needs a clear, evidence-backed difference and an honest account of how weak it is.

### 6.5 Risks to the differentiation, and what to watch

| Risk | Signal | What we do |
|---|---|---|
| Abdul adds booking or "message the shop" | Release notes, its "coming soon" list, its LINE OA posts | Re-run the comparison; lean harder on D2-D5 |
| LINE ships a consumer booking agent in Thailand | LINE Thailand announcements; Agent i expanding beyond Japan `[S]` | Treat as the main external risk in the report |
| Google AI Mode or ChatGPT reach Thai venues | Thai partners appear | Our reach to non-partner shops is the remaining edge |
| Shops do not answer, or testers do not send | E11 and E8 results | Report honestly; change the default to chat follow-up |
| Testers prefer Abdul's features | Interview feedback, repeat use | Report it. Consider whether to integrate rather than compete |

## 7. Using this in the proposal and evaluation

- The combined proposal already plans an **Abdul comparison** table in the final report (M6, E13 qualitative comparison). Use the feature table in section 4 as its first draft.
- Run the same five tasks on Abdul and on our bot with a few testers: set a one-off reminder, set a monthly reminder, ask the assistant to book a haircut, find out whether the shop replied, and withdraw a request. Record where each succeeds. Use Abdul as a normal customer only, with no probing, in line with the team's research boundary `[TEAM-BRIEF]`.

## 8. Open questions

1. Who tests Abdul as a normal customer to check section 3 and the five-task comparison? (Its terms forbid probing; normal use is allowed.)
2. Do we keep recurrence (S3) as the only parity feature, or accept being weaker than Abdul on all reminders?
3. Should "Lamai" and "Cole" be verified before the proposal cites them? I could not find them.
4. Does the team want to measure whether testers would use this instead of Abdul, or only compare features?
5. Is the extended scope document updated to match section 5.2, or left as a record of the options?

## 9. Sources

- Team files on branch `jui` of ommhungry/f-abdul: `docs/research/ai-assistant-jui/2026-11-08-abdul-teardown.md`, `docs/research/abdul-recon/abdul-recon-2026-11-09.md`, `docs/research/ai-assistant-jui/2026-11-08-agent-market-competitors.md`, `docs/combined/combined-proposal.md`, `docs/review/TEAM-BRIEF.md`. The sources they cite are listed in those files.
- Searches on 12 Nov 2026 for Abdul news and for "Lamai" and "Cole": no relevant results. Related LINE products seen: [LINE AI Assistant (Japan)](https://www.linecorp.com/pr/news/global/2024/138), [Agent i in chat](https://aismiley.co.jp/ai_news/line-yahoo-agent-in-chat/).
