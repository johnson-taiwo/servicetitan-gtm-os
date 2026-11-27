# Week 2B: Audience segment + Workflow (Segment, Enrich, Act)

**Status:** LIVE in Clay (V2: staff count + AI fit), 27 Nov 2026. Workflow: https://app.clay.com/workspaces/736170/workflows/tc-workflows/wf_0tp63p7jJYZwTEDMV7q

Week 2 proved the logic in a table (tables are for testing). Week 2B moves it to where it runs at scale: the companies
live in Clay **Audiences**, a dynamic **segment** picks the ones worth acting on, and a **Workflow** enriches and routes
each one as it enters the segment.

## 1. Audience criteria (one sentence)
**US-headquartered trade contractors in Clay's 11-200 staff band that posted any job in the last 30 days and are not a
likely ServiceTitan customer.** Exclusions: companies whose job posts ask for ServiceTitan experience (likely customers),
companies with no website (no ID to match on), anything outside the US. Source: Week 1 ICP (`context/icp/icp.md`) and the
Week 2 trigger (`workflow-spec.md`).

## 2. The segment
- **Universe:** 56 companies loaded into Audiences > Companies from the Week 2 table with Clay's "Create or update
  Audiences record" step, keyed on domain (0 credits). 2 of the 58 have no website and could not be loaded.
- **New Audience fields** carry the Week 2 signal: Size band (Clay), Hiring role family, Role posted, Job posted in 30
  days, Likely ServiceTitan customer; plus the outputs 2B route and 2B route reason.
- **Segment "ServiceTitan - US trade contractors hiring (30 days)":** Country = United States AND Job posted in 30 days
  = true AND Likely ServiceTitan customer = false AND Size band is any of 11-50, 51-200. **49 companies**, the same 49 that
  passed Week 2's free checks. A random sample of 5 was checked against every criterion.

## 3. The enrichment and the AI fit check
**Enrich company** on the record's domain (identifier preserved from the trigger) returns the real staff count. Week 2
showed Clay's size band is wrong for about 1 in 5 small contractors, so the band only selects; the count decides.

**AI fit (added in V2, same prompt and model as the Week 2 AI column):** is this a home or building service contractor in
HVAC, plumbing, electrical or garage doors? Answers yes / no / unclear only from the enrichment text, with the exact
words as evidence, written onto the record. Size alone let through a capital firm, a locksmith, controls integrators and
a staffing firm; the AI step drops them.

## 4. The Workflow and the routing rule
```
Segment (record enters) -> Enrich company (staff count) -> AI fit (service contractor?) -> Route (10-99 staff + AI fit) -> Write to Audiences
```
| Route | Rule | Destination (visible) |
|---|---|---|
| Ready for review | 10 to 99 staff AND AI says yes | segment "ServiceTitan 2B - Ready for review" |
| Needs research | no staff count, OR 10-99 staff and AI says unclear | segment "ServiceTitan 2B - Needs research" |
| Not a fit | staff outside 10-99, OR AI says no | segment "ServiceTitan 2B - Not a fit" |
The rule is plain code, no AI, so every decision is explainable; the reason is written onto the record
("56 staff, service contractor: ..."). The write-back updates the same record by its Audience ID (never creates a duplicate) and
skips blanks.

## 5. Test and result
- Rule tested first on sample values (blank, text "10", 99.0, 100, yes / no / unclear): all routed correctly.
- **V1 (size only):** two 10-company tests, then published and run on all 49: Ready 33, Needs research 2, Not a fit 14.
  That showed size alone passes non-trade firms, so the AI step was added.
- **V2 (size + AI fit):** tested on 10 companies (6 of 8 size-passes correctly dropped: LB Capital, Acme Lock and Door,
  ControlTouch, Bestmark, Koniag, ECC), then published and run on all 49: **49 done, 0 failed.**
- **Final: Ready for review 14 · Needs research 8 · Not a fit 27.** AI answers: 21 yes, 20 no, 8 unclear.
  Examples: O'Connor Plumbing 56 staff, AI yes ("provides dependable plumbing...") -> Ready for review; LB Capital 41
  staff, AI no ("Driving Growth for Roofing Businesses") -> Not a fit; Bradham Brothers 15 staff, AI unclear ->
  Needs research.
- **Cost (Clay run log):** 167.2 credits, 158 runs, 0 failed. V1: tests 20 + full run 49. V2: four 10-company tests 44.2
  + full run 54 (about 1.1 per company: staff count + AI). Loading companies and the segments were free.

## What a reviewer should know
- "Needs research" holds 6 companies at 10-99 staff where the AI could not tell from a short description (e.g. Oak
  Electric, Infinity Technical Services, which Week 2's website check confirmed as service contractors). A website-read
  step on "unclear" rows, as in Week 2, is the next improvement.
- The workflow is live: any company that later enters the segment is enriched and routed automatically (about 1.3 credits each).
- Next (Week 4): "Ready for review" companies flow on to people search and outreach, or into HubSpot through the
  Lead channel value "Outbound - office under pressure".

## Evidence
`screenshots/1-workflow-graph.png` (the 5 steps) · `screenshots/2-workflow-runs.png` (all runs, 0 failed) ·
`screenshots/3-audiences.png`
