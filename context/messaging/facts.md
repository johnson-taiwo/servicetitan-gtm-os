# ServiceTitan - Authorized Copy Facts

> The ONLY source of names, numbers and claims for prospect-facing copy (`/campaign-copy` FACTS GATE). Anything not
> listed here renders as `{{placeholder}}`. **Study build:** "approved" means copied word for word from ServiceTitan's
> own public pages on the date shown. A real ServiceTitan team would replace that with a named approver. Nothing here
> is sent from this repo.

**Last updated:** 2026-11-23 by Johnson Taiwo

---

## Sender identity

| Field | Value |
|---|---|
| Sender name(s) | `{{sender_name}}` (not set: study build) |
| Title | `{{sender_title}}`, e.g. Account Executive, ServiceTitan |
| Signature format | "First / Title / ServiceTitan" |
| Sender persona notes | Peer to the owner, first person singular, respects the trades |

## What we do (locked value prop)

- **Variant A:** "ServiceTitan runs calls, dispatch, invoices and payroll in one system built for the trades."
- **Variant B:** "ServiceTitan's AI agent answers overflow and after-hours calls and books them straight onto the schedule."

Product one-liner (context, not copy): "AI software for commercial and residential trades" (homepage), sold per field
technician on 12 to 36 month terms (10-K).

## Approved claims and numbers (exact phrasings)

| Claim | Approved phrasing | Source | Approved by / date |
|---|---|---|---|
| AI phone agent | handles "overflow and after-hours calls to book, reschedule and confirm appointments" | 10-K FY2026 | public source, 2026-11-23 |
| Built for the trades | "built for the trades" | servicetitan.com | public source, 2026-11-23 |

## Numbers to keep fuzzy (never quote as hard numbers)

- Customer counts ("100,000 service professionals" vs about 10,800 Active Customers count different things).
- Average revenue growth claims ("21% in their first two years" vs "25% in just their first year": pages disagree).
- Revenue, valuation, any price or contract term.

## Authorized social-proof names

| Name | Approved phrasing (exact numbers from the case study) | Matches | Caveat | Source |
|---|---|---|---|---|
| Brothers Plumbing, Heating & Electric | cut their call abandon rate from near 30% to 5% | Residential plumbing, phones | Size not stated | servicetitan.com/case-studies/brothers-plumbing |
| Vines Restoration, Plumbing and HVAC | went from 400 to 500 abandoned calls a day to "slim to none", with 70+ trucks | Restoration, plumbing, HVAC; larger shop | Published 2023 | servicetitan.com blog, Vines story |
| Young Plumbing Services | went from 4 to 8 trucks in a year; revenue up 62% | Small residential plumbing | One year | servicetitan.com/case-studies/young-plumbing |
| Adams Power | gets service invoices out in a week and a half to two weeks, versus up to four weeks before | Commercial electric, plumbing, HVAC; 6 locations | Commercial | servicetitan.com blog, Adams Power story |
| Redding Heating & Air | call booking rate up 30% and close rate up 40% (with Max) | Residential HVAC, second location | Max is a newer package | servicetitan.com/case-studies/redding |
| Interstate AC | revenue up 33% in the first full year | Commercial HVAC and plumbing | Commercial | servicetitan.com/case-studies/interstate-ac |
| Crown Heating, Cooling and Plumbing | payroll team went from 3 people to 1 | HVAC and plumbing, union shop | - | servicetitan.com blog, Crown story |
| Home Run Roofing | grew from $1M to over $3M in the first year | Residential roofing | Roofing only | servicetitan.com blog, Home Run Roofing story |

## Proof caveats

- Match proof to the prospect: phones proof to phone-role posts, dispatch or growth proof to dispatcher posts, billing
  proof to office manager posts. Commercial proof (Adams Power, Interstate AC) only to commercial or mixed shops when the
  copy says so.
- No dispatch-specific result exists in research: cell C3 uses `{{dispatch_proof}}` and stays on hold.

## Banned

### Banned angles
- Price, discounts, contract length, "no long-term commitment".
- Any line saying the prospect is missing calls, losing money or behind (situation only, never a diagnosis).
- Naming or criticising a competitor's product (Jobber, Housecall Pro, etc.) in copy.
- Guarantees of growth or revenue.

### Banned words and phrasing (company-specific; global list in `knowledge-base/playbooks/copy-bans.yaml`)
- "users" for contractors (say contractors, shops, teams)
- "blue-collar", "just a contractor", anything that talks down to the trades

### Never in copy, under any circumstance
- Internal research notes, Trustpilot complaints, practitioner notes.

## Voice notes

- Register: plain, respectful, owner to owner.
- Hedges about the prospect: "my guess is", "could be off", "usually".
- Subjects: lowercase, 3 to 5 words, no variables.

## CTA ladder

- **Primary:** a question tied to the new hire ("Worth a look before the new hire starts?").
- **Secondary (final email):** referral ("who handles software for the office?").
- Never: calendar links, "book a call", "free".

## Internal only (never in prospect copy)

Pricing, contract terms, review-site complaints, funnel numbers, anything from `context/docs/research/practitioner.md`.
