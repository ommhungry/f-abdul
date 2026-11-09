# Abdul (อับดุล) teardown: requirements and functions, from public sources

- Date: 2026-11-08
- Method: black-box review of public material only. Sources were the homepage (TH and EN), the privacy policy (updated 2026-10-25), the terms of service (updated 2026-06-19), the LINE OA profile page, and the public response headers of the homepage.
- What was not done: no sign-up, no chatting with the bot, no traffic capture of app.abdul.chat, no decompiling. Abdul's terms forbid users to "reverse-engineer, disrupt, scrape, probe, or exploit Abdul" [V, abdul.chat/en/terms-of-service]. A team member who wants first-hand detail can use Abdul as a normal customer and take screenshots.
- Tags: [V] read directly from Abdul's own pages on 2026-11-08; [I] our inference.

## 1. What Abdul is

- An AI personal assistant that lives in a LINE Official Account (@abdul, LINE ID page 319gqomd).
- The OA is verified (blue badge) and had **41,573 friends** on 2026-11-08 [V, page.line.me/319gqomd].
- Web workspace at app.abdul.chat, plus a LIFF mini-app inside LINE [V, privacy policy].
- Made by nativelabs.studio. Contact: hello@abdul-ai.com [V].
- Terms dated 2026-06-18. Beta and possibly invite-only [V, terms].
- Positioning: "A reminder you can count on, right in LINE", for work, home, and shop or family LINE groups [V, homepage].

## 2. Functions (what it does today)

| Area | Function | Evidence |
|---|---|---|
| Reminders | Reminders set in natural Thai: one-off, daily, monthly, or "every Buddhist holy day"; typos and shorthand understood | [V] FAQ |
| Reminders | "Can't be missed" mode keeps messaging until the user answers; Done and snooze buttons in chat | [V] FAQ |
| Groups | Added to a LINE group, it reminds a tagged member (@Keng) on time; posts a daily team summary; sums up group discussion | [V] homepage |
| Memory | Saves notes, messages, images, files and links; "permanent memory"; search and recall | [V] terms, privacy |
| Tasks | Task management | [V] terms |
| Q&A | AI answers, with web search (self-hosted search first, Firecrawl as fallback) | [V] privacy |
| Calendar | Google Calendar: reads events for a daily briefing; **creates and edits events** on request | [V] privacy (scope calendar.events) |
| Images | Generates drawings (2-30 a week depending on plan) | [V] pricing |
| Files | Storage of 1 GB to 100 GB, plus storage per group; 24-hour trash | [V] pricing, privacy |
| Roadmap | Daily recap, Google Drive, a personality option, renaming Abdul, choosing Abdul's gender | [V] "coming soon" |

**The only actions it takes in the outside world are messages inside LINE and edits to the user's own Google Calendar.** It does not book, order, buy or contact any business [I, from the full feature list].

## 3. Pricing (homepage, 2026-11-08)

| Plan | Price | Main limits |
|---|---|---|
| เบบี๋ (Free) | 0 | 8 recurring reminders, 15 reminders a month, 1 GB storage, 2 drawings a week |
| วัยรุ่น | ฿83 a month (฿990 a year) | 20 recurring, 100 a month, 5 GB, Google Calendar |
| ร่างทอง (Pro) | ฿125 a month (฿1,490 a year) | 3 groups, 40 recurring, 200 personal and 200 per group a month, 100 GB |
| เถ้าแก่ (Business) | ฿409 a month (฿4,900 a year) | 10 groups, 500 a month per group, 100 GB plus 50 GB per group |

- 14-day Pro trial, no card. Payment by PromptPay, or by card through Stripe for auto-renewal [V].
- The terms (2026-06-19) still say the service is free during beta. Paid plans came later [V].

## 4. Architecture inferred from the privacy policy and headers

| Layer | What they use | Evidence |
|---|---|---|
| Channel | LINE Messaging API, LINE Login, LIFF | [V] |
| Web | Next.js behind Cloudflare | [V] headers: `x-powered-by: Next.js`, `server: cloudflare` |
| Backend and database | Railway (Singapore): database, queues, core app | [V] |
| Files | Backblaze B2 (US), served through the Cloudflare CDN | [V] |
| LLM | OpenRouter gateway with zero-retention, no-training routing. Two model families (see below) | [V] |
| Intent classifier | TypeSafe AI: classifies each message (to Abdul or not, question or not); group members are pseudonymised | [V] |
| Embeddings | Voyage AI | [V] |
| Search | Self-hosted search engine; Firecrawl as fallback | [V] |
| Payments | Stripe and PromptPay | [V] |
| Analytics | PostHog (pseudonymous ID), Google Analytics, Microsoft Clarity (screen recordings with text masked) | [V] |
| Data location | Singapore and the US; Abdul is the PDPA data controller | [V] |

LLM detail:
- DeepSeek models, served by Together AI and Fireworks AI, for chat, notes and recall.
- Gemini models for vision, image generation and drafting.

Inferred pipeline [I]:
1. A LINE webhook receives each message.
2. The intent classifier decides whether the message is addressed to Abdul.
3. An LLM extracts the reminder or note into structured data.
4. A job queue schedules a push message at the due time.
5. Embeddings make saved notes searchable.
6. Push messages are counted against LINE's paid message quota, which is why every plan has hard monthly reminder limits [I].

## 5. Requirements implied (baseline for our product)

Functional baseline, all present in Abdul:
1. Natural-language Thai time parsing, including recurrence and the Buddhist calendar.
2. Scheduled push messages with Done and snooze, plus an escalation mode.
3. Group mode: mentions, pseudonymised members, quota per group.
4. Personal memory: notes, files and links, with semantic search and delete.
5. Google Calendar read and write.
6. A LIFF mini-app for settings and payment; a web workspace.
7. Free tier, metered quotas, PromptPay subscription.

Non-functional requirements they had to meet:
- PDPA controller duties.
- Zero-retention LLM routing.
- Cross-border transfer disclosure.
- Pseudonymised analytics.
- Deletion that also removes the search index.
- Minimum age 13; under-18s need guardian permission.

## 6. Gaps, which are our opening [I]

1. **No action in the outside world.** It reminds; it does not book, reserve, order or contact.
2. No link to Thai merchant systems (salon, clinic or restaurant bookings).
3. No closed loop: it cannot confirm that the appointment happened or reschedule it with the business.
4. Weak moat: reminders and memory are easy to copy (LINE itself, ChatGPT and Gemini all remind). 41.5k friends is early traction, not lock-in.
5. It already has Calendar write access, so moving into "doing" is a natural next step for Abdul too. **Assume Abdul could add booking itself.**
