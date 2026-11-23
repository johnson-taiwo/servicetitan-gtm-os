# Practitioner notes - finding and sizing US trade contractors (first-hand, Aug - Nov 2026)

**Source: practitioner.** These are not ServiceTitan facts. They are what the author measured while building outbound
lists of US home-service and trade contractors (HVAC, plumbing, roofing, electrical and similar) as a list builder at
an outbound agency, August to November 2026. No client names or lead data. Numbers are from the author's own build
records, rounded where marked. They matter here because ServiceTitan's buyers are exactly these companies, and the hard
part of selling to them is not the copy, it is proving which companies are real, in trade, and big enough.

## 1. The core problem: size can't be read from a database

| Observation | Measured | What it means for a ServiceTitan list |
|---|---|---|
| Vendor headcount bands are a pre-sort, not proof | Free company dataset (People Data Labs open file, 6.2M US companies with a website): where it said 1-10 staff, Clay's company enrichment said 10+ for 164 of 398 companies tested (41%) | A "1-10" band hides many real 10+ contractors. Filtering on the band alone throws away buyers |
| LinkedIn misses field crews | Matching to a government loan file: 344 companies LinkedIn put under 10 staff had 10+ jobs on file | Technicians rarely keep LinkedIn profiles. LinkedIn headcount undercounts trades |
| Exact counts are possible for free | Feeding the free file's LinkedIn links to a free LinkedIn page counter: exact counts went from 353 to 559 of 955 companies; within 2 of Clay's count for 338 of 348 | A free exact count beats a paid band for a large share |

## 2. Public files that prove size (free, bulk)

| Source | What it gives | Measured result | Caveat |
|---|---|---|---|
| SBA PPP loan data (data.sba.gov, FOIA release 30 Sep 2024) | Jobs reported per borrower, name, city, state, NAICS | About 1 in 7 new trade companies matched by name + state (4,516 of 31,241); about 1,600 proven 10+ that LinkedIn could not size | Jobs are 2020-21 figures; a buyer has to accept them as proof |
| OSHA inspection data | Employees at the inspected site | Since 2021: about 4,900 roofing and 2,250 plumbing/HVAC establishments at 10+ | Only companies that were inspected |
| FMCSA carrier census | Drivers and trucks, with phone | About 600 roofing companies at 10+ drivers by name match alone | Only fleets with USDOT numbers |
| DOL Form 5500 | Benefit plan participants, phone, EIN | Exact lower bound on staff | Only companies with a retirement or benefit plan |

Rule learned: match on phone, or address plus name. Address alone gave 9,102 false hits (shared office buildings);
adding the name check cut it to 1,042.

## 3. Finding contractors the databases don't have

- **Google Maps on a grid is fast and free.** 30 search points over Phoenix returned 229 unique roofing companies in
  66 seconds; about 65% were not in an existing list. Review count and rating come with it. Category and owner do not.
- **Size is still the bottleneck.** Of 40 well-reviewed Maps companies (100+ reviews) checked by hand, only 6 could be
  proven at 10+ staff. Reviews are a hint, not proof.
- **Overture Maps (open places data) has websites and Facebook links, almost no LinkedIn:** in the Phoenix area, 193
  LinkedIn links among 228,082 places.

## 4. Reaching the owner

- **Generic inboxes are common** (info@, office@). Personal Gmail is common at owner-run shops.
- **Guessing emails from a company's pattern fails.** Emails built from a format: about 14-16% passed verification.
  Emails a vendor had already found: about 75%.
- **Mail provider matters.** Contractors behind Mimecast security gateways bounced about 40% in one send. Split them
  out before sending.

## 5. Signals that fire on a schedule

- **Storms:** NOAA storm events, checked weekly, with a 14-day window, gave roofing outreach a dated, local reason
  ("the hail on [date] in [county]").
- **Permits:** city permit data names the contractor in some cities and not in others. For example, one city's permits
  could not name the builder. Check each city's fields before relying on it.

## What this changes for ServiceTitan's GTM (INFERRED from the above)

- The ICP's size line (contractors big enough for ServiceTitan) must be proven from public files, not database bands,
  or the list mixes buyers with one-truck shops.
- The strongest signals are physical and public (storms, permits, licences, loan and fleet files), not intent data.
