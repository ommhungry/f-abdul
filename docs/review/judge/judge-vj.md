# Judge verdict: card vj

- Date: 9 November 2026. All web checks by the judge were made on 9 Nov 2026.
- Work judged: `docs/review/validation-vj.md` and the revised copies in `docs/review/revised/vj/` (`README.md`, `idea-evaluation.md`, `open-questions.md`, `startup-evaluation.md`, `specs/scope-and-objectives.md`), compared with the originals in `docs/review/source-vj/`.

## Verdict: REVISE (targeted fixes only)

The work is strong. It covers every topic in the assignment. The claim table is well organised, and the tally is correct: 21 claims, of which 11 confirmed, 6 weaker, 1 wrong and 3 unverified. The arithmetic is also right: 9-12 person-hours a week gives 72-144 person-hours, and the Wilson interval for 45 of 50 is 78.6-95.7%. The Phase 1 and concierge (Wizard-of-Oz) analysis is sound and useful for the software deliverable, and the revised copies keep vj's structure and voice.

Four defects still need fixing before the manager combines this work:

1. Some evidence tags are stronger than the evidence behind them.
2. One cited paper is described wrongly.
3. The Google Maps terms analysis misses clauses that bear directly on the Phase 1 recommendation feature.
4. The revision notes do not list every change.

None of these needs new research beyond reading the Google terms that are already cited.

## Spot-checks (10 claims, all opened by the judge)

| # | Claim in the work | Source opened | Result |
|---|---|---|---|
| 1 | `line.me/R/oaMessage/{ID}/?{text}` opens the OA chat and puts the text in the input field. It works on iOS and Android, not on LINE for PC. The ID and text must be percent-encoded. | developers.line.biz/en/docs/messaging-api/using-line-url-scheme/ | Correct. [V] is honest. |
| 2 | Only one OA can be in a group | developers.line.biz/en/docs/messaging-api/group-chats/ ("At any time, only one LINE Official Account can be in a group chat") | Correct. |
| 3 | LINE Thailand has 54M MAU at 31 Mar 2026 | lycorp.co.jp/en/company/global/ | Correct. [V] is honest. |
| 4 | Google Maps Platform ToS s.3.2.3(a): no "copy and save business names, addresses, or user reviews"; no use with text-to-speech, which is (a)(iv) | cloud.google.com/maps-platform/terms | Correct, quoted accurately. **But see defect D3:** other clauses of the same section were missed. |
| 5 | Place IDs may be stored indefinitely; lat/lng may be cached for 30 days | developers.google.com/maps/documentation/places/web-service/policies | Correct. |
| 6 | Wongnai ToS forbids collecting shop data, addresses and reviews "for any purpose" without permission | wongnai.com/terms, item (3): "ไม่รวบรวมข้อมูลร้านค้า ที่อยู่ร้าน รีวิว ... เพื่อวัตถุประสงค์ใด ๆ ก็ตาม เว้นแต่จะได้รับอนุญาตจากทางบริษัท" | Correct. The judge could not confirm that the clause is numbered "s.2"; it is item (3) in the list of user duties. |
| 7 | Grab Dine Out "Book Table": 500+ brands in Bangkok and Phuket, premium, press release of 4 Sep 2025 | grab.com/th/en/press/others/grabdineout2025/ | Correct. The release says "mid-to-premium" and credits Chope. |
| 8 | Thai ASR: 6.8-10% CER "on clean benchmarks"; "no Thai phone-line or noisy-audio benchmark" | arxiv.org/abs/2601.13044 (full PDF) | **Partly wrong.** See defect D2. |
| 9 | LINE Reply API: paid above 25,000 messages a month from 1 Feb 2027; ฿5,000-a-month unlimited plan | lineforbusiness.com/th/news/20261030 | Correct. It is bought through an Agency Partner. |
| 10 | Free plan push quota is 300 a month | lineforbusiness.com/th/news/20240619_1 (cut from 500 to 300, effective 1 Aug 2024) | Correct. |
| 11 | Instagram: "Only after an Instagram user has sent your app user's Instagram professional account a message can your app send a message"; 24-hour window | developers.facebook.com Instagram Messaging API | Correct. The window can be 7 days after a Click-to-Direct ad, which is not material here. |
| 12 | Actions Center lists Thailand for reservations | Actions Center Business Link platform-policy tables (developers.google.com) | Correct: TH is listed as supported. |
| 13 | Hungry Hub: the reservation tool failed because people walk in, so it pivoted | e27.co (2023-02-20 article) | Correct. |

## Defects

### D1. Evidence tags inflated on the OA-to-OA claim (major)

**Where:**
- `revised/vj/idea-evaluation.md` line 35: "LINE Messaging API can't message another business's OA [V, via jui]"
- `revised/vj/startup-evaluation.md` line 20: "(cannot message another business's OA [V])"
- `revised/vj/specs/scope-and-objectives.md` line 61: "[V, via jui]" covers both claims in the sentence

**What is wrong:** The validation report itself (claim #1) says that no OA-to-OA path is documented, and that this part is an **inference** [I] from a bot sending only to its own users, groups and rooms. The revised copies promote the inference to [V]. The brief requires honest tags.

**Fix:** Tag the OA-to-OA statement "[I, from LINE sending-messages docs]" and keep [V] only for "only one OA can be in a group". In the startup evaluation, change "[V]" to "[I]".

### D2. Thai ASR paper described wrongly (major)

**Where:**
- `validation-vj.md` claim #7 ("6.8-10% character error rate on clean benchmarks ... We found no Thai phone-line or noisy-audio benchmark")
- `validation-vj.md` claim #8 ("clean-speech results only")
- `revised/vj/idea-evaluation.md` line 18 ("6.8-10% character error rate on clean speech")

**What is wrong:** The cited paper, "Typhoon ASR Real-time" (arXiv 2601.13044), includes **TVSpeech**, a curated noisy, "in-the-wild" benchmark built to "bridge the gap between academic 'clean speech' and ... real-world environments". The paper reports 6.81% CER on Gigaspeech2-Typhoon. On TVSpeech, a Whisper model trained on the Typhoon data pipeline drops from 10.36% to 6.32% CER, and Gemini 3 Pro scores 10.95%. So a noisy-audio benchmark does exist. What is missing is a **telephone (narrowband) speech** benchmark. The conclusion still holds (Thai voice over phone lines is unproven), but the cited source is misdescribed.

**Fix:** Rewrite claim #7 as: "Thai ASR reaches about 6-11% CER on read speech (Gigaspeech2) and on noisy real-world TV audio (TVSpeech) [S, arXiv 2601.13044]. No telephone-audio benchmark was found." In claim #8, delete "clean-speech results only". Make the same change in `revised/vj/idea-evaluation.md` line 18.

### D3. Google Maps terms analysis is incomplete for the recommendation feature (major)

**Where:**
- `validation-vj.md` claim #16
- `revised/vj/idea-evaluation.md` step 1
- `revised/vj/specs/scope-and-objectives.md` line 37

**What is wrong:** The work quotes only s.3.2.3(a). The same section also says:
- **(b):** no caching except as the Service Specific Terms permit.
- **(c)(vii):** Customer will not "use Google Maps Content to improve machine learning and artificial intelligence models, including to train, test, validate or fine-tune the models".
- **(d)(iii):** Customer will not "use the Google Maps Core Services in a listings or directory service".
- **(e)(i):** Customer will not "display or use Places content on a non-Google Map".

Phase 1 keeps "venue recommendations" in scope and proposes a labelled test set for intent and slot accuracy. Both can collide with these clauses:
- A test set built from Places content runs into (c)(vii).
- A venue list that shows Places details in LINE needs a check against (d)(iii), plus Google's attribution rules for Places content shown without a Google map.

The recommended fix ("store only place IDs plus facts we verify") is necessary but not sufficient.

**Fix:**
1. Add (c)(vii), (d)(iii) and (e)(i) to claim #16 [V, cloud.google.com/maps-platform/terms].
2. State the design rule: fetch Places details at display time by place ID, show Google attribution, and never put Places content into evaluation or test datasets.
3. Mark "Is a booking-assistant venue list a 'listings or directory service'?" as an open question for the team.
4. Add one bullet to the scope constraints.

### D4. Revision notes do not list every change (major)

**What is wrong:** The assignment and the method require each revised copy to list every change. Several changes are made silently.

| File | Changes made but not listed |
|---|---|
| `revised/vj/README.md` | The note says "Router order corrected", but the order is unchanged (API, web form, LINE handoff, email, phone). What actually changed is the content of each step, including "phone: human in Phase 1". Also not listed: the 54M MAU figure added to Core idea, and the team added to Status. |
| `revised/vj/idea-evaluation.md` | The Strengths rewrite (restaurant walk-in caveat, 54M MAU, long-tail source); competitors now named in Risks; the LINE Reply API cost added; Suggested path step 2 reframed as Wizard-of-Oz; the text-to-speech note. |
| `revised/vj/startup-evaluation.md` | The stage line edited ("team details" removed); rows 3 and 5 changed (cost evidence, data-source terms); confidence on row 4 raised; the "Valuation today" metric list rewritten (50-100 paying users and the >90% target replaced); unknown #5 added; the LINE Pioneer Program added under unconventional criteria. |
| `revised/vj/specs/scope-and-objectives.md` | The Phase 1 objective dropped "and pay"; the "Why" paragraph dropped "frequent"; "Budget and runway" became "Budget"; "Additional verticals (courts, salons, spas)" lost its list; PDPA notice and outcome-check items added to scope; the merge note added to out-of-scope. |

**Fix:** Expand each "Revision notes (9 Nov 2026)" block to a complete list of changes, one bullet each. Correct "Router order corrected" to "Router steps annotated with current feasibility; order unchanged".

### D5. New claims in revised copies with no source in the validation report (minor)

**Where:** `revised/vj/open-questions.md` line 22: "LINE: no native OA booking feature found in Thailand; third-party add-ons and MINI Apps exist [S]."

**What is wrong:** `validation-vj.md` contains no evidence for this, and no source is named. (The Pioneer Program / LINE HACKS 2027 line does trace to jui's verify file [S, thansettakij.com], which is fine.)

**Fix:** Add the source and its access date, or tag the claim UNVERIFIED.

### D6. Confidence for competitive position inconsistent (minor)

**Where:** `validation-vj.md` §3.1 row 4 says "2 (High confidence)", but `revised/vj/startup-evaluation.md` line 19 says "Medium-High".

**Fix:** Use one value in both places.

### D7. Missed implication of LINE's agent program for the OA-to-OA constraint (minor)

**Where:** `validation-vj.md` claim #1 and §5.

**What is wrong:** jui's verified research (`2026-11-08-verify-agent-proposal.md` line 121) reports that LINE announced agent APIs and agent-to-agent protocols (LINE DEV CONF 2026). That could relax the OA-to-OA limit that drives vj's whole handoff design. The revised copies mention the Pioneer Program only as a networking route.

**Fix:** In claim #1 and in the scope constraints, add: "May change if LINE's announced agent-to-agent APIs open up [S, via jui]; design the handoff as a replaceable channel adapter."

### D8. Minor accuracy and tidiness points (minor)

- `revised/vj/startup-evaluation.md` line 18: "paid above 25,000 a month" is missing the word "messages".
- `revised/vj/README.md` line 18 cites "[S, validation #3]". An internal report is not a secondary source. Cite the underlying sources or write "(see validation-vj.md #3)" without a tag.
- `[V, via jui]` does not fit the brief's definition of [V] ("a primary source you opened"). The header discloses this, and the judge re-opened four of these sources and confirmed them. Still, the tag should read "[V, opened by jui 8 Nov 2026]" to keep the definition honest.
- The title of `startup-evaluation.md` says "10 criteria + 5 extras", but only 4 extras are listed. This is pre-existing in vj's original and the validation did not flag it. Note it or fix the count.
- `validation-vj.md` claim #16 gives the Wongnai clause as "s.2(3)". The judge confirmed the text but not the section number. Cite it as "item (3), user obligations" unless the number is confirmed.

## What is right and should be kept

- The correction of claim #3 (Reserve with Google is inbound only; Fresha and Chope have no open booking API) is accurate and drives the right design decision.
- The observation that **the shop's reply after an `oaMessage` handoff lands in the user's chat, not the bot's**, is a real and important gap. Turning it into an explicit outcome-check requirement is correct.
- The Phase 1 metric critique is well reasoned. It includes the time budget, per-target effort, and why 50 bookings cannot show ">90%". The replacement targets are realistic for 3 students at 3-4 h a week.
- The Wizard-of-Oz framing is valid. The Dahlbäck, Jönsson and Ahrenberg (1993) citation is correct, and it gives the senior project a real software deliverable: an operator console, a channel router, logged human steps, and a separate software evaluation.
- The vertical-by-vertical channel table (§4) fills a real research gap.

## Scope note

This card does not cover the Abdul teardown (that is the abdul-recon card) or the full requirement-type research (the requirements card). The revised scope adds software-quality metrics, but it does not classify requirements by type. The manager should make sure the combined proposal maps vj's anti-goals and constraints into the requirements specification (for example: safety, "never claim success without confirmation"; legal and compliance, PDPA and data-source terms; interface, LINE URL scheme limits).

## Re-check (9 Nov 2026)

Scope: only the parts changed since the first verdict, in `validation-vj.md` and the five files under `revised/vj/`. Revision notes were compared against `source-vj/`.

| Defect | Status | Evidence |
|---|---|---|
| D1 Tags inflated on OA-to-OA | Fixed | The OA-to-OA claim is now tagged "[I, from LINE sending-messages docs]" in validation claim #1, idea-evaluation line 61, startup-evaluation row 5 and scope Constraints. [V] is kept only for "one OA per group". |
| D2 ASR paper misdescribed | Fixed | Claims #7 and #8 and idea-evaluation lines 35-37 now say "about 6-11% CER on read speech (Gigaspeech2) and noisy TV audio (TVSpeech); no telephone-audio benchmark". This matches the paper (6.81% on Gigaspeech2; 6.32-10.95% on TVSpeech). |
| D3 Google s.3.2.3 incomplete | Fixed | The quotes for (a), (a)(iv), (b), (c)(vii), (d)(iii) and (e)(i) in claim #16 match the live terms word for word (re-read 9 Nov 2026). The design rule is stated in claim #16, idea-evaluation step 1, and the scope venue-database bullet and Constraints. The "directory service" open question was added to claim #16 and open-questions. |
| D4 Incomplete revision notes | Fixed | All five notes blocks now list each change. A diff against `source-vj/` found one small omission (see R1). The README now says the router order is unchanged. |
| D5 Unsourced LINE claim | Fixed, with one caveat | "No native OA booking feature" is now tagged UNVERIFIED as a negative. The IHG LINE MINI App claim checks out: IHG release of 14 Aug 2026 covering Thailand and Taiwan. The Goragod source did not resolve (see R2). |
| D6 Confidence mismatch | Fixed | "Medium-High" is now used in both validation §3.1 and startup-evaluation row 4. |
| D7 LINE agent APIs | Fixed | Added to claim #1, §5 item 3, the idea-evaluation LINE row and scope Constraints, with the "replaceable channel adapter" design note. |
| D8 Tidiness | Fixed | Changes made: "messages" added; the README cite is now "(see validation-vj.md #3)" with no tag; "[V, opened by jui 8 Nov 2026]" is used throughout and defined in each notes block; the title now reads "4 extras"; the Wongnai clause is cited as "user obligations item (3)". |

No new factual errors were found. No new tags are stronger than their evidence.

### Remaining minor points (non-blocking; the manager may fix them during integration)

- **R1 (minor).** `revised/vj/startup-evaluation.md` row 9 also dropped "lifestyle apps" ("large retail or bank groups with lifestyle apps" became "bank or retail groups"). The notes mention only dropping "travel". Add it to the row-9 note.
- **R2 (minor).** `revised/vj/open-questions.md` line 27 cites "chat.goragod.com". That host did not resolve on 9 Nov 2026 (DNS failure). Give a working URL, or drop the example and keep the IHG one.
- **R3 (minor).** `revised/vj/specs/scope-and-objectives.md` line 67 cites "s.3.2.3(b)" for "show Google attribution". Section (b) covers caching only. Attribution comes from the Places policies page or the Service Specific Terms. Cite that page instead.

### Final verdict: PASS

All eight defects are fixed. The three items left are minor citation tidy-ups and do not affect any decision.
