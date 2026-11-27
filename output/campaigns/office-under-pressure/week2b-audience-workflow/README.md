# Week 2B: Audience segment + Workflow (Segment, Enrich, Act)

**Status:** LIVE in Clay, 27 Nov 2026. Workflow: https://app.clay.com/workspaces/736170/workflows/tc-workflows/wf_0tp63p7jJYZwTEDMV7q

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

## 3. The enrichment (one, because it decides qualification)
**Enrich company** on the record's domain (identifier preserved from the trigger) returns the real staff count. Week 2
showed Clay's size band is wrong for about 1 in 5 small contractors, so the band only selects; the count decides.

## 4. The Workflow and the routing rule
```
Segment (record enters) -> Enrich company (staff count) -> Route (10-99 staff rule) -> Write to Audiences
```
| Route | Rule | Destination (visible) |
|---|---|---|
| Ready for review | 10 to 99 staff | segment "ServiceTitan 2B - Ready for review" |
| Needs research | no staff count found | segment "ServiceTitan 2B - Needs research" |
| Not a fit | staff count outside 10-99 | segment "ServiceTitan 2B - Not a fit" |
The rule is plain code, no AI, so every decision is explainable; the reason is written onto the record
("56 staff, inside 10-99"). The write-back updates the same record by its Audience ID (never creates a duplicate) and
skips blanks.

## 5. Test and result
- Rule tested first on 8 sample values (including blank, text "10", 99.0 and 100): all routed correctly.
- Two 10-company test runs, then the workflow was published and run on the whole segment.
- **49 of 49 runs done, 0 failed. Ready for review 33 · Needs research 2 · Not a fit 14.** Examples: O'Connor Plumbing
  56 staff -> Ready for review; Horsetooth Heating, Air & Plumbing 8 -> Not a fit; ECC 149 -> Not a fit; Electrify My
  Home, no count -> Needs research.
- **Cost:** 69 credits (Clay's run log): 20 for the two tests, 49 for the full run; loading and the segment were free.

## What a reviewer should know
- "Ready for review" means size and signal pass, not that the company is a service contractor. The Week 2 AI fit check
  is the second gate: it drops e.g. a capital firm and a trade-show company that pass on size alone. Adding that check as
  a second workflow step is the next improvement.
- The workflow is live: any company that later enters the segment is enriched and routed automatically (1 credit each).
- Next (Week 4): "Ready for review" companies flow on to people search and outreach, or into HubSpot through the
  Lead channel value "Outbound - office under pressure".

## Evidence
`screenshots/1-workflow-graph.png` (the 4 steps) · `screenshots/2-workflow-runs.png` (49 runs, 0 failed, 69 credits) ·
`screenshots/3-audiences.png`
