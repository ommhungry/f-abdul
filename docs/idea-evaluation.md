# Idea Evaluation

**Verdict:** real pain point and a workable wedge, but "AI calls the venue" is a feature, not a moat.

## Strengths
- Booking is frequent, annoying, easy to hand off
- LINE gives distribution and a familiar interface
- Calling/messaging reaches long-tail venues without online booking

## Risks
- Voice AI in Thai on noisy lines is unreliable; easily copied
- Many Thai venues use LINE OA rather than phone
- A wrong or missed booking destroys trust: need confirmations, retries, human fallback
- Per-booking cost (voice minutes, retries)
- Legal: AI call disclosure, recording consent, PDPA
- Competition: booking platforms and other assistants

## Channels
| Channel | Feasibility |
|---|---|
| Platform APIs (Chope, Fresha, Google Reserve partners) | Reliable, needs partner access |
| Web forms via browser agent | Works, brittle |
| Email | Easy, slow |
| Phone (voice AI) | Works, costly, least reliable |
| Venue LINE OA | LINE Messaging API can't message another business's OA; use user-tap deep link with prefilled text |
| Instagram/Facebook DMs | Mostly blocked for third parties |

## How the bot learns each venue's channel
1. Seed from public data (Google Places, Wongnai, Facebook/Instagram, venue sites)
2. Probe and verify top venues; keep confidence score + last-verified date
3. Learn from every booking attempt (success/failure per channel)
4. Ask venues for their preferred channel (merchant onboarding)

Note: use official APIs where possible; scraping beyond terms is a risk.

## Suggested path
1. One vertical, one city
2. Concierge MVP (human or semi-automated booking) to test demand
3. Build the moat: user preferences, venue channel database, merchant relationships
4. Revenue: per successful booking or subscription; venue commission later
