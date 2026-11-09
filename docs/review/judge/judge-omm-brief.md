# Judge verdict: omm's business and architecture brief

- Card: omm-brief
- Judge run date: 9 Nov 2026. All web sources below were accessed on 9 Nov 2026.
- Work judged:
  - Validation report: `docs/review/validation-omm-brief.md`
  - Revised brief: `docs/review/revised/omm/business-and-architecture-brief.md` (version 0.2)
  - Against the original: `docs/research/business-and-architecture-brief.md` (version 0.1)
- Tags: `[V]` primary source the judge opened; `[S]` secondary source; `[I]` judge inference.

## Verdict: PASS WITH MINOR FIXES

The validation is careful, and its central claims hold up when checked again. The revision keeps omm's structure, section order, and voice. Every addition is dated and marked. No omm content was deleted, apart from one small unlisted wording change (D3). The new prototype scope (§10.1) is about the right size for three students, and the architecture feedback is technically correct.

Two fixes matter before the manager combines this brief with the others:

1. One sentence tagged `[V]` is wrong. Stripe Thailand does not require a business entity (D1).
2. Several startup-sized passages that the validator itself flagged are still unannotated. They now contradict §10.1 (D2).

Both fixes are quick. Neither needs new research.

## 1. Spot-checks against sources (11 claims)

| # | Claim in the work | Judge result | Evidence |
|---|---|---|---|
| 1 | From 1 Feb 2027 LINE Thailand gives 25,000 free Reply API messages a month, with no carry-over. The Unlimited plan costs ฿5,000 a month and is bought through a designated Agency Partner on the Agency Global Platform. VAT is not stated. The notice is dated 30 Oct 2026. | CONFIRMED | `[V, lineforbusiness.com/th/news/20261030]`. The notice is dated 30 ตุลาคม 2569 and takes effect 1 กุมภาพันธ์ 2570. VAT is not mentioned. |
| 2 | Thai OA plans: Free 300; Basic ฿1,280 / 15,000 / ฿0.10; Pro ฿1,780 / 35,000 / ฿0.06; prices exclude 7% VAT | CONFIRMED | `[V, lineforbusiness.com/th/service/line-oa-features]` |
| 3 | The Free plan cannot buy extra messages; pushes above the cap are not sent | CONFIRMED, with a weaker basis than the tag suggests | The Thai page lists the Free plan's extra-message price as "-". It does not say "cannot top up" in words. The Messaging API pricing page says that above the limit "an error response will be returned and the messages won't be sent". Its free plans show "N/A" for extra messages, but its example uses Japanese plans `[V]`. The conclusion holds. |
| 4 | Push messages count per recipient, and reply messages do not count today. Messages to users who blocked the account are not counted. | CONFIRMED | `[V, developers.line.biz/.../pricing]` |
| 5 | Webhook redelivery is off by default. Its retry count and interval are undisclosed, and delivery is not guaranteed. A redelivered event keeps its `webhookEventId` and sets `isRedelivery`. Events can arrive out of order, so check `timestamp`. LINE recommends asynchronous processing. | CONFIRMED | `[V, receiving-messages]`. The page recommends async processing. It does not literally say "return 2xx quickly", but a non-2xx response triggers redelivery, so the advice is sound. |
| 6 | `X-Line-Retry-Key` works on push, multicast, narrowcast, and broadcast. It is valid for 24 h and returns `409` on a duplicate. Retry only on 500 or a timeout. A 200 does not mean the message was delivered. | CONFIRMED | `[V, retrying-api-request]` |
| 7 | Stripe PromptPay is customer-initiated, and Invoices and Subscriptions support only `send_invoice`. Only Thai Stripe accounts can accept it, "which needs a Thai business entity". | First part CONFIRMED. **"Needs a Thai business entity" is WRONG.** | `[V, docs.stripe.com/payments/promptpay]` lists TH as the only business location. Stripe's Thailand help page lists **Individual** and **Sole proprietorship** as supported business types `[S, support.stripe.com/questions/what-information-is-required-to-open-a-stripe-account-in-thailand, via search]`. See D1. |
| 8 | GoWabi: "Private messaging or hidden link promotion is not allowed"; 5% base commission from 1 Aug 2025; ฿500 minimum payout; 3% withholding tax | CONFIRMED | `[V, gowabi.com affiliate FAQ]` |
| 9 | The `oaMessage` URL opens an official account's chat with text pre-filled in the input field, not sent | CONFIRMED | `[V, using-line-url-scheme]`: it "enters the text message … into the message input field". The page does not say "not sent" in words, but that is the behaviour it describes. The scheme does not work in LINE for PC, and the brief does not mention this (minor; see D8). |
| 10 | ETA s.15 (automated messages are attributed to the originator) and s.17/1 (withdrawal for input errors) | CONFIRMED in substance. The summary is simplified. | jui's file quotes ETDA's English s.15 text. The judge's search found that the MDES and ETDA English texts and Tilleke cover s.17/1: it applies to a **natural person** who makes an error in **another party's** automated system, and the person must **notify the other party forthwith** `[S, mdes.go.th law 5099; tilleke.com 2019 digital reforms, via search]`. The revision leaves out these conditions. See D4. |
| 11 | LIFF: send the ID token and verify it with `POST /oauth2/v2.1/verify`. Never send profile fields from the client. The access token is revoked when the LIFF app closes. | CONFIRMED | `[V, liff/using-user-profile]` |

Extra checks:

- Abdul's prices are ฿83, ฿125, and ฿409 a month, or ฿990, ฿1,490, and ฿4,900 a year, with a 14-day trial and no card. The page links to `app.abdul.chat`. CONFIRMED `[V, abdul-ai.com]`.
- Cole calls need a paid plan and cost 40 credits a minute, with confirmation before the call. The page does not mention browser tasks. It **does** say Cole "ช่วยหาข้อมูล" (helps find information) `[V, pocodot.ai/th/line-ai-assistant]`. See D3.
- The MEA page carries 12 menu items marked `_test`, all in the sustainability section. The page also has live-looking official content and an update date of 17 Sep 2569. It lists **MEA Connect (LINE)** as a bill-check channel `[V]`. The host name `awspoc-www` makes "staging or proof of concept" a fair suspicion. "Is a staging copy" is too firm. See D6.
- Reply-token validity: the sending-messages page does not state it `[V]`. Leaving it UNVERIFIED is honest.

**Evidence tags are honest overall.** Facts from the platform side (Agent i, ActEngine, agent APIs, one official account per group, Fresha MCP) are correctly tagged `[S, via jui]` and listed as "not reopened". The single dishonest tag is in D1.

## 2. Structure, voice, and completeness of the revision

- **Structure:** all 13 sections are kept in the same order. One subsection (§10.1) and one §7 subsection were added, and both are dated. No section was removed. A line diff shows only edits and in-place extensions.
- **Voice:** additions use omm's register and the short-sentence style. Notes are marked "(added 9 Nov 2026)", "*Senior-project adaptation*", or "*Context note*". Good.
- **Change list:** 17 numbered notes cover nearly everything. A few changes are not listed (D3).
- **Dropped or distorted content:** nothing substantive. One small distortion: Cole's "research" capability was removed (D3).

## 3. Prototype scope (§10.1)

About 225–300 person-hours is a correct estimate `[I]`. The Must list has four items: capture to a confirmation card, a reminder scheduler, evidence states, and a "due before payday" summary. The Should list has two: one shared item and the `oaMessage` handoff. The Won't list is explicit. This is a sensible core that fits the time available and can be evaluated. It keeps omm's real differentiator, which is evidence-tracked completion. The evaluation plan is concrete and measurable: extraction accuracy on labelled bills with injected samples, time to a tracked obligation, fault-injected reminder correctness, task success, and SUS scores.

Gaps (all minor, D8):

- No hours are split between building, documentation, and evaluation. The proposal, reports, and the labelled dataset will take a large share.
- Voice-note input appears in §1 and §5, but §10.1 does not place it on any list.
- The quota cost decision is left open. Basic costs ฿1,280 + VAT for the evaluation month; the alternative is a daily digest. This is a real cost decision for students and should be a named open decision.
- The evaluation will most likely run after 1 Feb 2027, under the new Reply rules. The brief handles this correctly, but §10.1 could say so.

## 4. Architecture feedback

The feedback is technically correct. It covers the signature over the raw body, async handling, redelivery being off by default, an inbox table with a unique `webhookEventId`, the retry key per (occurrence, recipient), unfollow handling, LIFF ID-token verification, a Postgres job table with `SKIP LOCKED`, and Temporal kept as an upgrade path. Two refinements would help (D5):

- §8.6 says "a transactional outbox carries work from committed records to the job table". When the job table lives in the same database, inserting the job row in the same transaction **is** the outbox. A separate outbox relay adds a hop with no benefit. An outbox is needed only for an external broker.
- §8.8 should say that a `409` reply to a retry-key request means "already accepted". The handler should then mark the send done rather than raise an error. Otherwise the crash-recovery path loops.
- §8.1 signature check (minor): the header holds the **base64-encoded** HMAC-SHA256 digest. Compare it in constant time.

## 5. Defects

| ID | File and section | What is wrong | Why it matters | Exact fix | Severity |
|---|---|---|---|---|---|
| D1 | revised brief §9 "Payment experience" | "Only Stripe accounts in Thailand can accept PromptPay `[V, 9 Nov 2026]`, which needs a Thai business entity." Stripe Thailand supports Individual and Sole-proprietorship accounts. The `[V]` tag sits on a sentence whose second half was never verified and is wrong. | It is a decision-driving claim (it justifies "out of scope"), and the evidence tag is misleading. The conclusion still holds for other reasons: VAT, consumer terms, a named PDPA controller, and personal liability for the account. | Replace with: "Only Stripe accounts in Thailand can accept PromptPay `[V, S28]`. Stripe Thailand accepts individual and sole-proprietor accounts `[S, Stripe support, 9 Nov 2026]`, but a team member would then be personally liable for the account, and VAT, consumer terms, and a PDPA controller would still be needed `[I]`. That is out of scope for the senior project." Apply the same correction to validation report claim #31 and to L1 if they repeat the "entity" framing. | Major |
| D2 | revised brief §6 "Recommended booking pilot", §8.10 last paragraph, §10 "Proposed decision gates", §11 ratings ("Worth a focused paid validation") and risk register ("Charge during pilot"), §12 decision table ("First booking category", "Pilot success thresholds") | These passages are still startup-sized and carry no annotation. They conflict with the §10 "unpaid evaluation" note and with §10.1. The validator's own findings L1 (it names "Charge during pilot"), L3, and L7 recommended changing them. The global header note covers them only implicitly. | The combined proposal will take text from this brief. Unannotated gates such as "50% week-4 retention", "60% contribution margin", "paid conversion", "10–20 merchants", and "independent security review" will contradict the prototype scope. | Add a one-line dated italic note under each: "*Senior-project note (9 Nov 2026): future work. For the prototype, see §10.1.*" Under the decision-gates table, add: "For the prototype, the gates are the §10.1 evaluation measures." In the §11 risk row "Low willingness to pay", change the mitigation note to "(prototype: survey or fake-door pricing, no money collected)". Add one line to the Revision notes for this. | Major |
| D3 | revised brief §4 Cole row; Revision notes | The validator removed "research" from Cole's capabilities and replaced it with "files and Q&A". It also added "with confirmation before sending" and changed "introductory" to "one-time free". None of this is listed in the notes. The Thai page does support a research-like ability ("ช่วยหาข้อมูล"). Other unlisted changes: the §8.1 loading-animation, reply-token, and secrets bullets (note 11 does not mention them), the §4 cheapest-tier comparison sentence (L5), the MEA Smart Life addition, and the [V] tag on No Drama Padel. | The assignment requires every change to be listed. Removing a competitor capability without saying so understates the competition, even if only slightly. | Restore "help finding information (research)" in the Cole row. Extend note 3 with "added confirmation-before-send; reworded credits; replaced 'research' wording". Extend note 11 with "loading animation, reply-token limit, secret storage". Add a note for the §4 price-anchor sentence. | Minor |
| D4 | revised brief §7 "Automated messages and input errors" | The s.17/1 summary leaves out three things: it applies to a **natural person** who errs in **another party's** automated system, and the person must **notify the other party forthwith**. Whether our bot acts "on behalf of the originator" under s.15 is an open legal question (jui's file says it needs a lawyer). The text presents it as settled. | It overstates how directly the law requires the confirm step. The design conclusion (an editable confirmation) is still sound. | Add: "s.17/1 applies to a natural person dealing with another party's automated system, and requires prompt notice of the error `[S, MDES/ETDA English text via search; Tilleke 2019]`. Whether the assistant's messages count as the user's under s.15 needs legal review (not legal advice)." | Minor |
| D5 | revised brief §8.6 prototype choice; §8.8 retry-key bullet; §8.1 signature bullet | (a) A transactional outbox in front of a job table in the same database is redundant. (b) It does not say to treat a `409` with the retry key as "already accepted / done". (c) The signature is base64-encoded, and the comparison must be constant-time. | These are precision issues in the design that will be finalised by 22 Nov. (b) can cause a retry loop after a crash. | (a) "Insert the job row in the same transaction as the record that creates it; this is the outbox. A separate outbox relay is needed only for an external queue." (b) "Treat `409` ('retry key already accepted') as success and mark the occurrence sent." (c) "Compare the base64 HMAC-SHA256 digest with `x-line-signature` in constant time." | Minor |
| D6 | revised brief §4 Substitutes citation note; §13 citation status | "The S06 host is a staging copy" is stated as fact. The page has official content dated 17 Sep 2569, and the `_test` items are confined to one menu section. The page also shows MEA Connect, MEA's own LINE channel for bill checks, which is a directly relevant substitute and is missing. | Accuracy and a missed competitive fact: the utility itself is on LINE. | Say "appears to be a proof-of-concept or staging host (`awspoc-www`, `_test` menu items)". Add: "MEA also offers bill checking through its own LINE channel, MEA Connect `[V, S06, 9 Nov 2026]`." | Minor |
| D7 | revised brief §13 | S35, S39, and S41 are duplicate rows that reuse the links [s19]/[s20], [s04], and [s25]. S35 is never cited. The [s36] link points to a file on another branch, which may not resolve after integration. | Clutter and possible broken links in the combined document. | Delete S35, S39, and S41. Record "rechecked 9 Nov 2026" in the S04, S19, S20, and S25 rows. Check the S36 path after the manager integrates the branches. | Minor |
| D8 | revised brief §10.1 | There is no split of hours between build, documentation, and evaluation. Voice-note input is not on any list. The LINE plan cost (Basic ฿1,280 + VAT, or a daily digest) is not a named decision. `oaMessage` does not work in LINE for PC. | Scope realism and the test plan. | Add a rough split, e.g. about 50% build, 25% evaluation, 25% documentation `[I]`. Add "voice notes" to Won't, or move them to Should. Add a row to §12: "LINE plan for the evaluation month". Add a note that the handoff works only on mobile LINE. | Minor |
| D9 | revised brief §2 "Research limits" | It still says the snapshot is "as of 6 November 2026". The header now says facts were rechecked on 9 Nov. | A small internal inconsistency. | Change to "as of 6 November 2026, with key facts rechecked 9 November 2026". | Minor |

No blockers.

## 6. On the validation report itself

- The claim counts are internally consistent: 22 CONFIRMED + 5 WEAKER + 3 UNVERIFIED = 30, plus 1 not applicable and 3 gaps.
- The "no factually WRONG claims" finding stands for omm's original. The single wrong statement in this run is one the validator introduced itself (D1).
- Claim #36 (`[V]` "cannot be topped up") is slightly generous. The Thai page shows "-", not explicit wording. It is acceptable.
- Findings L1–L8 and A1–A15 are well reasoned. The strengths list (§5) is a fair basis for combining the three briefs.

## 7. Sources opened by the judge (9 Nov 2026)

- https://www.lineforbusiness.com/th/news/20261030 `[V]`
- https://lineforbusiness.com/th/service/line-oa-features `[V]`
- https://developers.line.biz/en/docs/messaging-api/pricing/ `[V]`
- https://developers.line.biz/en/docs/messaging-api/receiving-messages/ `[V]`
- https://developers.line.biz/en/docs/messaging-api/retrying-api-request/ `[V]`
- https://developers.line.biz/en/docs/messaging-api/sending-messages/ `[V]`
- https://developers.line.biz/en/docs/line-login/using-line-url-scheme/ `[V]`
- https://developers.line.biz/en/docs/liff/using-user-profile/ `[V]`
- https://docs.stripe.com/payments/promptpay `[V]`
- https://support.stripe.com/questions/what-information-is-required-to-open-a-stripe-account-in-thailand `[S, via search snippet]`
- https://www.gowabi.com/customer_faq/gowabi-affiliates-faq-en/ `[V]`
- https://abdul-ai.com/ `[V]`
- https://pocodot.ai/th/line-ai-assistant `[V]`
- https://awspoc-www.mea.or.th/our-services/mea-service/check-pay `[V]`
- https://www.mdes.go.th/law/detail/5099-ELECTRONIC-TRANSACTIONS-ACT--NO-3---B-E-2562-2019- and https://www.tilleke.com/resources/thailands-legal-system-undergoes-major-digital-reforms `[S, via search snippets; full s.17/1 text not opened]`
- `docs/research/ai-assistant-jui/2026-11-08-verify-agent-proposal.md` #13 (local file, read)

## Re-check (9 Nov 2026)

Scope: only the parts changed to fix D1–D9, plus the manager's question about which ETA section the brief cites. Files: `docs/review/revised/omm/business-and-architecture-brief.md` and `docs/review/validation-omm-brief.md`.

### Status of D1–D9

| ID | Status | What the judge checked |
|---|---|---|
| D1 | Fixed | §9 now says only Thai Stripe accounts can accept PromptPay `[V, S28]`. It says Stripe Thailand accepts individual, sole-proprietor, company and partnership accounts `[V, S39]`. Personal liability, VAT, consumer terms and a PDPA controller are tagged `[I]`. Validation claim #31 and finding L1 were corrected the same way. See residual R1. |
| D2 | Fixed | Dated "future work, see §10.1" notes now appear in five places: §6 (booking pilot), §8.10 (security review), §10 (gates now point to the §10.1 measures), §11 (the rating and "Charge during pilot", which now reads "survey or fake-door pricing, no money collected") and §12 (booking category and pilot thresholds). |
| D3 | Fixed | Revision notes 3, 6, 11, 12, 17, 18 and 19 now list the changes that were missing. The Cole row now says "help finding information (ช่วยหาข้อมูล)". |
| D4 | Fixed | §7 now has the caveats: s.17/1 covers a natural person using another party's system, and requires prompt notice of the error. It says legal review is needed under s.15, and it now says "support, but do not strictly require". |
| D5 | Fixed | §8.6 now says "Insert the job row in the same transaction … that is the outbox." §8.8 now treats a 409 as success. §8.1 now specifies a base64 digest compared in constant time. |
| D6 | Fixed | The MEA host is now described as one that "appears to be" proof-of-concept or staging, with official content dated 17 Sep 2569. MEA Connect (LINE) is added. |
| D7 | Fixed | S35 and S41 are removed. S04, S19, S20 and S25 are marked "rechecked". S39 is reused for the Stripe support page. The S36 path caveat is noted. |
| D8 | Fixed | §10.1 now has an estimated 50/25/25 hours split, voice notes as a Could item, and a note that `oaMessage` works on mobile only. A §12 row now covers the LINE plan for the evaluation month. |
| D9 | Fixed | §2 now says the facts were rechecked on 9 November 2026. |

### Residual issue (minor, not blocking)

- **R1.** Revised brief §10 Phase B, the "Senior-project adaptation" note, still says "charging users would need **a legal entity**, a Thai Stripe account, …". This contradicts the corrected §9. **Fix:** replace "a legal entity, a Thai Stripe account" with "a Thai Stripe account (an individual account is possible, but a member becomes personally liable)". Severity: minor. The manager can apply it during integration.

### ETA section numbering (the manager's question)

The judge opened ETDA's official English translation of the ETA B.E. 2544 (`etda.or.th/getattachment/8faa736b-…/ENG-Version.aspx`, PDF pp. 5–6) `[V, 9 Nov 2026]`. What the act says:

- **s.13**: an offer or acceptance may be made by data message. It does **not** cover attribution.
- **s.15**: paragraph 1 says "Whoever sends a data message … it shall be deemed that the data message belongs to such person". Paragraph 2(2) says a data message is deemed the originator's if sent by "an information system programmed to operate automatically in advance, by the originator or a person authorized to act on behalf of the originator".
- **s.16**: when the addressee may regard a message as the originator's.
- **s.17**: the addressee may regard the message "as being what the originator intended to send and to act on that assumption", unless it knew or should have known of an error.

**Conclusion:** the revised brief's §7 cites the **right** section. The automated-message attribution rule is **s.15 paragraph 2(2)**, so there is no defect in this card.

The other researcher's file is wrong on this point. `docs/requirements/requirement-types-research.md` (§1.1 ETA row, line 20, §1.3.9 / `LEG-06`, and limitation 9) says the rule is "s.13 para 2(2)" and that the reliance rule is "s.15". By the ETDA translation, these should be **s.15 para 2(2)** and **s.17**. That researcher worked from an English draft (SOFTIC), which numbers the sections differently. The manager should fix that file before combining. Optionally, the brief's §7 could add "(paragraph 2(2))" and "s.17 lets the receiver act on it as intended".

### Final verdict: PASS

D1–D9 are fixed, and the fixes introduce no new factual errors. R1 is a one-phrase consistency fix that the manager can make during integration.
