# CRM design: ServiceTitan demo request to MQL to deal (Week 3)

**Status:** BUILT AND TESTED 2026-11-27 in Johnson's own HubSpot trial portal (both robots ON). Rules drafted by Claude from
the Week 1-2 rules; Johnson to review the wording. How each piece was built, click by click: `build-guide.md`.
Filled in from Treford's CRM Design Workbook (Part 1, steps 1.1 to 1.6). Every rule reuses a Week 1 or Week 2 rule
(source named), so the CRM judges a lead exactly the way the outbound list was judged.
**Teaching case:** ServiceTitan is a public company and this is a portfolio build, not ServiceTitan's real CRM.
Deal amounts and win % are illustrative.

---

## 1.1 The motion
| Decision | Answer | Source |
|---|---|---|
| Company or product | ServiceTitan: one system for a trade contractor's calls, dispatch, invoices and payroll, with an AI agent that books overflow calls | `context/positioning/value-proposition.md` |
| Target account | US head office, 10 to 99 employees, a service trade (HVAC, plumbing, electrical, garage door and similar). Not a staffing firm, not already a ServiceTitan customer | `context/icp/icp.md` Layer 1, S1; Week 2 spec §2 |
| Relevant buyer | The owner (economic buyer), or the operations / general manager (feels the pain daily) | `context/icp/personas.md` §1-2 |
| Qualifying event | **Inbound:** a demo request (form). **Outbound** (Week 2): a positive reply to the "office under pressure" emails | Week 2 spec §1 |
| What makes a lead qualified | **Fit** (who they are, checked by Clay) AND **intent** (what they did: asked for a demo, or replied yes). Fit alone is a target; intent alone is noise | Workbook 1.1: "keep fit separate from intent" |
| Handoff | The contact owner gets a call task due in 1 working day and an alert that says why they fit | Workbook 2.2 |

### Fit and intent, kept apart
| | High intent (asked for a demo / replied yes) | No intent yet |
|---|---|---|
| **Meets criteria** | **MQL** -> owner + call task | Outbound target (Week 2 list) |
| **Needs review** | A person decides (segment "Needs review") | Ignore until more evidence |
| **Doesn't meet criteria** | Lead, Unqualified, reason saved | Never contacted |

## The fit rule (Clay writes the answer; plain formulas, no AI guess)
Checked in this order. The first rule that fires sets the answer and the reason.
| # | Check | Answer | ICP reason written |
|---|---|---|---|
| 1 | Email is personal (gmail, yahoo, hotmail, outlook, icloud, aol, proton) | Needs review | "Personal email: no company to check" |
| 2 | Clay found no company for the domain | Needs review | "No company data for <domain>" |
| 3 | Domain is on the exclusions list (known ServiceTitan customers) | Doesn't meet criteria | "Already a ServiceTitan customer" |
| 4 | Head office not in the United States | Doesn't meet criteria | "HQ <country>, not US" |
| 5 | Employee count missing | Needs review | "No staff count" |
| 6 | Employee count under 10 or over 99 | Doesn't meet criteria | "<n> staff, outside 10-99" |
| 7 | Industry is not a service trade (staffing, recruiting, software, retail, and anything without trade words) | Doesn't meet criteria | "Industry <industry>, not a service trade" |
| 8 | Everything passed | Meets criteria | "US HQ, <n> staff, <industry>" |

*Why plain formulas:* the reason is always readable, costs nothing, and a teammate can apply the same rule by hand
(Johnson's build pattern: deterministic before AI). Week 2 used an AI column for fit because outbound lists are messy.
Inbound has the email domain, so formulas are enough.

---

## 1.2 Lifecycle stages
HubSpot's default stages. Automatic updates only move a stage forward, so a Customer is never demoted by a new form.
| Stage | Evidence to enter | Who owns it | Next action |
|---|---|---|---|
| Lead | Arrives through the demo form, Clay (outbound list) or an import | Marketing (unowned) | Clay checks fit |
| MQL | Fit = Meets criteria AND intent (demo request or a positive reply) | Named SDR (contact owner) | Call within 1 working day |
| SQL | Sales confirms on the call: a real office pain, the decision maker in the room, a next step agreed | Sales (set by hand) | Accept the opportunity |
| Opportunity | A deal is created in the ServiceTitan pipeline (robot 2) | Deal owner | Work the pipeline stages |
| Customer | Deal Closed won | Account manager | Onboarding handoff |

## 1.3 Pipeline "ServiceTitan" (buyer-led, USD)
Stage names say what the BUYER did.
| Deal stage | Win % | Buyer evidence to enter | Must know by now | Next buyer action |
|---|---|---|---|---|
| Discovery held | 20 | Talked through their office pain (calls, dispatch or billing) | Primary pain, technician count | Agree to an operations review |
| Operations review | 40 | Walked us through their day: trucks, techs, current tools, who decides | Current software, decision maker, timeline | Book a demo on their own workflow |
| Demo and proposal | 60 | Saw ServiceTitan on their own jobs; proposal sent | **Amount (required)** | Review pricing with the decider |
| Contracting | 80 | Agreed terms; contract out for signature | Expected close date | Sign |
| Closed won | 100 | Signed | Close date, amount | Onboarding |
| Closed lost | 0 | Said no, or the quiet-deal policy ran out | **Lost reason (required)** | Revisit date |

**Quiet-deal policy:** silence is not a loss on its own. After 3 follow-ups over 21 days with no reply, close as
lost with reason "Went quiet". Revisit in 90 days, or sooner if a new hiring signal fires (Week 2 trigger).
**Lost reasons (dropdown):** Price · Staying on current tool · Timing · No decision maker · Went quiet · Not a fit.

### Deal amount (illustrative, logic shown)
ServiceTitan prices "based on the number of field technicians" (10-K, `context/docs/research/company.md` §3). No
public per-technician price exists. Competitor-published estimates are $245 to $500 per technician per month
(UNVERIFIED). Illustrative value = estimated technicians × $300 × 12. Technicians ≈ 60% of staff (INFERRED: the rest
are office and management).
| Number of employees (company) | Branch | Est. technicians | Deal amount (yearly) |
|---|---|---|---|
| 10 to 24 | Small | ~10 | $36,000 |
| 25 to 49 | Mid | ~22 | $80,000 |
| 50 to 99 | Large | ~45 | $160,000 |
| Unknown or outside | None met | n/a | No amount + task "Confirm technician count" (never guess) |

## 1.4 Properties (only the ones a rule or report uses)
| Property (object) | Type and allowed values | Who writes it | Used for |
|---|---|---|---|
| ICP fit (contact, `icp_fit`) | Dropdown, exactly: `Meets criteria`, `Doesn't meet criteria`, `Needs review` | Clay only | Robot 1 branches; segment; report |
| ICP reason (contact, `icp_reason`) | Single-line text | Clay only | Owner alert; audit |
| Lead channel (contact, `lead_channel`) | Dropdown: `Demo request`, `Outbound - office under pressure` | Form (hidden field) / Clay | Report: which door works |
| MQL date (contact, `mql_date`, reused from the June build) | Date | Robot 1 | Speed-to-lead report |
| Number of employees (company) | Number (HubSpot default) | Clay (company update) | Robot 2 amount |
| Lifecycle stage, Contact owner, Lead status (contact) | HubSpot defaults | Robots / sales | Stage, ownership |
| Lost reason (deal, `st_lost_reason`) | Dropdown (list above) | Sales | Loss report |
**Rule:** each fact sits on the right object. ICP fit on the contact summarises company facts, and the reason says which.

## 1.5 The workflows in plain words
### Robot 1: Demo request -> MQL (contact-based)
| Part | Answer |
|---|---|
| Trigger | Filled in "ServiceTitan demo request" AND lifecycle is Lead (Customers and later stages never enter) |
| Evidence | ICP fit from Clay (sent by webhook with record ID + email) |
| Wait | Until ICP fit is known, up to 15 minutes |
| Meets criteria | Lifecycle -> MQL; MQL date = today; owner assigned if empty (never overwritten); call task "Call <first name> about their ServiceTitan demo" due in 1 working day, assigned to owner; internal alert "New MQL: <name>, <company>. Why: <ICP reason>" |
| Doesn't meet | Lead status = Unqualified; stays Lead; reason saved |
| Needs review | No action: segment "Needs review" catches it; a person decides |
| **Failure path (timed out)** | Clay never answered: ICP fit = Needs review is NOT guessed; a task "Clay check didn't come back for <email>" goes to the owner, so nothing fails silently |
| Re-enrolment | Off. A second demo request doesn't re-run the robot or create a duplicate task |
### Robot 2: SQL -> deal (contact-based)
| Part | Answer |
|---|---|
| Trigger | Lifecycle becomes SQL (set by sales by hand) |
| Guard | Skip if the contact already has an open deal (no duplicate deals) |
| Branch | Associated company's Number of employees: 10-24 / 25-49 / 50-99 / none met |
| Action | Create deal "<Company> - ServiceTitan", pipeline ServiceTitan, stage Discovery held, amount per band, owner = contact owner, associated with contact + company. None met: no amount + task "Confirm technician count" |
| Then | Lifecycle -> Opportunity |
| Re-enrolment | Off |
### Existing customer asks for a demo
Robot 1 doesn't enrol them (trigger requires Lead). The segment "Customers who requested a demo" plus a task to the
account manager: probably an add-on or a new location, not a new logo.

## 1.6 Tests (run 27 Nov 2026 through the real form, fake people at real company domains)
| # | Test | Expected | Actual | Evidence |
|---|---|---|---|---|
| 1 | Felix, work email at O'Connor Plumbing (US, 56 staff, plumbing) | Meets; MQL; owner; call task due next working day; alert | ICP fit **Meets criteria**, reason "US HQ, 56 staff, Specialty Trade Contractors"; lifecycle **MQL** (date stamped by HubSpot); owner **Johnson Taiwo**; task **"Call Felix about their ServiceTitan demo"**, type Call, high priority, due **Mon 30 Nov 9:00** (Friday + 1 working day); alert sent | `screenshots/test1-felix-contact-record.png` |
| 2 | Nora, work email at Salesforce (91,443 staff) | Doesn't meet; Lead; Unqualified; reason saved | ICP fit **Doesn't meet criteria**, reason "91443 staff, outside 10-99"; stays **Lead**; lead status **Unqualified**; no owner, no task | HubSpot record |
| 3 | Gina, Gmail address | Needs review; Lead; in the review list; no task | ICP fit **Needs review**, reason "Personal email: no company to check"; stays **Lead**; in list "Needs review (ServiceTitan)" | HubSpot list |
| 4 | Casey, an existing **Customer** at Frey Lutz, fills in the form | Not treated as a new lead; stays Customer | Took the **Customer** branch: stays **Customer**, never sent to Clay; owner set; task **"Existing customer Casey asked for a demo"** (to-do) | HubSpot record |
| 5 | Felix (test 1) set to **SQL** by hand | Deal at Discovery held, amount for 56 staff, associated; contact -> Opportunity | Deal **"O'Connor Plumbing - ServiceTitan"**, **$160,000** (Large band), stage **Discovery held**, pipeline ServiceTitan, owner Johnson, linked to contact and company; contact **Opportunity** | `screenshots/pipeline-servicetitan-board.png` |
| 6 | Gina (test 3) set to SQL by hand: no company | Size unknown: deal with no amount + task to confirm; Opportunity | Deal created with **no amount** and task **"Confirm technician count for this deal"**; Opportunity. **Two flaws found:** the deal was named " - ServiceTitan" (no company to name it after) and the task had **no owner** (Gina never got one). **Both fixed** the same hour: size-unknown deals are now named after the person, and robot 2 now starts by assigning an owner if there is none (never overwriting one) | robot 2 read back by API |

**Clay credits for all Week 3 tests:** 4 (balance 22,460.2 before, 22,456.2 after).

### What the tests taught
- A record that skips a stage (Needs review straight to SQL) arrives without an owner. Every robot that creates work
  should make sure an owner exists first.
- A deal name built from a company token is blank when there is no company. Name from something every record has.
- The customer guard matters: without it, a current customer's demo request would have been scored as a new lead.
