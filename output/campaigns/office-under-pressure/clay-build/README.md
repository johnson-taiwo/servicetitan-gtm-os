# Week 2 Clay build: Office Under Pressure

**Clay workbook (view access):** https://app.clay.com/workspaces/736170/workbooks/wb_0tp15rlsUJYKZbuDF4W
Workbook "ServiceTitan | Contractors Hiring | 24 Nov 2026", 9 tables: 01 Exclusions · 02 Hiring Contractors (main) ·
03-07 signal searches (phones, dispatch, office manager, technicians, "ServiceTitan" in a job post) · 08 Owners ·
09 Demo requests (Week 3: the HubSpot fit check).

| File | What it is |
|---|---|
| `office-under-pressure-clay-build-guide.md` | The build plan: 10 parts, each with why / click / type / check / cost |
| `office-under-pressure-clay-flow.png` (.html, .json) | Flow diagram: 4 lanes, 8 steps |

## Funnel (measured)
| Step | Rows | Cost |
|---|---|---|
| Free Clay company search: US HQ, specialty trade + building equipment contractors, 11-200 band | 3,221 companies | 0 |
| Posted any job in the last 30 days (imported) | 58 | 0 |
| Passed free checks (domain, not a likely ServiceTitan customer) | 49 | 0 (lookups and formulas) |
| Enrich Company: staff count (1 credit per row, 49 rows) | 27 at 10-99, 10 no count (held) | 49.0 |
| AI fit with evidence (0.8 per row; 3 test rows, 24, then 27 re-run after tightening the prompt) + website check on 5 unclear rows | 16 service contractors | 43.2 + 2.1 |
| Routing (6 cells) | 15 ready to contact, 1 held (dispatch, no proof yet) | 0 |
| Owners (free Find People from the Ready view) | 28 found, 20 kept by title | 0 |
| Email waterfall + validation | 15 verified on the company domain, 4 "not found", 1 held (other domain) | 75.5 |
| **Send-ready (email lines filled from the spec)** | **15 people at 9 companies** | **169.8 total, about 11 per person** |

Credits are Clay data credits, read from the workspace balance and the usage report. Actions were not recorded
separately (the Treford plan includes 50,000).

## Known limits
- The email waterfall's first step ran on all 28 owners before its run condition was set (the 8 dropped owners cost
  credits); the condition now limits it to kept owners.
- The exclusions table has no customer domains (none are public); the working customer filter is the "ServiceTitan
  in a job post" search, which dropped 7 companies.
