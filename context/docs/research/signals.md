# Signals - ServiceTitan (raw research, checked 2026-11-23)

Scope: public evidence of what makes a trade contractor need ServiceTitan now, and public data sources that could detect it. Raw facts only. Interpretation is marked INFERRED. Every line carries its source URL and the date checked (2026-11-23).

Legend for "How verified": PAGE = page opened and text read this session. FILE = official file opened. SEARCH = seen in a search-engine summary only (page refused automated fetch); lower confidence.

---

## 1. What ServiceTitan itself says triggers a purchase or expansion

| # | Statement (quoted exactly) | Source (checked 2026-11-23) | How verified |
|---|---|---|---|
| 1 | Pricing page: "For residential and commercial trade contractors with four trucks in a single shop, to thousands across multiple locations, we have you covered." | https://www.servicetitan.com/pricing | PAGE |
| 2 | 10-K: "As our customers grow their businesses while using our platform, they often hire and add more users to their existing subscription and also complete more transactions through our platform". | https://www.sec.gov/Archives/edgar/data/1638826/000163882626000028/ttan-20260131.htm | PAGE |
| 3 | 10-K: "As our customers grow on our platform and expand into additional locations, generating more sales, hiring more technicians and automating more workflows, they can also significantly increase GTV". | same as 2 | PAGE (https://www.sec.gov/Archives/edgar/data/1638826/000163882626000028/ttan-20260131.htm) |
| 4 | 10-K pricing basis: Core fees "are charged on a per unit basis which may vary by product, primarily on a per technician per month basis"; Pro pricing is "generally based on the number of field technicians at the customer but in some cases directly tied to the number of end customers or the customer's revenue." | same as 2 | PAGE (https://www.sec.gov/Archives/edgar/data/1638826/000163882626000028/ttan-20260131.htm) |
| 5 | 10-K buyer: "The key decision maker for our customers is typically the business owner." Larger customers may have a CIO or "a general manager to oversee business operations. The general manager is typically also a key influencer in buying decisions." | same as 2 | PAGE (https://www.sec.gov/Archives/edgar/data/1638826/000163882626000028/ttan-20260131.htm) |
| 6 | 10-K, PE driver: "the influx of professional operators, including private equity owners, who are investing in and consolidating trades businesses, standardizing the operations of their portfolio companies, implementing best practices and new software functionality, and accelerating the digital shift". | same as 2 | PAGE (https://www.sec.gov/Archives/edgar/data/1638826/000163882626000028/ttan-20260131.htm) |
| 7 | 10-K named competitors: "Salesforce, SAP, FieldEdge, Workwave, ServiceTrade, AccuLynx, BuildOps, HouseCall Pro, JobNimbus and Jobber." Competition includes "narrow bundled solutions for down-market trades businesses." | same as 2 | PAGE (https://www.sec.gov/Archives/edgar/data/1638826/000163882626000028/ttan-20260131.htm) |
| 8 | 10-K acquisition channels: "digital marketing, outbound direct marketing, significant word-of-mouth referrals, and partnerships with industry associations, original equipment manufacturers, and distributors." | same as 2 | PAGE (https://www.sec.gov/Archives/edgar/data/1638826/000163882626000028/ttan-20260131.htm) |
| 9 | Comparison pages vs Jobber and vs Housecall Pro: "If you are a medium-to-larger enterprise or have ambitions to grow, our platform will be far more able to support you compared to Jobber." (Housecall Pro page uses "medium-sized to larger enterprise"). Cites "Support for Multiple Business Divisions, Business Units, and Properties". | https://www.servicetitan.com/comparison/servicetitan-vs-jobber ; https://www.servicetitan.com/comparison/servicetitan-vs-housecall-pro | PAGE |
| 10 | PE page: "500+ add-on integrations facilitated for PE-backed customers last year"; "Average of under 2 months integration time." | https://www.servicetitan.com/market/private-equity-solutions-software | PAGE |
| 11 | Q2 FY2027 call (Sept 8, 2026): "We recently started leading with Max for new logos in residential in-home trades." Pricing page: "All packages are now available for in-home Residential customers; Max pilots are actively launching for other business types." | https://www.tradingkey.com/news/transcripts/262158714-tradingkey ; https://www.servicetitan.com/pricing | PAGE |
| 12 | Q2 FY2027 call, customer example: "Delponte's technician to admin ratio improved from 2:1 in 2025 to 3:1 in 2026". | https://www.tradingkey.com/news/transcripts/262158714-tradingkey | PAGE |
| 13 | Case study (Priority Heating, Cooling & Plumbing, Jan 14, 2026): trigger was a failed home-built system; "They were still scribbling credit card numbers at job sites, mailing invoices and tracking down paperwork"; "it often took 40-50 days to get paid". | https://servicetitan.com/blog/success-story-priority-heating-cooling-plumbing | PAGE |
| 14 | Case study (Ellington Air Conditioning, 7 service techs, "$6.5 million in annual sales"): had built "his own robust proposal and estimating system in Excel"; owner's advice: "What is your expectation of growth? What are your goals in the next five years?" | https://www.servicetitan.com/blog/success-story-ellington-ac | PAGE |
| 15 | Case study (Affordable Heating & Air): "One of my first steps was to say we needed to move to ServiceTitan"; company grew "from one truck to 50 employees" and opened "a second branch" in Simi Valley. | https://www.servicetitan.com/blog/success-story-affordable-heating-air | PAGE |
| 16 | Case study (The Air Squad) shows the reverse trigger: "ServiceTitan was too expensive. As the owner of a small company, he couldn't justify the price anymore, especially if he wanted to grow his team." He moved to a competitor and returned three weeks later. Revenue cited: $675,000 (2021), $1 million (following year). | https://www.servicetitan.com/blog/success-story-the-air-squad | PAGE |
| 17 | Published technician minimum or "ideal customer" size. | Not found on servicetitan.com. Third-party claims (2+ techs, 3+ techs, 22-tech contract minimum) seen only in search summaries of non-official pages; not verified. | - |

INFERRED from 1-16: ServiceTitan's own words tie purchase to (a) adding technicians, (b) adding locations or divisions, (c) PE ownership and add-on integration, (d) homegrown or spreadsheet systems breaking, (e) moving from down-market tools (Jobber, Housecall Pro) when growth plans exceed them.

---

## 2. Signal sources: does a real, accessible data source exist?

### 2a. Building / mechanical / plumbing / electrical permits

| # | Source | URL | Coverage | Cost | Refresh | Notes | How verified (2026-11-23) |
|---|---|---|---|---|---|---|---|
| 18 | City of Chicago, Building Permits (Socrata ydr8-5enu) | https://www.chicago.gov/city/en/dataset/building_permits.html ; https://data.cityofchicago.org/Buildings/Building-Permits/ydr8-5enu | Chicago, "January 1, 2006 to present" | Free | "Data is updated daily" | "Contractor type, name, and contact information. Data includes up to 15 different contractors per permit if applicable." | PAGE (city page); Socrata portal returned 403 to this agent |
| 19 | City of Austin, Issued Construction Permits (3syk-w9eu) | https://data.austintexas.gov/d/3syk-w9eu ; https://catalog.data.gov/dataset/issued-construction-permits | Austin; "Building, Electrical, Mechanical, and Plumbing Permits and Driveway/Sidewalk Permits" | Free | data.gov "Dataset Last Updated: October 26, 2026" (catalog mirror) | Includes issue date, location, description of work, square footage, valuation, units. Contractor columns not confirmed (portal 403). | PAGE (data.gov mirror) |
| 20 | NYC, DOB NOW: Build - Approved Permits (rbx6-tga4) | https://data.cityofnewyork.us/Housing-Development/DOB-NOW-Build-Approved-Permits/rbx6-tga4 ; https://catalog.data.gov/dataset/dob-now-build-approved-permits | NYC; "all approved construction permits in DOB NOW except for Electrical, Elevator, and Limited Alteration Application (LAA) which have their own datasets" | Free | data.gov mirror "Dataset Last Updated November 17, 2026" | Older BIS-era permits are in "DOB Permit Issuance" (separate dataset). Work-type and contractor field names not confirmed. | PAGE (data.gov mirror) |
| 21 | Shovels.ai | https://www.shovels.ai/ ; https://www.shovels.ai/pricing | Claims "Building permits 210M+", "Licensed contractors 4.33M+", "Jurisdictions covered 3,600+", "New permits added monthly 7M+" | Free $0/mo (1 year history, 500 credits/mo); Basic $599/mo (25,000 credits); Pro $999/mo; Enterprise custom. "1 credit = 1 record". | Not stated on pages read | Has a Contractors dataset ("Contractor profiles with licenses, work history"). | PAGE |
| 22 | BuildZoom (data arm buildzoomdata.com) | https://buildzoomdata.com/ | Vendor/third-party claims vary: 350M+ permits, 2,400 jurisdictions; Cherre cites 175M | Paid, price not published | Bulk exports weekly or monthly (per search summary) | API gated behind a data account. Reseller: Cotality Building Permits. | SEARCH (buildzoom.com/data page returned almost no text) |
| 23 | Construction Monitor | https://www.constructionmonitor.com/ | Homepage: "10,660 This Week", "2,751,532 Last 12 Months", "33,461,947 Total Permits"; "Millions of Permits Updated Daily"; "Data digestable via API and FTP." | Paid. Only price found is old (Los Angeles: $96/month, $960/year) from JLC article; current price Not found | Daily (vendor claim) | Building, solar, pool permits. | PAGE (homepage) / SEARCH (price) |

### 2b. Severe weather and storm events

| # | Source | URL | Coverage | Cost | Refresh | Notes | How verified (2026-11-23) |
|---|---|---|---|---|---|---|---|
| 24 | NOAA NCEI Storm Events Database | https://www.ncei.noaa.gov/stormevents/ ; bulk CSV: https://www.ncei.noaa.gov/pub/data/swdi/stormevents/csvfiles/ | US, from January 1950; by county/zone, event type (hail, wind, tornado, flood, heat, cold, etc.) | Free | NCEI FAQ (search summary): updated "within 75-90 days after the end of a data month". Bulk folder holds StormEvents_details-ftp_v1.0_d2026_c20261119.csv.gz (a 2026 file built 2026-11-19). Search summary of main page says data runs to "January 2026" (conflicts with the newer file; check before use). | Lagging source; good for history, not real-time. | PAGE (bulk folder) / SEARCH (lag) |
| 25 | NOAA SPC Storm Reports | https://www.spc.noaa.gov/climo/reports/ | US, daily; tornado, hail (size), wind reports with county, state, lat/lon | Free | Daily; "All Reports Are Considered Preliminary" | CSV links for each report type. Example read: hail report, Poyen, AR, 22-23 Nov 2026. | PAGE |
| 26 | NWS API (api.weather.gov) | https://api.weather.gov/alerts/active | US active alerts (heat, freeze, storm warnings) | Free, no key; User-Agent header required | Real-time | Returned GeoJSON FeatureCollection to a test call this session. | PAGE (live call) |
| 27 | Commercial hail maps (HailTrace, Interactive Hail Maps, HailPoint, others) | https://www.interactivehailmaps.com ; https://www.hailpoint.com/products | Address-level hail swaths | Paid. Prices seen only on third-party or old pages (e.g. HailTrace ~$94-$260/mo, IHM $999-$1,999/yr per a competitor page). Not verified. | Per storm | Mainly sold to roofers. | SEARCH |

### 2c. New or renewed contractor licenses

| # | Source | URL | Coverage | Cost | Refresh | Notes | How verified (2026-11-23) |
|---|---|---|---|---|---|---|---|
| 28 | California CSLB Public Data Portal | https://www.cslb.ca.gov/onlineservices/dataportal/ ; https://www.cslb.ca.gov/onlineservices/dataportal/ContractorList.aspx ; ListByClassification.aspx | All CA licenses "currently renewed, or *expired but renewable" | "There is no charge for this service." | "The information provided is current at the time the list is downloaded." | License master: "license number, business name, address, telephone number, license status, issue/expiration dates, classification(s), bond information, and workers' compensation information." Personnel file: names, titles. Workers' comp file: coverage type, carrier, policy dates. "Email addresses are not provided". Classes listed on the page include "C-4 - Boiler, Hot Water Heating and Steam Fitting Contractor", "C-10 - Electrical Contractor", "C-20 - Warm-Air Heating, Ventilating and Air-Conditioning Contractor", "C-36 - Plumbing Contractor", "C-38 - Refrigeration Contractor", "C-39 - Roofing Contractor". Up to 10 classes per list. Format .xls. | PAGE |
| 29 | Texas TDLR license files | https://www.tdlr.texas.gov/LicenseSearch/licfile.asp | TX: "Airconditioning Contractors (3.67 Mb)", "Electrical Contractors (3.80 Mb)", "All A/C Technicians (10.52 Mb)", "All Licenses (190.14 Mb)" | Free download | "The files are updated everyday and saved in CSV (Comma delimited) format." | Texas plumbing licenses are not in this list (separate board); not checked. Also mirrored as "TDLR - All Licenses" (7358-krk7) on Austin open data. | PAGE |
| 30 | Florida DBPR Instant Public Records (Construction Industry Licensing Board files) | https://www2.myfloridalicense.com/construction-industry/public-records/ ; https://www2.myfloridalicense.com/instant-public-records/ | FL CILB Certified and Registered files; separate Electrical Contractors files | Free download | "Data is refreshed weekly" (search summary) | ASCII, quote/comma delimited; includes licensee name, DBA, class code; active and inactive licensees. | SEARCH (pages 403) |

INFERRED: Issue date and expiration date fields (CSLB, TDLR) allow "new license in last N days" and "renewal due" lists. CSLB workers' comp "exempt" status could flag no-employee shops; not tested.

### 2d. Hiring technicians, dispatchers, CSRs

| # | Source | URL | Coverage | Cost | Refresh | Notes | How verified (2026-11-23) |
|---|---|---|---|---|---|---|---|
| 31 | Clay "Find Jobs" (source and enrichment) | https://university.clay.com/lessons/find-jobs-source-enrichment-clay-101 | "This waterfall pulls data from: PredictLeads Google Jobs Clay's Native Jobs Database"; inputs: company domain, job keywords | A Clay community reply says the source is free within plan search and 50,000-row table limits (not official) | On run; separate "Job Posting signal" monitors a table on a schedule | Lesson: "Google jobs has a maximum of four keywords per search." Indeed coverage not stated. | PAGE (lesson) / SEARCH (cost, community) |
| 32 | Indeed | https://docs.indeed.com/release-notes | Indeed's remaining APIs are employer-side (Job Sync, Sponsored Jobs). Public job-search Publisher API: third-party sources say retired (2023 or 2024; they disagree). | n/a | n/a | No official open job-search API found. | SEARCH |
| 33 | TheirStack | https://theirstack.com/en/pricing | Aggregates job boards and ATS (LinkedIn, Indeed, Glassdoor per third-party pages; own docs cite 300k+ sources) | Free: "50 company credits and 200 API credits per month"; paid "begin at $19 monthly"; 1,000 company credits "$109 one-time" | Not stated on page read | Coverage of small trade shops posting only on Indeed/Craigslist not verified. | PAGE (pricing) / SEARCH (sources) |
| 34 | LinkedIn Jobs official API for third parties | - | Not found (no public job-search API identified) | - | - | - | - |

### 2e. Google Business Profile reviews and new locations

| # | Source | URL | Coverage | Cost | Refresh | Notes | How verified (2026-11-23) |
|---|---|---|---|---|---|---|---|
| 35 | Google Places API (New) | https://developers.google.com/maps/documentation/places/web-service/data-fields ; https://developers.google.com/maps/billing-and-pricing/pricing | Global business listings; field "User rating count userRatingCount" is in Place Details Enterprise / Text Search Enterprise; "reviews" needs Enterprise + Atmosphere | Place Details Enterprise $20.00 per 1,000 (first tier), Text Search Enterprise $35.00 per 1,000, Enterprise + Atmosphere $25-$40 per 1,000; free usage cap listed as 1,000 events/month for these SKUs | Live per call | Policy: "You must not pre-fetch, cache, or store Places API content beyond the allowed exceptions, although the place_id is exempt from caching restrictions." | PAGE |

INFERRED: Review-count growth needs repeat calls over time; the caching rule limits storing past counts from the API itself. Not tested for compliance.

### 2f. Company size proof (free government data)

| # | Source | URL | Coverage | Cost | Refresh | Fields | How verified (2026-11-23) |
|---|---|---|---|---|---|---|---|
| 36 | SBA PPP FOIA loan data | https://data.sba.gov/dataset/ppp-foia | "loan-level data on all disbursed PPP loans" (2020-2021); 14 files: data dictionary, public_150k_plus_240930.csv, public_up_to_150k_1..12_240930.csv | Free | Last updated October 21, 2024; frequency irregular | Header read from file: LoanNumber, DateApproved, BorrowerName, BorrowerAddress, BorrowerCity, BorrowerState, BorrowerZip, ... BusinessAgeDescription, ... JobsReported, NAICSCode, ... PAYROLL_PROCEED. JobsReported is 2020-21 headcount. | PAGE + FILE (dictionary and first line of CSV) |
| 37 | OSHA enforcement / inspection data (DOL) | https://www.osha.gov/data ; https://catalog.data.gov/dataset/dol-enforcement-data-inspection ; enforcedata.dol.gov now redirects to https://data.dol.gov/ | Inspections, violations; establishment name, NAICS | Free; DOL API key needed per third-party pages | Not confirmed | An employee-count field in the inspection table (e.g. nr_in_estab): Not found / not confirmed. OSHA ITA injury data has establishment size and annual_average_employees (per OSHA data dictionary, search summary). | SEARCH |
| 38 | DOL EBSA Form 5500 datasets | https://www.dol.gov/agencies/ebsa/about-ebsa/our-activities/public-disclosure/foia/form-5500-datasets | Employers with benefit plans filing 5500 / 5500-SF; 2025 group listed | Free CSV | "typically updated around the first of each month" | Participant counts (plan-level, not exact headcount). File layouts per CSV. | SEARCH (dol.gov 403) |
| 39 | FMCSA Company Census File (USDOT) | https://data.transportation.gov/d/az4n-8mr2 ; https://catalog.data.gov/dataset/company-census-file ; https://www.fmcsa.dot.gov/registration/fmcsa-data-dissemination-program | Entities with a USDOT number (only trucks/vehicles that require registration) | Free | "updated daily from a 24-hour old database" (FMCSA, search summary); catalog modified 2026-11-06 | Power units and drivers from Form MCS-150. | SEARCH |
| 40 | Census CBP / SUSB | see market.md section 6 | Counts by size class, not company names | Free | Annual | Market sizing only, not a lead list. | FILE |

INFERRED: Many small HVAC/plumbing vans fall under weight thresholds and may not hold a USDOT number, so FMCSA coverage of service fleets is partial. Not measured.

### 2g. PE acquisition / new ownership announcements

| # | Source | URL | Coverage | Cost | Refresh | Notes | How verified (2026-11-23) |
|---|---|---|---|---|---|---|---|
| 41 | Capstone Partners residential services M&A reports | https://www.capstonepartners.com/insights/report-residential-services-ma-update | Sector deal counts, named deals | Free | Periodic (Oct 2026 edition read; July 2026 HVAC update also exists) | Aggregate, few target names. | PAGE |
| 42 | Trade press deal news (Contractor Magazine, PM Magazine, ACHR News, Dealroom news) | https://www.contractormag.com/plumbing/residential-plumbing/news/55391460/blue-cardinal-expands-west-texas-footprint-with-dd-home-services-acquisition ; https://dealroom.co/news/169369-southern-home-services-buys-vanderford-its-second-houston-deal-in-a-year/ | Named add-on deals (e.g. Southern Home Services buying Vanderford Air and Plumbing, closed Nov 2, 2026) | Free to read | As published | No structured feed confirmed. | SEARCH |
| 43 | Platform press releases (Apex, Flint, Champions, Wrench) | https://www.apollo.com/insights-news/pressreleases/2026/05/apex-service-partners-and-alpine-investors-announce-strategic-mi ; https://www.constructionowners.com/news/flint-group-sells-majority-stake-to-permira-funds-as-home-services-platform-targets-expansion | Platform-level deals | Free | As published | See market.md section 5. | PAGE |
| 44 | Structured PE deal database (PitchBook etc.) | - | Paid; not checked | - | - | Not checked this session. | - |

### 2h. Switching triggers from reviews and forums

Reddit (r/HVAC, r/Plumbing, r/smallbusiness): Not accessible. reddit.com refused this agent's fetch and the search tool reported reddit.com is "not accessible to our user agent". No Reddit quotes are included. Other public forums and review sites below.

| # | Quote (exact) | Who / where | Source (checked 2026-11-23) | How verified |
|---|---|---|---|---|
| 45 | "nobody loves their software, they just hate it less than the last one. ServiceTitan if you're growing past five or six trucks, Jobber or Housecall Pro if you're smaller and want something the techs will actually use. Whatever you land on, don't expect it to fix the phones." | user "tommy23", HeatingHelp forum, posted 2026-11-06 | https://forum.heatinghelp.com/discussion/197740/field-service-business-software | PAGE |
| 46 | "I'm not entirely happy with the one we've been using for several years now. I feel like it's missing a few key features and am looking to upgrade." | thread starter "JohnNY" (NYC master plumber), Sept 2024 | same as 45 | PAGE (https://forum.heatinghelp.com/discussion/197740/field-service-business-software) |
| 47 | "I use Housecall Pro and been happy with it." | user "EzzyT", Sept 2024 | same as 45 | PAGE (https://forum.heatinghelp.com/discussion/197740/field-service-business-software) |
| 48 | Housecall Pro "just didn't have all the aspects we needed to run a residential and commercial service, install and new construction business." | Capterra reviewer of ServiceTitan | https://www.capterra.com/p/150053/ServiceTitan/reviews/ | SEARCH (Capterra blocked) |
| 49 | Housecall Pro "is primarily for residential, and our client base is 75% commercial." | Capterra reviewer of ServiceTitan | https://www.capterra.com/p/150053/ServiceTitan/reviews/ | SEARCH |
| 50 | ServiceTitan "not worth five times the cost of Housecall Pro" (reverse direction). | Capterra reviewer | https://capterra.com/p/150053/ServiceTitan/reviews/?page=8 | SEARCH |
| 51 | Of "37 Capterra reviewers who reported switching away from ServiceTitan", common destinations were FieldPulse, Housecall Pro, Jobber, BuildOps (reverse direction). | Capterra alternatives page | https://capterra.com/p/150053/ServiceTitan/alternatives/ | SEARCH |
| 52 | Vendor-written growth ceiling claim: HVAC contractors "typically hit Housecall Pro's practical ceiling at $1.2M-$1.8M in annual revenue or 8+ active technicians". Seller of a competing add-on; unverified. | US Tech Automations blog | https://ustechautomations.com/resources/blog/automate-hvac-contractors-outgrow-housecall-pro-2026 | SEARCH |
| 53 | Jobber's own reviews list "limited and basic reporting options" as a con (Capterra summary). | Capterra | https://www.capterra.com/p/127994/Jobber/reviews/?page=3 | SEARCH |

INFERRED from 45-53: Public complaints cluster on growth past a handful of trucks, commercial or new-construction work, reporting and job costing depth, and (in the reverse direction) ServiceTitan's price for small shops. Sample is small and partly vendor-written.

---

## 3. Signal-to-source summary

| Signal | Real source found? | Best free source | Paid option | Refresh |
|---|---|---|---|---|
| Permits (mechanical/plumbing/electrical) | Yes | City open data (Chicago, Austin, NYC) | Shovels.ai, BuildZoom, Construction Monitor | Daily (Chicago); varies |
| Severe weather / storms | Yes | SPC reports (daily), NWS API (real-time), NCEI Storm Events (75-90 day lag) | Hail map vendors | Daily / real-time / monthly |
| New or renewed licenses | Yes (state by state) | CSLB (CA), TDLR (TX), DBPR (FL) | Shovels.ai contractor data | On download / daily / weekly |
| Hiring techs, dispatchers, CSRs | Partly | Clay Find Jobs (PredictLeads, Google Jobs, Clay DB) | TheirStack | On run |
| Review count growth / new locations | Yes, with caching limits | Google Places API (1,000 free events/month on Enterprise SKUs) | Same API, paid tiers | Live |
| Size proof | Yes (old or partial) | PPP JobsReported (2020-21), Form 5500, FMCSA, OSHA | - | Static / monthly / daily |
| PE acquisition | Partly (unstructured) | Trade press, platform releases, Capstone | PitchBook (not checked) | As published |
| Switching complaints | Partly | HeatingHelp, Capterra/G2 (blocked to fetch) | - | Ad hoc |

## Gaps
- Reddit threads: Not found (site not accessible to this agent).
- Exact column names for Austin and NYC permit datasets (Socrata portals returned 403).
- OSHA inspection employee-count field: not confirmed.
- Current Construction Monitor and BuildZoom prices.
- Whether Clay Find Jobs covers Indeed postings from small trade shops.
- Official ServiceTitan statement of a minimum technician count.
