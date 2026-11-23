# Signal matrix - ServiceTitan

*Built 23 Nov 2026 with the method in `knowledge-base/playbooks/signal-matrix-framework.md` and
`workflow-anatomy.md`. Inputs: `icp.md`, `personas.md`, `context/docs/research/`. Tags as in `icp.md`. Work in progress:
volumes are estimates until a test pull runs. **INFERRED** = our reading.*

---

## Matrix 1: segments x personas (the angle in each cell)

Angles describe the prospect's situation, never a diagnosis of failure (framework guardrail).

| Segment | Champion: Ops / General Manager | Economic buyer: Owner | Influencer: Office Manager / CSR lead |
|---|---|---|---|
| **S1. Growing residential shop** (10-99 staff) | Adding technicians faster than dispatch can absorb: one board for jobs, techs and invoices (cu §5) | Growth that doesn't cost the owner's evenings: "running a business" instead of the phones (cu §3, Young Plumbing) | More calls than the office can answer: booking and after-hours calls handled (cu §2: Brothers, Vines) |
| **S2. Multi-location / PE platform** (100+ or 3+ sites) | One way of working across every acquired brand (sg §1 #6, #10) | Comparable numbers across brands for the board and the sale (mk §5) | One phone and booking setup across locations (cu §2: Adams Power) |
| **S3. Commercial service contractor** | Job costing and project billing in the same system as service (cu §2: Interstate AC, PIERCE) | Cash: invoices out in days, not weeks (cu §3: Adams Power) | Payroll and billing off spreadsheets (cu §2: Crown) |
| **S4. Roofing and exteriors** | Crews and estimates scheduled through a storm week (pr §5) | Grow from $1M to $3M without adding office staff at the same rate (cu §2: Home Run Roofing) | Lead flow after a storm captured, not lost (INFERRED) |

Priority: **S1 high** (largest pool, most proof), **S4 high** (dated physical trigger, our practitioner edge), **S3
medium** (longer cycle, BuildOps competes), **S2 medium** (few accounts, ServiceTitan's enterprise team covers them).

---

## Matrix 2: buying signals, scored on the four tests

Four tests, 1 to 5 each, out of 20 (class slide 21): **Reason** = clear buying reason · **Volume** = enough companies
per month · **Data** = data you can actually get · **Story** = a natural story for the email. Status uses the
framework's labels. Iron Law: "could a Clay operator build this right now with zero ambiguity?"

| # | Signal | Mother intent (catalog) | Data source and tool | Reason | Volume | Data | Story | **Total** | Status | Segments |
|---|---|---|---|---|---|---|---|---|---|---|
| G1 | **Hiring office staff:** CSR, dispatcher, call-center or office manager role open in the last 30 days | Hiring specific roles | Clay "Find Jobs" (PredictLeads, Google Jobs, Clay jobs data), keywords on title; company domain then sized (sg §2d #31) | 5 | 4 | 4 | 5 | **18** | VERIFIED (PARTIAL): coverage of small shops untested | S1, S2, S3 |
| G2 | **Hiring field technicians:** 2+ technician or installer roles open in the last 30 days | Hypergrowth (hiring velocity) | Clay "Find Jobs", job count per company (sg §2d) | 4 | 5 | 4 | 4 | **17** | VERIFIED (PARTIAL) | S1, S3, S4 |
| G3 | **Storm hit the service area:** hail or wind report in the company's county in the last 14 days | Niche-specific play | SPC daily storm reports and NWS API (real time); NCEI Storm Events lags 75-90 days (sg §2b); roofers in those counties via Google Maps grid (pr §3) | 3 | 4 | 5 | 4 | **16** | VERIFIED | S4 |
| G4 | **Outgrowing a starter app:** job post asks for Jobber or Housecall Pro experience, or the website's booking widget is a starter app | Competitor customers | Job description text via Find Jobs; website check with a Clay AI column (INFERRED, untested) | 5 | 2 | 2 | 5 | **14** | VERIFIED (COMPOUND): use only with G1 or G2 | S1 |
| G5 | **Second location opened** | Hypergrowth | New Google Business Profile for the same brand; new licence address (sg §2c, §2e) | 5 | 2 | 2 | 4 | **13** | VERIFIED (PARTIAL) | S1, S2 |
| G6 | **Permits filed with the company named** | Niche-specific play | City open data: Chicago daily with contractor names; Austin and NYC fields unconfirmed (sg §2a) | 3 | 3 | 3 | 3 | **12** | VERIFIED (PARTIAL): city by city | S1, S3 |
| G7 | **Bought by or into a PE platform** | New leadership / ownership change | Trade press, platform press releases, Capstone reports (sg §2g); unstructured | 4 | 2 | 2 | 3 | **11** | VERIFIED (PARTIAL) | S2 |
| G8 | **New or upgraded contractor licence** | Recently incorporated businesses | CSLB (free, name and phone), TDLR (daily), DBPR (weekly) (sg §2c) | 2 | 4 | 5 | 2 | **13** | KILLED as a trigger: new licensees are mostly under our size floor. Kept as a free source of company names and phones | - |
| G9 | **Review count growing fast** | Hypergrowth | Google Places API | 3 | 4 | 1 | 3 | **11** | KILLED: Places terms forbid storing past results, so growth can't be measured. Current review count kept as a size hint only (pr §3) | - |
| G10 | **Refrigerant change / end of 25C credits** | Regulatory changes | EPA rule, IRS pages (mk §1, §2) | 3 | 5 | 1 | 3 | **12** | KILLED as a signal: it hits every HVAC company at once, no company-level data. Kept as copy context | - |

**Universal vs niche.** G1, G2 and G4 come from one Clay "Find Jobs" build: change the keywords, and the same table
serves S1, S2 and S3. G3 (storms) and G6 (permits) are niche builds with their own data sources. That is the "build
once, validate many" point from the framework.

## The top signals to build first, and why

1. **G1, hiring office staff (18/20).** The clearest buying reason in the research: ServiceTitan's own proof is about
   phones and booking (abandoned calls "near 30%" cut to 5%, cu §2), and an open CSR or dispatcher role means the office
   is already the bottleneck. Free search in Clay, so it is also the Week 2 build.
2. **G2, hiring technicians (17/20).** ServiceTitan is priced per technician and grows when customers "hire more
   technicians" (sg §1 #3, #4). Highest volume. Stacks with G1: technicians and office staff at once is the strongest cell.
3. **G3, storm in the service area (16/20).** A dated, local, physical reason, the kind of signal no database sells.
   Best data access of all (free, real time). Niche to roofing and exteriors (S4), where Max is now piloting (co).
4. **G4, outgrowing a starter app (14/20, compound).** The sharpest story ("growing past five or six trucks", sg §2h)
   but thin data. Use as a secondary signal on G1 or G2 rows to switch the angle to displacement.

---

## Matrix 3: workflow tracker

| W # | Status | Workflow | Mother intent | Persona | Segment | Size | Secondary | Channel | Cadence |
|---|---|---|---|---|---|---|---|---|---|
| W1 | Planned | Office under pressure: hiring CSRs or dispatchers | G1 | Owner (send), Ops/GM (second thread) | S1 | 10-99 | G2, G4 | Email + LinkedIn | Weekly |
| W2 | Planned | Adding technicians | G2 | Owner | S1, S3 | 10-99 | G1 | Email | Weekly |
| W3 | Planned | Storm week for roofers | G3 | Owner | S4 | 10+ | Size proof (pr §2) | Email | Daily check, weekly send |
| W4 | Planned | Second location | G5 | Owner | S1, S2 | 20+ | G1 | Email | Monthly |
| W5 | Planned | Commercial permits | G6 | Ops/GM | S3 | 20+ | G2 | Email | Weekly (Chicago first) |

Columns filled once live: campaigns, launch date, iterations, changelog, Clay URL, copy URL, leads, emails sent,
progress, bounce rate.
