# Workflow Spec: Office Under Pressure (hiring office staff)

**Status:** STRUCTURE-APPROVED 2026-11-23 (Johnson Taiwo; approval given up front for an end-to-end build, open to
review) · copy pending · C1 to C4 launch; C5, C6 hold (see Which cells can launch) · not sendable until a facts file,
a sender and suppression lists exist

Signal matrix reference: G1 / W1 in `context/icp/signal-matrix.md` (18/20), with G2 as a router and G4 as a context
clause. Method: `knowledge-base/playbooks/workflow-anatomy.md`, `signal-matrix-framework.md`.
Source tags: co company.md · cu customers.md · cmp competitors.md · mk market.md · sg signals.md · pr practitioner.md
(all in `context/docs/research/`). **INFERRED** = our reading, not sourced.

---

## 1. Mother Intent (Trigger) [ENGINE]

**Trade contractors in the US that posted a job for a customer service rep, call-center agent, dispatcher or office
manager in the last 30 days.**

- Catalog match: ⭐ "Hiring specific roles", Hiring & Team Composition (`workflow-anatomy.md`).
- Signal-led: the job post fires the send. Trade and size only filter; the role and technician hiring route the story.
- Why this signal: ServiceTitan's strongest proof is about the office, not the field: call abandon rate "near 30%" cut
  to 5% (Brothers Plumbing, cu §2), abandoned calls "400-500/day" to "slim to none" (Vines, cu §2), invoices out in
  "a week and a half to two weeks" instead of "four weeks" (Adams Power, cu §3). A company hiring for the office is
  paying to solve the problem the product solves.

## 2. Inclusion / Exclusion [ICP]

### Include

**ICP box**
| Filter | Value | Source |
|---|---|---|
| Country | United States (Canada in a later run) | icp.md Layer 1 |
| Trade | HVAC, plumbing, electrical, garage door; roofing goes to the storm workflow (W3) | icp.md Layer 1, S1 |
| Employees | 10 to 99, proven from an exact source (LinkedIn page count, PPP jobs, Form 5500, FMCSA, OSHA; Clay company enrichment last) | icp.md Layer 1; pr §1, §2 |
| Type | Residential or mixed residential and commercial service business (AI fit column) | icp.md S1 |

**Trigger definition**
- Job title contains any of: Customer Service Representative, CSR, Call Center, Call Centre, Customer Care, Dispatcher,
  Dispatch Coordinator, Service Coordinator, Office Manager, Office Administrator, Scheduler, Booking.
- Job title contains none of: Driver, Technician, Installer, Sales, Recruiter, Insurance, Bank (keeps the role an
  office role at a contractor; INFERRED keyword list).
- Posted 0 to 30 days before the run date.
- Hiring company is the contractor itself, not a staffing agency posting for a client.

**Recipient:** the owner (economic buyer, `personas.md` §2). The operations or general manager is a second thread when
found (`personas.md` §1). The office manager is not emailed: they are often the role being hired or backfilled.

### Exclude
| Who | Why | How matched |
|---|---|---|
| Job posts that ask for ServiceTitan experience | The company most likely already runs ServiceTitan (INFERRED) | Job description contains "ServiceTitan" or "Service Titan" |
| Staffing and recruiting agencies | They post office roles for clients (INFERRED) | Company industry = Staffing and Recruiting, or name contains "Staffing", "Personnel", "Recruit" |
| Franchise head offices | Bought centrally; segment S2 | AI fit column: franchisor = true |
| Suppliers, distributors, manufacturers | Partners, not users (sg §1 #8) | AI fit column: business type not in the allowed list |
| Under 10 employees, or size not provable | Below the targeting floor; unproven size is held, not mailed (icp.md Not a fit) | Size proof column |
| Existing ServiceTitan customers named in research | Customers | Domain blocklist from cu §2, plus `{{customer_suppression_list}}` (not public) |
| Contacted in the last 90 days, or in active pipeline | No double-touching | `{{crm_suppression_list}}` (not public) |

## 3. Sources [ENGINE]

| Purpose | Source | Tool | Status |
|---|---|---|---|
| Trigger: office job posts, last 30 days | PredictLeads, Google Jobs and Clay's own jobs data | Clay "Find Jobs" (source), keywords from the trigger definition | Exists (sg §2d #31). Whether the search is free and how well it covers small trade shops: test in Week 2 (sg Gaps) |
| Technician hiring count (router) | Same | Clay "Find Jobs" at the company domain | Exists; same coverage caveat |
| Company detail: domain, industry, HQ, LinkedIn page | Company website, LinkedIn | Clay "Enrich Company" (paid, last resort) or the free company file (pr §1) | Tested in practice (pr §1) |
| Size proof | LinkedIn page count (free); SBA PPP jobs, Form 5500, FMCSA, OSHA (free bulk files) | Free LinkedIn counter; offline match on phone or name + state | Tested in practice (pr §1, §2) |
| Business type and trade (fit) | Company website text | Clay Use AI column, JSON with an evidence field | To build in Week 2 |
| Owner / GM name | LinkedIn, company team page | Clay "Find People" at the company domain (free search) | Exists |
| Work email | Email waterfall | Clay "Find work email", strict validation | Exists; cost per row to be measured |

**Before the build:** run the Find Jobs search with the trigger keywords and read the count before importing (free if
the Week 2 test confirms; if it's paid, ask first).

## 4. Signals table [ENGINE]

| # | Signal | Role | How it's read | Output |
|---|---|---|---|---|
| 1 | Office role posted in the last 30 days | trigger | Job title matches the include list AND not the exclude list (keyword match) AND posted date is 30 days or fewer before the run | `is_office_hire` (true/false) |
| 2 | Which office role | router | Keyword match on the job title, first match wins in this order: Dispatcher / Dispatch / Scheduler / Service Coordinator = `dispatch`; Customer Service / CSR / Call Center / Call Centre / Customer Care / Booking = `phones`; Office Manager / Office Administrator = `office` | `role_family`: phones / dispatch / office |
| 3 | Also hiring field technicians | router | Count of open jobs at the same company whose title contains Technician, Tech, Installer, Plumber, Electrician, Apprentice, posted in the last 30 days. Clusters: 0 = office only; 1 or more = growing on both sides | `hiring_techs` (true/false) |
| 4 | Starter app named in the job post | context-clause (never routes) | Job description contains "Housecall Pro", "Jobber", "Service Fusion", "Kickserv", "FieldEdge" or "Successware" (keyword match) | `current_tool` (text or empty) |
| 5 | Trade | context-clause (never routes) | AI fit column, allowed values: HVAC, plumbing, electrical, garage door, multi-trade | `trade` |
| 6 | City | context-clause (never routes) | Job location city, else company HQ city | `city` |

Routing axes: role family (3) x hiring technicians (2) = **6 cells**.

Left out on purpose:
- Revenue: unreliable for trades and not needed to route (pr §1).
- Review count: a size hint only; Google's terms forbid storing it over time (signal matrix G9).
- Number of office roles open: one role is enough to fire; a count adds nothing to the angle.

## 5. Routing matrix with angles [ENGINE]

Fires in every cell: `is_office_hire` = true, ICP box passed, size proven 10 to 99.

What every cell assumes (INFERRED): a contractor adding an office role has more calls, jobs or invoices than the
current office handles, and has decided to pay a salary to handle them.

| Cell | Deterministic read | What it tells us (inferred situation) | The angle it earns |
|---|---|---|---|
| **C1. Phones, office only** | `role_family` = phones AND `hiring_techs` = false | Call volume has grown past the people answering it, while the field team is staying the same size | Answer and book every call without adding headcount for each one: AI virtual agent and online booking take overflow and after-hours calls. Proof: Brothers Plumbing, abandon rate "near 30%" to 5% (cu §2) |
| **C2. Phones, growing both sides** | `role_family` = phones AND `hiring_techs` = true | The company is adding technicians and the calls to keep them busy at the same time | Every new technician needs booked calls: the phones decide whether new trucks are full. Proof: Vines, abandoned calls "400-500/day" to "slim to none", 70+ trucks (cu §2) |
| **C3. Dispatch, office only** | `role_family` = dispatch AND `hiring_techs` = false | Scheduling has become a full-time job for the current crew size | One dispatch board that matches technicians to jobs and routes them, instead of a person juggling the whiteboard. Proof: `{{dispatch_proof}}` (no dispatch-specific result in research) |
| **C4. Dispatch, growing both sides** | `role_family` = dispatch AND `hiring_techs` = true | The company is adding technicians and someone to schedule them: the crew is outgrowing the way jobs are assigned today | Add trucks without the schedule breaking: dispatch, scheduling and technician data in one place. Proof: Young Plumbing, 4 to 8 trucks in one year (cu §2) |
| **C5. Office manager, office only** | `role_family` = office AND `hiring_techs` = false | The back office (billing, payroll, paperwork) needs a dedicated person | Invoices and payroll out of the spreadsheet. Proof: Adams Power, invoices in "a week and a half to two weeks" vs "four weeks" (cu §3); Crown, payroll team 3 to 1 (cu §2) |
| **C6. Office manager, growing both sides** | `role_family` = office AND `hiring_techs` = true | The company is growing in the field and adding the first or second office lead to run it | The office is being built now: build it on one system before it is built on spreadsheets. Proof: Young Plumbing, "I'm running a business" (cu §3) |

### Lines that never route
| Variable | When present | When empty |
|---|---|---|
| `current_tool` | One line can name the tool they're asking applicants to know, and that ServiceTitan's own comparison pages speak to shops with "ambitions to grow" beyond it (sg §1 #9). Never criticise the tool | No line about tools |
| `trade` | Picks the trade word ("HVAC calls", "plumbing calls") | n/a (every row passes the fit column) |
| `city` | Local colour ("in {{city}}") | Leave out |

**Branch coverage:** all 6 cells have first name, company name, role posted, role family, hiring technicians (true or
false), trade, city. No angle depends on `current_tool`. C3 has no research proof yet, so it uses a placeholder.

**Niche-native test:** each angle ties the exact role they posted to the part of the business it shows is stretched.
A general "grow your business" angle would fail this test; these could only be sent to a contractor hiring that role.

### Which cells can launch
| Cells | Status | What unblocks them |
|---|---|---|
| C1, C2, C4 | Launch in v1 | Nothing in structure. Facts file, sender and suppression lists still needed before any send |
| C3 | Hold | A dispatch-specific proof point (ask ServiceTitan or find one) |
| C5, C6 | Launch in v1, lower priority | Office manager posts may be backfills, not growth (INFERRED): watch reply rate in the first 200 sends |

## 6. Variables contract [ENGINE]

Copy may use only these. If a variable isn't listed, copy can't say it.

| Variable | Source field | Exists in branches |
|---|---|---|
| `{{first_name}}` | Owner or GM first name (Clay Find People), cleaned | All |
| `{{company_name}}` | Company name, legal suffix stripped (LLC, Inc), short form | All |
| `{{role_posted}}` | The job title they posted, cleaned to plain words (e.g. "customer service rep") | All |
| `{{role_family}}` | phones / dispatch / office (signal 2) | All |
| `{{hiring_techs}}` | true / false (signal 3); copy needs a version for each value | All |
| `{{trade}}` | HVAC / plumbing / electrical / garage door / multi-trade (signal 5) | All |
| `{{city}}` | Job or HQ city (signal 6) | Most; copy must work without it |
| `{{current_tool}}` | Tool named in the post (signal 4) | Some; never required |
| `{{proof_name}}`, `{{proof_result}}` | Static per cell from section 5 (C1 Brothers Plumbing, C2 Vines, C4 and C6 Young Plumbing, C5 Adams Power); C3 = `{{dispatch_proof}}` | All (C3 unfilled) |
| `{{sender_name}}`, `{{sender_title}}` | Not set (study build) | All (unfilled) |
| `{{cta}}` | Not set | All (unfilled) |

Never available to copy:
- A raw employee count or number of open jobs.
- Any claim that the company is missing calls or losing money (situation only, never a diagnosis).
- Prices or contract terms (not public; the top complaint area, cu §7).

## 7. Cadence & channel [ENGINE]

- **Periodicity:** always-on, run weekly. Job posts are the highest-velocity signal (`workflow-anatomy.md`
  Periodicity Matrix). Send only while the post is 30 days old or less.
- **Dedupe:** one entry per company (key: domain) per 90 days; the owner is the contact.
- **Channel:** email, 3 emails over 10 days in one thread; LinkedIn connection to the GM when found.
- **Send gates:** C1, C2, C4, C5, C6 send; C3 collects in its own view until a proof point exists.

---

## Definition of done
- [x] Mother intent: one sentence, signal-led, matched to the catalog
- [x] Inclusion: ICP box + trigger definition + recipient, tagged [ICP]
- [x] Exclusion: who's out and why
- [x] Sources: trigger + every enrichment
- [x] Signals table: every read deterministic, every role declared
- [x] Count clusters declared (signal 3: 0 / 1 or more)
- [x] Routing matrix with every angle filled
- [x] Branch coverage checked
- [x] Variables contract: every variable mapped to its source field
- [x] Engine/ICP markers on every section
- [x] Cadence & channel declared

## Open items
1. Confirm in Clay that the Find Jobs search is free and how many small trade shops it covers (Week 2, first step).
2. A dispatch proof point (unblocks C3).
3. Facts file, sender, CTA and suppression lists before any copy is sent (`/campaign-copy` builds the facts file).
