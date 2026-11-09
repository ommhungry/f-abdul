# Abdul manual black-box test script

- Version: 2026-11-09. Companion to `abdul-recon-2026-11-09.md`.
- Purpose: let one team member observe how Abdul (LINE OA @abdul) behaves, **as an ordinary user**, so we can confirm or reject the inferences in the recon report.
- This script is **for a human, typing by hand**. Do not automate it, script it, or run it through any bot, emulator or proxy.

## Ground rules (read before you start)

1. **Act as a normal customer.** Abdul's terms forbid users to "reverse-engineer, disrupt, scrape, probe, or exploit Abdul". Normal use of the features they advertise is allowed. Do not try to break, overload, jailbreak or extract the system prompt. Do not send prompt-injection text.
2. **Pace.** No more than about 10 messages in any 10 minutes. Spread the script over 3 to 7 days. The recurring tests need days anyway.
3. **No real personal data in test content.** Registration asks for a mobile number. The tester may use their **own** number only with informed consent, knowing that Abdul stores it under its privacy policy. Never record the number in notes or the repo. Do not use a fake number or someone else's number. If no team member consents, stop and record "blocked at registration". For everything else, use made-up names ("คุณเอ", "ร้านทดสอบ"), made-up bills and fake amounts. Do not upload real ID cards, bills or medical documents. Do not connect a Google account that holds real personal events. Use a fresh test Google account for the calendar tests.
4. **Groups.** Only use a test group made of consenting team members. Tell them first that a bot will read the group.
5. **Paying.** Use only the free plan and the free 14-day trial. Do not enter card details. If a test needs a paid tier after the trial, skip it and record "not tested".
6. **Finish clean.** At the end, ask Abdul to delete everything (T39), disconnect Google Calendar, remove Abdul from the test group, and block or remove the OA.
7. **Screenshots.** Crop out your own name, profile picture and phone number before saving. Store screenshots in a team folder, not in the repo.

## What to record for every test

Copy this block for each test into your notes (spreadsheet or Markdown):

| Field | What to write |
|---|---|
| Test ID | e.g. T07 |
| Date and time sent | Local time, to the minute |
| Exact text sent | Copy-paste |
| Abdul's reply text | Copy-paste, or "no reply" |
| Reply delay | Seconds from send to reply (rough is fine) |
| UI elements | Buttons, quick replies, Flex cards, links to the LIFF app |
| Parsed result shown | The time, date and recurrence Abdul echoed back |
| Correct? | Yes / No / Partly, and why |
| Reminder fired at | Actual time the push arrived (for reminder tests) |
| Quota or plan message | Any mention of limits, plan or upgrade |
| Screenshot file | File name |
| Notes | Anything surprising |

---

## Part A. Onboarding (2 tests)

| ID | Do this | Watch for |
|---|---|---|
| T01 | Add @abdul as a friend from `lin.ee/6z0oLc2`. Do not type anything yet. | Greeting text, rich menu items, whether it asks you to register in a LIFF page, what the LIFF page asks for (phone number? survey?). Registration asks for a mobile number. The tester may use their **own** number only with informed consent, knowing that Abdul stores it under its privacy policy. Never record the number in notes or the repo. Do not use a fake or someone else's number. If no team member consents, stop and record "blocked at registration". |
| T02 | Type: `สวัสดี อับดุลทำอะไรได้บ้าง` | How it describes itself. Is it "reminders" first, or "second brain" first? Which features does it list? |

## Part B. Personal reminders, Thai (12 tests)

Pick times 5 to 15 minutes ahead so you can watch them fire. Replace the bracketed parts.

| ID | Type exactly | Expected (from public claims) | Record especially |
|---|---|---|---|
| T03 | `อีก 10 นาที เตือนเก็บผ้า` | One-off in 10 minutes | Echo format; did it fire on time? |
| T04 | `บ่ายสาม เตือนโทรหาช่างแอร์` (send before 15:00) | Today 15:00 | Whether it assumes today or asks |
| T05 | `2 ทุ่ม เตือนรดน้ำต้นไม้` | Today 20:00 | Correct reading of "ทุ่ม" |
| T06 | `ตี 2 เตือนดูบอล` | 02:00 next day | Does it pick today 02:00 (already past) or tomorrow? Record the echoed date only; you need not stay awake. Check the next morning whether the push arrived and at what time, then cancel. |
| T07 | `บ่าย 2 ประชุม` (no word "เตือน") | Should still create a reminder, or ask | Does it need the word "เตือน"? |
| T08 | `ศุกร์หน้า 10 โมง โทรตามลูกค้า ABC` | Next Friday 10:00 | Which Friday it picks, especially if you test on a Thursday or Friday |
| T09 | `พรุ่งนี้เช้าเตือนจ่ายค่าน้ำ` (no exact time) | Asks for a time, or picks a default | The default morning time it chooses |
| T10 | `ทุกวัน 2 ทุ่ม เตือนกินวิตามิน` | Daily 20:00, labelled recurring | Counts as 1 of 8 recurring slots on free? |
| T11 | `ทุกวันที่ 25 เตือนจ่ายค่าบัตร` | Monthly on the 25th | How it shows the next date |
| T12 | `ทุกวันพระ เตือนใส่บาตร` | Lunar holy days | Does it state the next วันพระ date? Check it against a Thai calendar |
| T13 | `ทุกวันจันทร์ถึงศุกร์ 7 โมง เตือนออกกำลังกาย` | Weekdays only | Supported, or reduced to daily? |
| T14 | `พรุ่งนิ้ 9 โมง เตือนส่งงาน` (typo on purpose) | Understands the typo | Correct date? |

## Part C. Personal reminders, English and mixed (4 tests)

| ID | Type exactly | Record especially |
|---|---|---|
| T15 | `remind me to call the bank in 15 minutes` | Does it reply in English or Thai? |
| T16 | `remind me every Monday at 9am to submit the timesheet` | Weekly recurrence in English |
| T17 | `tomorrow 3pm dentist` | Accepts a bare note-style entry? |
| T18 | `พรุ่งนี้ 10am meeting กับทีม` (mixed) | Handles code-switching |

## Part D. Reminder interaction (6 tests)

| ID | Do this | Record especially |
|---|---|---|
| T19 | When T03 fires, press **Done** (เรียบร้อย). | Confirmation text; does the item disappear from the list? |
| T20 | When T04 or T05 fires, press **Snooze** (อีก 1 ชม.). | Other snooze options? Does it fire again exactly 1 hour later? |
| T21 | When a reminder fires, **ignore it** for 30 minutes. | Does it nag again without escalation mode? |
| T22 | `อีก 5 นาที เตือนปิดเตา ห้ามพลาดเด็ดขาด` then ignore it. | The "can't miss" mode: how often it repeats, for how long, and whether it stops by itself. Stop after 30 minutes by replying `โอเค ทำแล้ว`. |
| T23 | `มีเตือนอะไรบ้าง` | Lists all reminders? Format? Link to the LIFF app? |
| T24 | `เลื่อนเรื่องจ่ายค่าบัตรเป็นวันที่ 27` then `ยกเลิกเตือนกินวิตามิน` | Can it edit and cancel by plain language? Does it confirm which item? |

## Part E. Memory and questions (5 tests)

| ID | Do this | Record especially |
|---|---|---|
| T25 | `จดไว้: รหัสล็อกเกอร์ฟิตเนส 1234` (fake number) | Saved as a note? Any warning about sensitive data? |
| T26 | Next day: `รหัสล็อกเกอร์คืออะไร` | Recall works across days? |
| T27 | Send a photo of a **made-up** bill you write by hand ("ค่าไฟ 500 บาท ครบกำหนด 30 พ.ย."). | Does it read the due date? Does it **offer** to set a reminder, or only store the photo? (This tells us how close it is to "acting".) |
| T28 | `ช่วยหาร้านกาแฟเปิดดึกแถวสามย่าน` | Web search answer, sources shown? Does it offer to book, call, or only list? |
| T29 | `ช่วยจองคิวตัดผมพรุ่งนี้บ่าย 2 ที่ร้านแถวบ้าน` | **Key gap test.** Does it refuse, set a reminder instead, or claim to book? Record the exact wording. |

## Part F. Limits and plan (3 tests)

| ID | Do this | Record especially |
|---|---|---|
| T30 | Ask: `แพ็กเกจฟรีเหลือเตือนได้อีกกี่ครั้ง` | Does it know your quota? |
| T31 | Ask: `ราคาแพ็กเกจรายเดือนเท่าไหร่` and, separately, open abdul.chat in a browser and click the **รายเดือน** toggle. | The real monthly-billing prices (the recon report could not verify them). |
| T32 | Only if the trial has ended and you are on free: keep creating normal reminders over the month until a limit message appears. Do not spam to reach it faster. | The exact limit message, the upgrade path, and whether existing reminders still fire. |

## Part G. Groups (needs the ร่างทอง trial; 5 tests)

Use a test group with 2 or 3 consenting teammates. Teammate B should **not** add Abdul as a friend.

| ID | Do this | Record especially |
|---|---|---|
| T33 | Invite Abdul into the group. | Join message; does it explain privacy in the group? |
| T34 | Teammates chat normally for a few messages (e.g. `กินข้าวยัง`). | Does Abdul stay silent? Any wrong reply? |
| T35 | `@อับดุล อีก 10 นาที เตือน @B ส่งรายงาน` | Mentions B when it fires? Works although B never added the OA? |
| T36 | When T35 fires, have teammate C (not B) press Done. | Who is allowed to close it? |
| T37 | `@อับดุล สรุปที่คุยกันวันนี้ให้หน่อย` | Summary quality; does it name members? |
| T41g | `@อับดุล ทุกเช้า 8 โมงสรุปงานวันนี้ให้ทีมด้วย` (group variant of T41) | Accepted, or "coming soon"? Next morning: did a summary arrive at 08:00, and what did it contain? Cancel it afterwards. |

## Part H. Calendar (optional, test Google account only; 2 tests)

| ID | Do this | Record especially |
|---|---|---|
| T38a | Connect a **fresh test** Google account through the LIFF app. | The consent screen scopes (expect `calendar.events`). Screenshot the scope list. |
| T38b | `ลงปฏิทินนัดหมอฟันวันพฤหัสหน้า 4 โมงเย็น` | Event created in the test calendar? Did it ask to confirm first? Does it also set a reminder? |

## Part H2. Open items from the recon report (3 tests)

Run these before Part I. They settle items listed as unknown in the recon report (section 4.5 items 2, 9 and 10; section 8 "daily summary").

| ID | Do this | Record especially |
|---|---|---|
| T41 | `ทุกเช้า 8 โมงสรุปงานวันนี้ให้หน่อย` (personal chat) | Accepted, refused, or "coming soon"? Next morning: did a briefing arrive at 08:00, and what did it list? Cancel it afterwards. |
| T42 | `วันที่ 5 ม.ค. 2570 เตือนต่อภาษีรถ` | Reads the Buddhist-era year 2570 as 2027 and "ม.ค." as January? Record the echoed date exactly. Cancel it afterwards. |
| T43 | Send a 5-second **voice message** saying "พรุ่งนี้ 9 โมงเตือนโทรหาหมอ" (fake content). | Is it transcribed and turned into a reminder, ignored, or answered with "not supported"? (The privacy policy says audio may not be processed.) |

## Part I. Clean-up (2 steps, mandatory)

| ID | Do this | Record especially |
|---|---|---|
| T39 | `ลบข้อมูลทั้งหมดของฉัน` | Asks for confirmation? States that files and search data are deleted? Afterwards, ask `รหัสล็อกเกอร์คืออะไร`. It should not remember. |
| T40 | Disconnect Google Calendar, remove Abdul from the test group, remove or block the OA. Revoke the app at myaccount.google.com/permissions for the test account. | Any final message from Abdul. |

---

## After the run

Write a short summary (one page) with:
1. Which recon inferences were **confirmed**, **rejected**, or still **unknown** (use the failure list in section 4.5 of the recon report).
2. Parse accuracy: correct / total for Parts B and C.
3. Median reply delay and the largest reminder lateness you saw.
4. The exact wording from T27, T28 and T29 (how Abdul handles "doing" requests). This is the most important evidence for our gap analysis.
5. Real monthly prices from T31.

Save the summary in `docs/research/abdul-recon/` as `manual-run-<date>.md`, with no personal data and no screenshots that show real names or phone numbers.
