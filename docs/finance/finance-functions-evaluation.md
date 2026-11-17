# Finance assistant: evaluation of proposed functions

| Field | Value |
|---|---|
| Version | 0.1 draft, 17 November 2026 |
| Status | For team review. Not approved |
| Direction | Pivot from a general assistant to a **finance specialist in LINE**. Users send transfer slips; the bot reads them, tracks money, reminds about bills, and gives guidance |
| Input | The team's list of 9 functions (section 2), plus research from 17 Nov 2026 on slips, bank apps, regulation and data sharing (section 7) |
| Team | 3 students, 3-4 hours a week each. Proposal due 22 November 2026; project ends 30 April 2027 |

**How to read this.** Evidence tags follow the team convention: `[V]` primary source opened, `[S]` secondary source or search result, `[I]` our inference, `UNVERIFIED` not checked. The web facts here come from search results, so they are `[S]` at best. **Hour figures are estimates.** Priorities are proposals for the team to accept or change.

## 1. The purpose test

The advisor's earlier comment was that we were adding features without a purpose. So every function below is tested against one sentence:

> **Know where your money stands and what you owe next, without typing it in, inside LINE.**

A function passes if it helps the user answer one of these questions with little effort:

1. What do I own and owe? (net position)
2. Where did my money go? (past)
3. What do I have to pay, and when? (next)
4. How much will I have left? (forecast)

A function that needs a lot of manual entry, or that answers none of these, is weaker no matter how interesting it is.

### What we must be honest about
- **Thai bank apps already summarise spending automatically.** K PLUS "My Spending" needs no manual logging, groups spending into categories, and has a budget feature `[S, kasikornbank.com]`. A spending chart alone will lose to the user's own bank app.
- **Our edge is what a bank app cannot see:** slips from **every** bank in one place, what each transfer was **for**, money **shared with other people**, and bills tied to **proof of payment** `[I]`.
- **Slips cover only part of the picture.** They show transfers and QR payments. Card payments, cash and automatic debits do not produce a slip for the user. Salary arriving in an account does not produce a slip either `[I]`. Every report must say how complete its data is.
- **A direct competitor exists.** KBTG Labs (Kasikorn's tech arm) built "เหมียวจด", which reads e-slips automatically `[S, marketingoops.com]`. Its current status is unknown.

## 2. Summary of the evaluation

| # | Function (team's wording) | Purpose test | Feasible in LINE? | Value to user | Main risk | Effort (est.) | Proposed priority |
|---|---|---|---|---|---|---|---|
| 3 | List expenses (ค่าอาหาร, ค่าเดินทาง, ค่าผ่อน...) | Q2 | **Yes**, from slips plus photos of statements | High | Partial data | 25-35 h | **Must** |
| 4 | Show what is due in X days or by month end | Q3 | **Yes** | **Very high** | None major | 10-15 h | **Must** |
| 2 | List incomes (เงินเดือน, ปันผล, ค่าเช่า...) | Q1, Q4 | Yes, mostly manual or document photos | Medium-high | Income rarely comes with a slip | 8-12 h | **Must** (simple) |
| 7 | Dashboard for assets, incomes and expenses | Q1, Q2 | Yes: summary card in chat, full view in a LIFF page | High | Build effort | 6-8 h (chat card) / 15-25 h (LIFF) | **Must** (chat card), **Should** (LIFF) |
| 8 | Break down an installment: principal (เงินต้น) vs interest (ดอกเบี้ย) | Q1, Q3 | Yes, by calculation from loan terms | High | Loan types use different methods | 10-15 h | **Should** |
| 5 | Estimate next month's income | Q4 | Yes for fixed income | Medium alone; **high as "money left after bills"** | Variable income is hard to predict | 6-10 h | **Should**, reshaped |
| 1 | List every asset: stocks, forex, crypto, real estate, vehicles | Q1 | Yes for manual entry; partly for live prices | Medium | Manual entry burden; market-data licensing | 12-30 h | **Should** (manual), **Could** (live prices) |
| 9 | AI that analyses assets | Q1 | Yes, if descriptive only | Medium | **Investment advice needs an SEC licence** | 8-12 h | **Should** (descriptive), **Won't** (buy/sell advice) |
| 6 | For assets still being paid off: what to do, which documents, can AI file it? | Q3 | Checklist yes; **filing no** | Medium, occasional | Legal accuracy; in-person procedures | 6-10 h (checklist) | **Could** (checklist), **Won't** (filing) |

Effort for everything above: about **106-172 h** (estimate). The Must set alone is about **55-77 h**. Section 5 compares this with the team's capacity.

## 3. Function-by-function evaluation

### F3. List expenses
**What the user does.** Forwards a slip, sends a photo of a card statement or receipt, or types "ข้าว 60".

**How the data comes in**
| Source | How | Quality |
|---|---|---|
| Transfer and PromptPay slips | Read the slip's QR payload and verify it with a slip-verification service (EasySlip, SlipOK); fall back to OCR if the QR cannot be read | High. The services check against bank systems and return amount, sender, receiver and time `[S, easyslip docs; slipok guide]`. EasySlip v2 also checks duplicates `[S]` |
| Credit card statements | Photo or PDF of the monthly statement, read by a vision model, one line per transaction | Medium. Needs a confirmation step |
| Cash | Typed in chat ("ข้าว 60") | Depends on the user |

**Categories.** The bot asks once what a recipient is ("Is นาย ก your landlord?") and remembers it. Future transfers to the same recipient are labelled automatically. The user can correct a label at any time. This is the part bank apps cannot do `[I]`.

**Installments (ค่าผ่อน)** are recognised as payments to a known lender and linked to the loan in F8.

**Feasibility.** High. LINE lets a bot receive images that users send `[S, LINE Messaging API overview]`. Slip-verification services have free tiers, reported as about 100 slips a month `[S, UNVERIFIED]`. A pilot with 15-20 testers may exceed that, so check pricing.

**Risks.** Partial data (no slip for cards, cash or debits). Slips show names and partial account numbers, so storage and deletion rules matter. A misread amount must never be saved without a confirmation tap.

**Verdict: Must.** It is the core of "without typing it in".

### F4. What is due in X days or by month end
**What the user sees.** "Due in the next 7 days: credit card ฿8,420 (Fri), car installment ฿6,900 (25th), electricity (estimated ฿1,200, 28th)." Each item has Paid, Snooze and Details buttons.

**How it works**
- Bills and installments are added from a photo, a typed message, or automatically when the bot sees the same payment repeat monthly.
- Reminders are sent before each due date.
- **A bill is marked paid when a matching slip arrives** (same recipient, close amount, near the due date). The user confirms the match. Until then, the reminder continues.

**Feasibility.** High. This reuses the reminder engine and the "Done (slip attached)" evidence level from the earlier combined proposal `[V, combined proposal s.4.2]`.

**LINE cost.** Scheduled reminders are push messages. The free plan has 300 a month `[V, judge-vj check 10]`. A daily "due soon" push for 20 testers would exceed it. An on-demand view (the user taps a menu button) is a reply message and costs nothing.

**Verdict: Must.** This answers "what do I owe next", which bank apps do not do across all bills.

### F2. List incomes
**Problem.** Income rarely creates a slip for the person receiving it. Salary is a bank credit. Dividends arrive by bank credit with a broker notice. Rent arrives as a tenant's transfer, and the tenant may send the slip `[I]`.

**How the data comes in**
- **Recurring income set once:** "salary ฿35,000 on the 25th". This covers most people.
- **Photos of documents:** payslip (สลิปเงินเดือน), dividend notice, withholding-tax certificate (50 ทวิ).
- **Rent:** when a tenant sends a slip, the user forwards it, and it is matched to "rent from tenant A".

**Verdict: Must**, in the simple form (recurring entries plus forwarded slips). It is needed for F5 and F7.

### F7. Dashboard
**In chat.** A summary card (a LINE Flex message): this month's spending by category, income received, bills still due, and **how much of spending came from slips versus typed entries**.

**Full dashboard.** A LIFF page (a web page that opens inside LINE) with charts for assets, incomes and expenses over time.

**Feasibility.** High. The chat card is cheap. The LIFF page is a small web app and needs login checks (`IF-LIFF-01` in the SRS).

**Verdict: Must** for the chat card, **Should** for the LIFF dashboard.

### F8. Principal versus interest for each installment
**What the user sees.** "This month's ฿6,900 car payment: about ฿5,400 principal, ฿1,500 interest. Remaining principal about ฿182,000." And a what-if: "Paying ฿2,000 extra a month finishes 8 months earlier."

**How it works.** Code calculates the split from the loan terms: amount borrowed, interest rate, term and start date. The user enters these once from the contract, or sends a photo of the contract or a statement.

**Important limits**
- **The method depends on the loan type** `[I]`.
  - Home loans usually use a reducing balance, and many Thai home loans have floating rates that change.
  - Car hire-purchase (เช่าซื้อ) contracts have used a flat-rate method. Rules on how rates are stated and calculated changed in recent years `UNVERIFIED`.
  - Credit card interest is charged daily on the outstanding balance.
- So the result is an **estimate**, unless it comes from the lender's own statement. The bot must say which.

**Verdict: Should.** High value, because people rarely know their split. Start with fixed-rate reducing-balance loans, then add other types.

### F5. Estimate next month's income
**Reshape it.** Income alone is not very useful. Most people already know their salary. The useful question is **"how much will I have left after bills?"**

> Expected income (recurring entries and scheduled dividends) minus bills and installments due (F4) minus typical spending (F3 average) = **estimated money left**.

**Feasibility.** High for fixed income. Low for variable income (freelance, sales commission). Show it as a range and label it as an estimate.

**Verdict: Should**, as a cash-flow forecast rather than an income forecast.

### F1. List every asset
| Asset | How the value is known | Feasibility |
|---|---|---|
| Bank deposits | User enters the balance; or later, the Bank of Thailand's "Your Data" sharing | Manual now. Data sharing is planned to start around the end of 2026 for deposits, then expand in 2027-28 `[S, bot.or.th]` |
| Thai stocks and funds | User enters holdings; prices from a market-data source | Manual holdings easy. Live SET prices may need a data licence `UNVERIFIED` |
| Foreign exchange | Holdings entered; rates from the Bank of Thailand | Easy `[I]` |
| Crypto | Read-only API key from the exchange, or manual entry | Feasible; storing API keys is a security risk |
| Real estate, vehicles | User enters a value; we do not estimate market value | Manual only |

**Risk.** Manual entry goes against "without typing it in". Asset values change, so stale data misleads.

**Verdict: Should** for manual entry with the last-updated date shown. **Could** for live prices.

### F9. AI that analyses assets
**Legal boundary.** In Thailand, advice on the value or suitability of securities, or on buying and selling them, is an investment-advisory service under the SEC's licensing regime `[S, Tilleke & Gibbins]`. The SEC's AI guidance also suggests disclosing chatbot use `[S]`. **We cannot recommend specific stocks, funds or trades.** A lawyer should confirm where general budgeting guidance ends `UNVERIFIED`.

**What we can do (descriptive):**
- Net worth and its trend.
- How assets are spread: cash, stocks, property, crypto, as percentages.
- Concentration: "70% of your investments are in one stock".
- Debt ratios: debt payments as a share of income.
- An emergency-fund measure: months of spending covered by cash.

Code computes every number. The AI explains what the number means in plain Thai. It never tells the user what to buy or sell.

**Verdict: Should** for the descriptive health check. **Won't** for investment recommendations, unless the project later works with a licensed partner.

### F6. Assets still being paid off: what to do, and can AI file it?
**A correction to the idea.** "ผ่อน = ยังไม่ใช่เจ้าของ" is true for some loans, not all `[I, to be confirmed with a legal source]`.
- **Car hire-purchase (เช่าซื้อ):** the finance company owns the car until the last payment. Afterwards the owner collects the registration book (เล่มทะเบียน) and transfers ownership at the Department of Land Transport.
- **Home loan with a mortgage (จำนอง):** the buyer already owns the house. The bank holds a mortgage. After the last payment, the mortgage is redeemed (ไถ่ถอนจำนอง) at the Land Office.

**What the bot can do**
- A **checklist** for each case: documents, where to go, typical fees, and a reminder a month before the final installment.
- The checklist is written by the team from official sources, with the source and date shown. The AI does not invent steps.

**Can AI file it for the user?** No.
- These steps are in person, need the owner's identity documents and signatures, or a power of attorney.
- Acting for the user in a government process is a legal and liability risk the project cannot carry.

**Verdict: Could** for two checklists (car hire-purchase and home mortgage). **Won't** for filing.

## 4. Gaps in the list

The list is missing two things that make the product different from a bank app:

| Missing function | Why it matters | Effort (est.) | Proposed priority |
|---|---|---|---|
| **Splits and IOUs:** "dinner ฿1,200 for 4", track who paid, mark paid when their slip is forwarded | Slips are already how friends prove payment in LINE. Bank apps do not do this `[I]` | 10-15 h | **Should** |
| **Ask about your money:** "how much did I spend on Grab last month?" | Natural use of a chat interface. Code runs the query; AI only words the answer | 8-12 h | **Should** |

## 5. Proposed scope and hours

| Priority | Functions | Effort (est.) |
|---|---|---|
| **Must** | F3 expenses (slips, statements, typed), F4 due soon with slip matching, F2 incomes (simple), F7 summary card | 55-77 h |
| **Should** | F8 principal and interest, F5 money left after bills, F1 manual assets, F9 descriptive health check, F7 LIFF dashboard, splits and IOUs, ask about your money | 73-117 h |
| **Could** | F1 live prices, F6 checklists | 12-25 h |
| **Won't** | Investment recommendations, AI filing government procedures, moving money, credit scoring | - |

**Capacity.** The earlier combined proposal estimated about 121-198 team-hours of delivery time to 25 April 2027, depending on holidays, exams and weekly hours `[V, combined proposal s.6.3]`. Testing, evaluation and documentation also take a large share (about 60 h in that plan). So the realistic build budget is roughly **60-130 h**.

**Reading:** the Must set fits. Some Should items fit. Not all of them do. The team should rank the Should list before the proposal.

### Foundations every function needs (included in the estimates above only partly)
- LINE webhook, signature check, job worker, database, deployment.
- The confirmation card: nothing is saved or changed without the user's tap.
- Per-user data isolation, encryption, export and delete.
- An evaluation set of real Thai slips and statements (with personal data removed) to measure reading accuracy.

The earlier combined proposal costed these at about 40-55 h (M1 and parts of M5) `[V, combined proposal s.6.2]`. Much of that design carries over.

## 6. Risks across all functions

| Risk | Why | What we do |
|---|---|---|
| **Privacy and security** | A net-worth view is a high-value target. Financial data is not a "sensitive" category under PDPA s.26, but harm from a leak is high `[I]` | Minimum data; encryption; no slip images kept longer than needed; delete on request; no API keys stored unless F1 live prices is built |
| **Wrong numbers** | A misread amount or a wrong interest estimate leads to bad decisions | Verify slips by QR; confirmation tap; label estimates; show the source of every number |
| **Partial data** | Slips miss cards, cash and debits | Show coverage on every report; offer statement photos |
| **Advice crossing into licensed activity** | SEC licensing for investment advice | Descriptive only; legal check before the pilot |
| **Data sent abroad to an AI model** | PDPA s.28 cross-border transfer | Send only what extraction needs; use providers with no-training terms |
| **Competition** | Bank apps, เหมียวจด | Focus on cross-bank, meaning, shared money, bills with proof |
| **LINE costs** | 300 free push messages a month | On-demand views; limited scheduled pushes |

## 7. Research notes (17 November 2026, search results)

- Thai slips carry a QR payload that verification services check against bank systems. They return amount, sender, receiver and time: [EasySlip docs](https://document.easyslip.com/en/guide/getting-started), [EasySlip v2 payload](https://document.easyslip.com/en/v2/verify/bank/payload), [thai-qr-payment slip verify](https://thai-qr-payment.js.org/guide/slip-verify/), [thanakan library](https://github-redirect.dependabot.com/codustry/thanakan).
- K PLUS spending summary and "My Spending": [kasikornbank.com expense summary](https://www.kasikornbank.com/en/personal/digital-banking/kplus/help/Pages/expense-summary.html), [KBank income and expense article](https://www.kasikornbank.com/th/personal/article/creditcard/pages/income-expense-tips.aspx).
- เหมียวจด by KBTG Labs: [marketingoops.com](https://www.marketingoops.com/?p=321517).
- DIY LINE expense bots: [automatron](https://github.com/dtinth/automatron), [n8n LINE receipt template](https://n8n.io/workflows/12680-track-receipt-expenses-via-line-with-openai-gpt-4o-mini-and-google-sheets/).
- Investment advisory licensing and SEC AI guidance: [Tilleke on investment advisors](https://www.tilleke.com/insights/regulation-investment-advisors-thailand/2/), [Tilleke on AI governance](https://www.tilleke.com/insights/thailands-ai-and-machine-learning-governance-framework-for-capital-markets), [Chambers Fintech 2026](https://gpg-pdf.chambers.com/fintech-2026/865/).
- Bank of Thailand "Your Data": [BOT news 10 Nov 2025](https://www.bot.or.th/en/news-and-media/news/news-20251110.html), [BOT Your Data project](https://www.bot.or.th/en/financial-innovation/digital-finance/open-data/Your_Data_Project.html).

## 8. Decisions for the team

1. Accept the purpose sentence in section 1, or write a better one.
2. Who is the first user: students, or working adults with salaries, bills and loans? This changes F1, F6, F8 and F9 a lot.
3. Rank the Should list.
4. Confirm the Won't list, especially no investment recommendations.
5. Who checks the legal points: the SEC boundary for F9, and the F6 ownership and redemption steps?
6. Do we test slip-verification services in the first spike (free tier limits, accuracy on real slips)?
