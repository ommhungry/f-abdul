# Judge verdict: abdul-recon (9 November 2026)

- Card: abdul-recon. The user asked for deep reverse engineering of Abdul "with anything", with tools installed if needed. The manager limited the work to passive public information, because Abdul's Terms of Service forbid users to "reverse-engineer, disrupt, scrape, probe, or exploit Abdul".
- Files judged: `docs/research/abdul-recon/abdul-recon-2026-11-09.md` (the "report"), `docs/research/abdul-recon/manual-blackbox-test-script.md` (the "script"), `docs/research/abdul-recon/raw-notes/*`, and the tools log `~/tools/re-kit (local, not in repo)/README.txt`. I compared against `docs/research/ai-assistant-jui/2026-11-08-abdul-teardown.md` (the "teardown").
- Judge's own lookups: all passive and low-rate, done on 2026-11-09. I ran `dig` against public resolvers, made one RDAP query per domain through rdap.org (3 s apart), made one crt.sh query, ran one `curl -I` on https://abdul.chat/, and made one fetch of the public LINE OA page. I did not contact api.*, media.*, admin.* or app.* hosts.

## Verdict: PASS WITH MINOR FIXES

I found no boundary violation, and the core infrastructure facts check out. Four defects need text fixes before the team relies on the report: the Cloudflare account claim is overstated, the ฿199 monthly estimate contradicts the page, the n8n tag is wrong, and the script would stop at registration. None needs new research beyond what the boundary allows.

---

## 1. Boundary compliance

| Check | Result | Evidence |
|---|---|---|
| Requests to api., media. or admin. hosts | **None found.** These hosts appear only as DNS answers. | `raw-notes/subfinder-passive-2026-11-09.txt` has only `A`/`CNAME` lines. `headers-2026-11-09.txt` lists only abdul.chat, app.abdul.chat, abdul-ai.com, www.abdul-ai.com (did not resolve), nativelabs.studio, and robots.txt. |
| Reading or de-minifying Abdul's JavaScript | **None found.** | `~/tools/re-kit (local, not in repo)/evidence/abdul-2026-11-08/` holds only HTML, the text extracts, and sitemap.xml. It has no `.js` files. The pricing text templates the report quotes are in the server-rendered HTML, not in a script bundle. |
| Traffic capture, HAR files, proxies | **None.** | `re-kit/captures/` and `re-kit/specs/` are empty. |
| Chatting with the bot | **None.** | The report (section 9) and the script both say so. Nothing in raw-notes shows bot traffic. |
| Active scans or brute force | **None.** subfinder is passive by design: it queries third-party datasets and has no brute-force mode. The team ran it at `-t 2 -rl 2`. Resolving the results with `dig` is a normal DNS lookup. | README.txt install log; `subfinder-passive-2026-11-09.txt` |
| webanalyze | Acceptable. It ran with `-crawl 0 -worker 1`, so it made one GET per page and read only the HTML and headers. The log shows `search subdomains: true`, but that setting has no effect when crawl is 0. | `webanalyze-2026-11-09.txt` |
| app.abdul.chat | **Borderline, but acceptable.** The analyst ran `curl -I` and followed the 307 redirect to `/home`, then ran webanalyze once on the signed-out landing page. That page is public and linked from the marketing HTML. Nobody signed in. Strictly, the brief allows headers and HTML of "public marketing pages", and the app's landing page is not a marketing page. Report section 9 discloses the request. No further requests to app.* should be made (see defect D-9). | `headers-2026-11-09.txt` |
| Personal data | **No personal data about individuals.** The whois output shows only `Registrant State: Bangkok, Country: TH` and an empty organisation. The only e-mail address is the company address hello@abdul-ai.com, which is fine. I found no phone numbers. My regex hit one false positive, a Wayback timestamp. The GA4 ID, the PostHog key prefix, and the Google site-verification token are public and belong to the company. | grep of every file in the folder |
| Third-party data with no link to Abdul | Minor. `ct-summary` and `ct-nativelabs.studio.json` list hostnames for 4 of the studio's other clients (names redacted here). These are business or project labels, not personal data. They are irrelevant to Abdul, and one label could be a person's nickname. The report text does not name them. Redact them for data minimisation (D-8). | raw-notes |

## 2. Correctness: independent re-checks

| # | Claim in the report | My passive check (2026-11-09) | Result |
|---|---|---|---|
| 1 | abdul-ai.com registered 2026-06-17; Cloudflare Registrar; harlee/thomas | RDAP: registration 2026-06-17T02:22:27Z, registrar Cloudflare, Inc., NS HARLEE/THOMAS | **Confirmed** |
| 2 | abdul.chat registered 2026-09-02; meg/titan | RDAP: registration 2026-09-02T09:30:14Z, NS meg/titan, registrar Cloudflare | **Confirmed** |
| 3 | nativelabs.studio registered 2026-08-24; meg/titan | RDAP: registration 2026-08-24T07:55:53Z, NS meg/titan, registrar Cloudflare | **Confirmed** |
| 4 | Railway Singapore edge | `curl -I https://abdul.chat/`: `x-railway-edge: sin1`, `x-hikari-trace: sin1.98a6`, `server: cloudflare`, `x-powered-by: Next.js`, `x-middleware-rewrite: /th`, `cf-cache-status: DYNAMIC` | **Confirmed** |
| 5 | No SPF or DMARC on any of the three domains; MX on abdul-ai.com only | `dig TXT` shows no SPF on any domain. abdul-ai.com has only `google-site-verification`. `_dmarc` TXT is empty on all three. MX is `1 smtp.google.com` for abdul-ai.com and none for the others. | **Confirmed** |
| 6 | Certificates: abdul.chat Let's Encrypt 2026-09-02, then wildcard SSL.com 2026-09-04 | crt.sh shows exactly these two entries | **Confirmed** |
| 7 | LINE OA has 41,998 friends; profile links abdul-ai.com; description "AI ผู้ช่วยส่วนตัว" | page.line.me/319gqomd page data: `friendCount: 42003` a few hours later. The profile links abdul-ai.com and the description matches. | **Confirmed** (consistent) |
| 8 | Prices shown by default equal the yearly price divided by 12 | In the saved HTML, each card shows "฿125 / เดือน · จ่ายรายปี ฿1,490" (likewise 83/990 and 409/4,900). The templates `"orAnnual":"หรือ {perMonth}/เดือน เมื่อจ่ายรายปี"`, `"normally":"ปกติ"` and the "ประหยัดสูงสุด 38%" badge are present. 990/12 = 82.5, 1,490/12 = 124.2 and 4,900/12 = 408.3, so the shown prices are these values **rounded up**. | **Confirmed**, with one error in the follow-on estimate (D-2) |
| 9 | OA badge status unknown (section 8) | The same OA page data has `"badgeType":"certified"`, which is LINE's certified (blue) badge | **Resolvable.** The report says it is unknown, but it can be answered passively (D-6). |

### Manager concern: "very likely in the same Cloudflare account"

**Decision: overstated. It must be reworded.**

- Cloudflare gives a new zone a name-server pair from a limited pool. Zones in one account usually get the same pair. But the pool is small, so each pair is shared by a very large number of unrelated accounts. A matching pair is consistent with one account. On its own it is weak evidence, not "very likely".
- The report also ignores evidence that cuts the other way. abdul-ai.com belongs to the same product but is on a **different** pair (harlee/thomas). By the report's own logic, that would put the product's two domains in different accounts. So the pair does not reliably track ownership here.
- There is better passive evidence, and the report did not use it. RDAP shows "last changed" events for both domains on **2026-09-07, 09:22:59Z (nativelabs.studio) and 09:30:19Z (abdul.chat)**, about seven minutes apart. Both domains also use Cloudflare Registrar. This suggests that one operator worked on both domains in the same session. It is still an inference.

**Correct wording** (replace the bullet in section 2.1):

> abdul.chat and nativelabs.studio use the same Cloudflare name-server pair (meg/titan) [V DNS, RDAP]. Cloudflare usually gives all zones in one account the same pair, but each pair is shared by very many unrelated accounts, so this alone is weak evidence. Both domains also use Cloudflare Registrar, and their registry records were last changed about seven minutes apart on 2026-09-07 [V RDAP]. Together this is consistent with one operator managing both domains [I, low-to-moderate confidence]. abdul-ai.com uses a different pair (harlee/thomas), which shows the pair does not reliably track ownership. The footer credit "Made by nativelabs.studio" remains the main evidence linking the studio to Abdul [V].

## 3. Quality

- **Evidence tags.** Mostly good. The report uses [I] 19 times, and the whole architecture diagram is labelled [I]. Two tags are wrong:
  - "the studio builds client AI agents and runs n8n [V]". A certificate for `n8n.nativelabs.studio` shows only that the hostname existed, not that n8n runs there. That part should be [I]. "Builds AI agents" is [V] from the studio's own site.
  - Section 4.1 tags its "no hits" searches as [V]. This is acceptable, but it would read better as "searched 2026-11-09, no hits", because absence is not verified.
- **Architecture diagram.** Plausible. Each box traces to a source: hostnames to DNS and subfinder; Railway sin1, Cloudflare and Next.js to headers; and OpenRouter → DeepSeek on Together and Fireworks, Gemini, the TypeSafe AI classifier with member codes, Voyage, Firecrawl, Backblaze, Stripe, PostHog and Clarity to the privacy policy, which I checked in the saved text. Two small omissions do not change the picture. The diagram does not show a Stripe webhook back to the core. It places "self-hosted web search" on Railway, but the privacy policy does not say where that search runs.
- **Gap analysis.** Useful. It separates parity, the opening, and a new requirement, and it marks bill payment as high risk. Its bottom line is honest: Abdul could close the gap quickly, so the moat must be business-side reach and a closed loop. It would be stronger if each "opening" row named which member direction it supports (omm, vj or jui).
- **Implied requirements.** Usable as **inputs** to a requirements specification, but not yet as specification items. P-1..P-10 are sourced. P-11 is sourced as [I]. P-5 adds "with a stop rule", which is our own addition, and the text should say so. P-10 is a design constraint rather than a functional requirement. The D-1..D-8 table has no Source column, and none of the items has an acceptance criterion. The specification author will need to add priority, rationale and a testable criterion to each, for example "D-3: no committing action without an explicit confirm tap, verified by test X". I accept this for a recon file. It is not a defect at this stage, but the specification author must not copy these items verbatim.
- **Teardown corrections.** The header says the report "corrects two points in the teardown", but only one correction is labelled (pricing). The other is not identified.
- **Manual test script.**
  - **Safe.** It is for a human typing by hand only, at no more than 10 messages in 10 minutes. It forbids jailbreaks, prompt injection and system-prompt extraction. It uses no real personal data, a consenting test group, a fresh Google account, and no card details. It ends with deletion and revocation. This fits the normal-customer standard.
  - **Practical,** with one blocking problem. Abdul's privacy policy says registration collects a **mobile number**. T01 says "do not enter a real phone number … stop". Entering a fake number would also mean giving Abdul false registration details, which is itself questionable under the terms. As written, the script will most likely stop at T01.
  - **Mostly complete.** It is missing tests for three unknowns that the report itself lists:
    - the personal morning briefing and the group "ทุกเช้า 8 โมงสรุปงาน" (Flows E and F, and the "daily recap: coming soon" conflict in sections 4.5 item 10 and 8);
    - Buddhist-era years and Thai month names (section 4.5 item 2);
    - a voice message (section 4.5 item 9).
  - **Small slips.** Part I says "1 test" but contains T39 and T40. Part B times T06 for 02:00, so the tester must be awake to see it fire.

## 4. Defects

| ID | File and section | What | Why it matters | Exact fix | Severity |
|---|---|---|---|---|---|
| D-1 | Report §2.1, bullet 3 | "Cloudflare assigns a name-server pair per account, so they are very likely in the same Cloudflare account" | Overstated. Name-server pairs are shared by many unrelated accounts, and the product's own abdul-ai.com uses a different pair. The stronger RDAP signal is missing. | Replace with the wording in §2 above. In the §3.2 timeline, add "2026-09-07: RDAP last-changed event on both nativelabs.studio (09:22Z) and abdul.chat (09:30Z) [V RDAP]". | Moderate |
| D-2 | Report §4.4, "Correction to the teardown" | Says "If 38% is the yearly saving, the monthly-billing ร่างทอง price would be about ฿199". The page shows a **"ฟรี 2 เดือน"** seal on ร่างทอง and เถ้าแก่, so yearly = 10 × monthly. That gives about **฿149** for ร่างทอง (1,490/10) and about **฿490** for เถ้าแก่ (4,900/10). The "up to 38%" then most likely belongs to วัยรุ่น, which has no seal: 990/0.62/12 ≈ ฿133. Also, 1,490/12 is 124.2 but the page shows 125, so the shown prices are rounded up. | The headline correction gives a figure that the page itself contradicts. | Replace the estimate sentence with: "The ร่างทอง and เถ้าแก่ cards carry a 'ฟรี 2 เดือน' seal, which implies monthly billing of about ฿149 and ฿490 (yearly ÷ 10). The 'save up to 38%' figure most likely applies to วัยรุ่น, implying about ฿133 a month (**estimates**, UNVERIFIED until T31). Shown prices are yearly ÷ 12, rounded up." Change "1,490/12 ≈ 124" to "≈ 124.2, shown as ฿125". | Moderate |
| D-3 | Script, T01 and ground rule 3 | The script forbids a real phone number, but registration requires one (privacy policy). Using a fake number would mean giving false registration details. | The whole run will probably stop at T01, so the script is impractical as written. | Replace the T01 instruction with: "Registration asks for a mobile number. The tester may use their **own** number only with informed consent, knowing that Abdul stores it under its privacy policy. Never record the number in notes or the repo. Do not use a fake or someone else's number. If no team member consents, stop and record 'blocked at registration'." Add the same point to ground rule 3. | Moderate |
| D-4 | Report §2.3, last bullet | "So the studio builds client AI agents and runs n8n [V]" | A certificate shows only that a hostname existed. That n8n runs there is an inference. | Change to: "The studio's site says it builds AI agents for Thai businesses [V nativelabs.studio]. An n8n hostname certificate suggests it uses n8n [I]." | Minor |
| D-5 | Script, missing tests | No tests for the morning briefing or group daily summary, Buddhist-era years and Thai month names, or a voice message | These are open items the report says the script will settle (§4.5 items 2, 9, 10; §8 "daily summary"). | Add: T41 `ทุกเช้า 8 โมงสรุปงานวันนี้ให้หน่อย` (personal) and the group variant in Part G. T42 `วันที่ 5 ม.ค. 2570 เตือนต่อภาษีรถ`. T43 a 5-second voice note saying "พรุ่งนี้ 9 โมงเตือนโทรหาหมอ" (fake content). Record whether each works. | Minor |
| D-6 | Report §8, last row | The badge is listed as "not shown either way" | The public OA page data contains `"badgeType":"certified"`, a passive [V] answer. | Change the row to a finding: "Certified (blue) badge [V page.line.me data, 2026-11-09]". Remove it from §8. | Minor |
| D-7 | Report header line 4 | "corrects two points in the teardown", but only one correction is labelled | The reader cannot find the second correction. | Either label the second correction explicitly, or change the text to "corrects one point (pricing) and adds details". | Minor |
| D-8 | raw-notes `ct-summary-2026-11-09.txt`, `ct-nativelabs.studio.json` | The files list the studio's unrelated client project hostnames | Data minimisation. These are third parties with no link to Abdul, and one label may be a person's nickname. | Replace the client labels with `<client-1>`…`<client-4>` in the summary. Reduce the nativelabs JSON to the n8n and apex entries, or drop it. Keep the count ("4 client project hostnames"). | Minor |
| D-9 | Report §1 and §9 | Fetching app.abdul.chat (headers, the redirect to /home, and webanalyze) is disclosed, but the report does not say this lies at the edge of the brief | Transparency. The brief allows HTML and headers of public marketing pages, and app.* is the product itself. | In §9 item 1, add: "This is the outer edge of the boundary. Future runs should not request app.* at all; the marketing site already shows the same stack." | Minor |
| D-10 | Script, Part I heading | Says "1 test" but contains T39 and T40 | Consistency | Change it to "2 steps, mandatory". | Minor |
| D-11 | raw-notes `whois-2026-11-09.txt` | The lines "created: 2015-01-29 / 1985-01-01 / 2015-06-25" are IANA **TLD** creation dates | A reader could mistake them for domain dates. | Add a one-line note: "(IANA TLD records, not the domains)". | Minor |

## 5. What I could not verify

- The real monthly-billing prices. They are computed by page script, and I did not run or read it, in keeping with the boundary. T31 is the right way to settle them.
- Whether Cloudflare's current assignment logic still gives one pair per account. This is general knowledge, not something I could observe passively for these domains. The corrected wording does not depend on it.

---

## Re-check (9 Nov 2026)

Scope: only the changed parts of the report, the script and `raw-notes/`. This re-check made no network requests; every check was local (grep, jq, reading files).

| Defect | Status | What I checked |
|---|---|---|
| D-1 Cloudflare wording | **Fixed** | §2.1 now uses the judge's wording word for word. The §3.2 timeline has the 2026-09-07 RDAP row (09:22Z and 09:30Z), which matches my RDAP results. |
| D-2 Pricing | **Fixed, arithmetic correct** | 990/12 = 82.5 → ฿83; 1,490/12 = 124.17 → ฿125; 4,900/12 = 408.33 → ฿409 (rounded up). Under the "ฟรี 2 เดือน" seal: 1,490/10 = ฿149 and 4,900/10 = ฿490. For 38% on วัยรุ่น: 990 / (12 × 0.62) = ฿133.06. All three are labelled estimates, UNVERIFIED until T31. |
| D-3 Registration phone | **Fixed** | Ground rule 3 and T01 now say: the tester's own number with informed consent, never recorded, no fake number, otherwise stop. |
| D-4 n8n tag | **Fixed** | Now [I]. "Builds AI agents" stays [V] from the studio site. |
| D-5 Missing tests | **Fixed** | T41 (personal briefing), T41g (group summary), T42 (BE year 2570 and ม.ค.) and T43 (voice note) were added, each with a clean-up step. |
| D-6 Badge | **Fixed** | Recorded as `badgeType: certified` [V] in the timeline and removed from §8. |
| D-7 "Two corrections" | **Fixed** | The header now reads "corrects one point (pricing) … adds pricing details". |
| D-8 Client hostnames | **Fixed** | `ct-summary` uses `<client-1>`..`<client-4>` with a note. `ct-nativelabs.studio.json` now holds only the apex, www, wildcard and n8n entries. A repo-wide grep of `the local clone` for the four client labels finds **no matches** (I also removed them from this judge file). |
| D-9 app.* edge | **Fixed** | §9 item 1 marks it as the outer edge and says future runs should not request app.*. |
| D-10 Part I count | **Fixed** | Now "2 steps, mandatory". |
| D-11 whois TLD dates | **Fixed** | Each line is annotated "(IANA TLD record, not the domain)", with a header note. |
| Extra: §4.1 no-hit searches | **Fine** | Now "searched 2026-11-09, no hits; absence is not proof". |
| Extra: T06 | **Fine** | The tester records the echoed date, checks the push the next morning, then cancels. |
| Extra: §7 inputs note and D-x Source column | **Fine** | The note states that the items are inputs, not specification items. It flags P-5 and P-10 and separates the D-x requirement IDs from the judge's defect IDs. Sources trace to §6, the TEAM-BRIEF directions and the privacy policy. |

**New boundary issues:** none. The changes are text only. The script additions are ordinary-user messages sent by hand, with fake content.

**Remaining nit (optional, not blocking):** the D-8 source cites the privacy policy's "the request fails rather than being sent to one that does not" retain-data rule. That rule is about privacy-safe routing, not about falling back when the system is uncertain. Better wording: "loosely analogous to".

### Final verdict: PASS
