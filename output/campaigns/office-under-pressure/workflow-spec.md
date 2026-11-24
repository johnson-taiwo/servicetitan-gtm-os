# Workflow Spec: Office Under Pressure (contractors that are hiring)

**Status:** STRUCTURE-REVISED 2026-11-24 (Johnson Taiwo: trigger widened from office roles only to any job posted in
the last 30 days, after the Week 2 Clay check showed 1 office post in 30 days across 3,221 ICP companies; the role
being hired now routes) · copy revised for the new cells, review pending · C1, C3, C4, C5, C6 launch; C2 holds for a
dispatch proof point · not sendable until a sender and suppression lists exist

Signal matrix reference: G1 + G2 / W1 + W2 in `context/icp/signal-matrix.md` (18/20 and 17/20), merged into one
workflow because Clay's job data is thin for trade contractors (see "Week 2 Clay check" below). G4 stays a context
clause. Method: `knowledge-base/playbooks/workflow-anatomy.md`, `signal-matrix-framework.md`.
Source tags: co company.md · cu customers.md · cmp competitors.md · mk market.md · sg signals.md · pr practitioner.md
(all in `context/docs/research/`). **INFERRED** = our reading, not sourced.

### Week 2 Clay check (24 Nov 2026, free Clay company searches, US HQ, 11-200 staff)
| Filter | Companies |
|---|---|
| Industry "Specialty Trade Contractors" or "Building Equipment Contractors", no signal | 3,221 |
| + office role (CSR, dispatcher, office manager) posted in the last 30 days (trade code 238220/238210) | 1 |
| + office role open now, any post date (industry) | 9 |
| + any job posted in the last 30 days (industry) | **58** (chosen) |
| + office person who started in the last 6 months (industry) | 16 |

Clay sees job posts for about 2% of these companies. Trade contractors hire on Indeed and Facebook and field crews
skip LinkedIn (pr §1), so a narrow office-only trigger can't fill a list from Clay alone. Also: Clay's size band and
its own headcount estimate disagree for 6 of 42 companies, so size is checked per row, never taken from the band.

---

## 1. Mother Intent (Trigger) [ENGINE]

**US trade contractors (11-200 staff on Clay's band) that posted any job in the last 30 days.**

- Catalog match: ⭐ "Hiring specific roles" / "Companies in hypergrowth (hiring velocity)" (`workflow-anatomy.md`).
- Signal-led: the job post fires the send. What role they are hiring routes the story.
- Why: ServiceTitan's strongest proof is about the office (calls, dispatch, billing), and its pricing and growth follow
  technician count (sg §1 #3, #4). Hiring for either side is the moment the system is under load.

## 2. Inclusion / Exclusion [ICP]

### Include

**ICP box**
| Filter | Value (typed into Clay) | Source |
|---|---|---|
| Country | `locations.any(is_headquarters = true and country_name = "United States")` | icp.md Layer 1 |
| Industry | `industry in ("Specialty Trade Contractors", "Building Equipment Contractors")`, then the AI fit column keeps only HVAC, plumbing, electrical, garage door | icp.md S1; trade code 238220/238210 returned 1 row, too narrow |
| Size (search) | `company_size in ("11-50", "51-200")` | Clay band, pre-filter only |
| Size (gate) | 10 to 99 employees: Clay estimated employee count, confirmed by Clay "Enrich Company" headcount when they disagree | icp.md Layer 1; pr §1 |

**Trigger definition:** `jobs.exists(job_posted_date >= today() - interval 30 days)` on the company search.

**Recipient:** the owner (economic buyer, `personas.md` §2); the operations or general manager as a second thread when
found (`personas.md` §1).

### Exclude
| Who | Why | How matched |
|---|---|---|
| Job post asks for ServiceTitan experience | Probably already a customer (INFERRED) | Separate search: same filters + `job_description contains "ServiceTitan"`; lookup by domain |
| Not a service trade (painting, concrete, landscaping, roofing goes to W3) | Outside this workflow's trades | AI fit column |
| Staffing and recruiting firms | Post jobs for clients | Industry filter already excludes; AI fit column backstop |
| Under 10 or over 99 employees | Outside the ICP size | Size gate |
| Existing ServiceTitan customers named in research | Customers | Exclusions table (cu §2 names + domains) |
| Contacted in the last 90 days | No double-touching | `{{crm_suppression_list}}` (not public) |

## 3. Sources [ENGINE]

| Purpose | Source | Tool | Status |
|---|---|---|---|
| Trigger: companies with a job posted in 30 days | Clay companies dataset, job postings | Clay Find Companies (free search) with the filters above | Tested 24 Nov: 58 companies |
| Which roles they posted (router) | Same, one search per role family, joined on domain | Clay Find Companies (free) x 3: office roles, technician roles, any role | To run in Week 2 |
| Size gate | Clay estimated employee count; Clay "Enrich Company" when unsure | Clay (enrichment is paid) | Test on 1 row first |
| Business type and trade (fit) | Company website and description | Clay Use AI column, JSON + evidence | Prove on 3 rows first |
| Owner / GM | LinkedIn people data | Clay Find People at company (free search) | To run |
| Work email | Email waterfall | Clay "Find work email", strict validation | Paid; ready rows only |

## 4. Signals table [ENGINE]

| # | Signal | Role | How it's read | Output |
|---|---|---|---|---|
| 1 | Any job posted in the last 30 days | trigger | Company is in the trigger search result | `is_hiring` (true) |
| 2 | Hiring office staff | router | Company is also in the office search: `jobs.exists(job_posted_date >= today() - interval 30 days and job_title is_similar_to ("Customer Service Representative", "Dispatcher", "Office Manager"))` | `hiring_office` (true/false) |
| 3 | Which office role | router (inside office cells) | Keyword match on the office post's title: Dispatcher / Scheduler / Coordinator = `dispatch`; Customer Service / CSR / Call / Booking = `phones`; Office Manager / Administrator = `office` | `role_family` |
| 4 | Hiring field technicians | router | Company is in the technician search: same date filter + `job_title is_similar_to ("HVAC Technician", "Plumber", "Electrician", "Service Technician", "Installer")` | `hiring_techs` (true/false) |
| 5 | Starter app named in a post | context-clause (never routes) | Search with `job_description contains` "Housecall Pro" / "Jobber" / "Service Fusion" | `current_tool` |
| 6 | Trade | context-clause | AI fit column: HVAC / plumbing / electrical / garage door / multi-trade | `trade` |
| 7 | City | context-clause | Company HQ city from the search result | `city` |

Clusters: `hiring_office` x `hiring_techs` gives office only / field only / both / other roles only.

## 5. Routing matrix with angles [ENGINE]

Fires in every cell: `is_hiring` = true, ICP box passed, size gate passed.

What every cell assumes (INFERRED): a contractor paying to add people has more calls, jobs or invoices than the current
team handles.

| Cell | Deterministic read | What it tells us (inferred situation) | The angle it earns |
|---|---|---|---|
| **C1. Phones, office only** | `hiring_office` AND `role_family` = phones AND NOT `hiring_techs` | Call volume has grown past the people answering it | Calls answered and booked, after hours too, without a hire for each one. Proof: Brothers Plumbing, abandon rate near 30% to 5% (cu §2) |
| **C2. Dispatch, office only** | `hiring_office` AND `role_family` = dispatch AND NOT `hiring_techs` | Scheduling has become a full-time job | One dispatch board instead of a person juggling the whiteboard. Proof: `{{dispatch_proof}}` (none in research) |
| **C3. Office manager, office only** | `hiring_office` AND `role_family` = office AND NOT `hiring_techs` | The back office needs a dedicated person | Invoices and payroll out of spreadsheets. Proof: Adams Power invoices in 1.5 to 2 weeks vs up to 4 (cu §3) |
| **C4. Field only** | `hiring_techs` AND NOT `hiring_office` | Adding trucks while the office stays the same size | Every new technician needs booked calls and a schedule. Proof: Young Plumbing, 4 to 8 trucks in a year (cu §2) |
| **C5. Office and field** | `hiring_office` AND `hiring_techs` | Growing on both sides at once | The phones decide whether new trucks are full. Proof: Vines, 400 to 500 abandoned calls a day to slim to none, 70+ trucks (cu §2) |
| **C6. Other roles** | `is_hiring` AND NOT `hiring_office` AND NOT `hiring_techs` (e.g. sales, estimator, apprentice) | Growing, role not office or field | More booked calls and higher close rates from the same team. Proof: Redding, booking rate +30%, close rate +40% (cu §2) |

### Lines that never route
| Variable | When present | When empty |
|---|---|---|
| `current_tool` | One line can name the tool a post asks for; never criticise it | No tool line |
| `trade` | Picks the trade word | n/a |
| `city` | Local colour | Leave out |

**Branch coverage:** every cell has first name, company name, trade, city, the hiring flags. `role_posted` exists only in
cells with an office post (C1-C3, C5); C4 and C6 copy must not use it.

**Niche-native test:** each angle ties the type of hire to the part of the business it shows is stretched.

### Which cells can launch
| Cells | Status | What unblocks them |
|---|---|---|
| C1, C3, C4, C5 | Launch | Sender and suppression lists |
| C6 | Launch, watch | Weakest signal; check reply rate after the first 200 sends |
| C2 | Hold | A dispatch-specific proof point |

## 6. Variables contract [ENGINE]

| Variable | Source field | Exists in branches |
|---|---|---|
| `{{first_name}}` | Owner or GM first name (Clay Find People), cleaned | All |
| `{{company_name}}` | Company name, legal suffix stripped, short form | All |
| `{{role_posted}}` | Office post title cleaned to plain words | C1, C2, C3, C5 only |
| `{{trade}}` | AI fit column | All |
| `{{city}}` | Company HQ city | Most; copy must work without it |
| `{{current_tool}}` | Signal 5 | Some; never required |
| `{{social_proof}}` | Static per cell (section 5) | All (C2 unfilled) |
| `{{sender_name}}`, `{{sender_title}}`, `{{cta}}` | Not set (study build) | All (unfilled) |

Never available to copy: a raw employee count, the number of open jobs, any claim they are missing calls or money,
prices or contract terms.

## 7. Cadence & channel [ENGINE]

- **Periodicity:** always-on, run weekly. Job posts are the highest-velocity signal (`workflow-anatomy.md`
  Periodicity Matrix). Send only while the post is 30 days old or less.
- **Dedupe:** one entry per company (key: domain) per 90 days; the owner is the contact.
- **Channel:** email, 3 emails over 10 days in one thread; LinkedIn connection to the GM when found.
- **Send gates:** C1, C3, C4, C5, C6 send; C2 collects in its own view until a proof point exists.

---

## Definition of done
- [x] Mother intent: one sentence, signal-led, matched to the catalog
- [x] Inclusion: ICP box + trigger definition + recipient, tagged [ICP]
- [x] Exclusion: who's out and why
- [x] Sources: trigger + every enrichment
- [x] Signals table: every read deterministic, every role declared
- [x] Clusters declared (office x field: office only / field only / both / other)
- [x] Routing matrix with every angle filled
- [x] Branch coverage checked
- [x] Variables contract: every variable mapped to its source field
- [x] Engine/ICP markers on every section
- [x] Cadence & channel declared

## Open items
1. Done 24 Nov: Clay's job search is free and covers about 2% of small trade contractors (58 of 3,221 with a post in 30 days); trigger widened.
2. A dispatch proof point (unblocks C3).
3. Facts file, sender, CTA and suppression lists before any copy is sent (`/campaign-copy` builds the facts file).

---

## Copy

*Written 2026-11-23 with `/campaign-copy` rules (`knowledge-base/playbooks/copy-iron-laws.md`, `copy-bans.yaml`).
Register: SIGNAL play. Every name and number traces to `context/messaging/facts.md`. Variables only from section 6.
Dynamic lines resolve in Clay (one clean sentence per row, no spintax); static lines carry spintax. Routing is
exhaustive (6 cells cover every row), so no fallback is needed; C3 stays on hold until `{{dispatch_proof}}` exists.*

Line map (E1): L1 the role they posted · L2 technician hiring · L3 value prop · L4 matched proof · L5 CTA.

### Email 1 (day 0)

**Subject:** {new office hire|the office hire|adding to the office}

```
{{first_name}},

{{e1l1}}

{{e1l2}}

ServiceTitan {runs|keeps} calls, dispatch, invoices and payroll in one system built for the trades, and its AI agent {answers|picks up} overflow and after-hours calls and books them onto the schedule.

{{social_proof}}

{Worth a look before the new hire starts?|Worth comparing before the new hire starts?}

{{sender_name}}
```

**{{e1l1}}** (router: cell)
| Route | Line |
|---|---|
| C1 phones | The {{role_posted}} opening at {{company_name}} usually means call volume has grown past the people answering it. |
| C2 dispatch | Bringing on a {{role_posted}} usually means scheduling at {{company_name}} has become a full-time job. |
| C3 office manager | The {{role_posted}} opening at {{company_name}} usually means the billing and paperwork now need someone full time. |
| C4 field only | Adding technicians at {{company_name}} usually means more trucks to keep busy every morning. |
| C5 office and field | Hiring for the office and the field at once usually means {{company_name}} is growing on both sides. |
| C6 other roles | A new opening at {{company_name}} usually means the team is growing. |

**{{e1l2}}** (router: cell)
| Route | Line |
|---|---|
| C1, C2, C3 | Could be off, but my read is the field team is holding steady while the office workload grows. |
| C4 | My guess is the office will feel every one of those new trucks. |
| C5 | With a {{role_posted}} and technicians both open, my guess is the calls and the schedule are where it gets tight. |
| C6 | Could be off, but growth usually shows up first in the calls that need booking. |

**{{social_proof}}** (one per cell, from facts.md)
| Cell | Line |
|---|---|
| C1 phones, office only | Brothers Plumbing cut their call abandon rate from near 30% to 5% on it. |
| C2 dispatch, office only | {{dispatch_proof}} (cell on hold) |
| C3 office manager, office only | Adams Power now gets service invoices out in a week and a half to two weeks, versus up to four before. |
| C4 field only | Young Plumbing went from 4 to 8 trucks in a year on it. |
| C5 office and field | Vines Restoration went from 400 to 500 abandoned calls a day to slim to none, with 70+ trucks on the road. |
| C6 other roles | Redding Heating & Air saw their call booking rate rise 30% and their close rate 40% after turning on Max. |

QA: passed. insult ok, nexus ok, facts-traced ok, spintax ok (static only), no-cliche ok, no banned words [word count E1: 85 to 99 depending on route, counted with a sample name; under the 120 ceiling, above the 60 optimum: the value line is the one to cut if replies are low]

### Email 2 (day 3, same thread, subject "Re: <Email 1 subject>")

```
{{first_name}},

Once {the new hire|the new person} is in, the next question is usually what the owner can see from the office.

On ServiceTitan, booked calls, close rates and job costs {show up as they happen|are live}, not at month end.

{{e2_proof}}

{Would a short example from a shop your size help?|Useful to see a short example from a shop your size?}
```

**{{e2_proof}}**: C6 = Young Plumbing grew revenue 62% in a year on it. All other cells = Redding Heating & Air saw their call booking rate rise 30% and their close rate 40% after turning on Max.

QA: passed. adds new value (owner view, close rate) not in E1, no repeated lines across the sequence for any cell, subject unchanged [word count: 68]

### Email 3 (day 10, same thread)

```
{{first_name}},

If someone else at {{company_name}} {looks after|handles} the software for the office, who would that be?

Happy to send them the Brothers Plumbing call numbers instead.
```

QA: passed. referral CTA from the ladder, new ask, no PS [word count: 28]

### LinkedIn (to the operations or general manager, when found)

Connection request: empty (no note).

**DM 1** (router: cell: phones = C1, dispatch = C2, office = C3 and C5, field only = C4, other roles = C6)
| Route | Message |
|---|---|
| phones | {{first_name}}, with {{company_name}} adding to the phone team, is the extra call volume going to people, software, or both? |
| dispatch | {{first_name}}, with a {{role_posted}} joining {{company_name}}, is scheduling run on a board today or already in software? |
| office | {{first_name}}, as {{company_name}} adds an office lead, is billing handled in one system yet or spread across a few? |
| field only | {{first_name}}, with {{company_name}} adding technicians, is dispatch keeping up or is that the next thing to sort? |
| other roles | {{first_name}}, with {{company_name}} growing the team, are new calls booked by people, software, or both? |

**DM 2** (static)
```
No rush on that. I work with {trade contractors|contractors} who are hiring, and the Brothers Plumbing call numbers {usually|tend to} start a good conversation. Happy to send them over.
```

### Persona review and human test (read as the owner of a 25-person HVAC shop that just posted a CSR job; re-read 24 Nov for C4 to C6 as an owner hiring two technicians)

| Check | Finding | Change made |
|---|---|---|
| Relevance | L1 names the exact role they posted, so it reads as written to them | None |
| Credibility | Proof is a named contractor with a number, matched to the role | None |
| Insult test | "grown past the people answering it" could read as "you're missing calls" | Kept "usually means" + growth framing, no mention of missed calls; the phones proof talks about another company's numbers, not theirs |
| CTA friction | "Worth a look before the new hire starts?" is a yes/no tied to their timing | None |
| Sounds like a person? | E1 value line was long | Cut it to one sentence; E3 kept to two lines |
| Banned words | Checked against copy-bans.yaml: none of "saw your", "noticed", "free", "book a call", em dashes | None needed |

**Status of copy:** written and self-reviewed against the QA gate and the persona review; COPY-APPROVED (see Status). Sendable only once
`{{sender_name}}`, `{{dispatch_proof}}` and suppression lists exist (see Open items).
