# How I built the Week 3 CRM (re-enact it step by step)

*Written as Johnson's own build diary, 27 Nov 2026. Each step has: **why**, **click by click** (how to do it in the
HubSpot or Clay screens), **what I typed**, **check**, **cost**, and **what went wrong / what I learned**. Some steps
were done faster through the HubSpot API (noted as "Fast way"); the click path gives the same result.*
The rules behind every step are in `crm-design.md` (read that first: it says WHAT and WHY, this file says HOW).

**Accounts used:** my own HubSpot (portal "Akison", 48912703, Sales Hub Professional 14-day trial) and my own Clay
workspace ("Johnson's Workspace", Treford credits). Never an employer's or a client's account.

---

## Step 0. Turn on the trial and give the key the right permissions
**Why:** HubSpot's free plan can't run workflows. My June HubSpot build hit exactly this (403 on the workflows API),
so this time I started the trial first.
**Click by click:**
1. HubSpot > left menu > **Workflows** (it shows a lock) > the upgrade page > **Start 14-day trial** (Sales Hub
   Professional) > **Get started**. No card needed. Note the end date (about 11 Dec 2026).
2. Gear icon (Settings) > **Integrations** > **Private Apps** > "Go to Legacy Apps" > my app **Research Automation** >
   **Edit app** > **Scopes** > **Add new scope**. Search and tick: `forms`, `crm.objects.companies.write`,
   `crm.schemas.contacts.read/write`, `crm.schemas.companies.read/write`, `crm.schemas.deals.read/write`. Click
   **Update**, then **Commit changes**, then **Commit scope changes**. (`automation` was already there.)
**Check:** the left menu says "14 days left, Sales Hub Professional"; Workflows opens without the upgrade page.
**Learned:** the key (access token) stays the same when you add scopes, so nothing needs re-pasting.

## Step 1. Write the rules first
**Why:** the CRM Design Workbook says design on paper before clicking. A rule a teammate can apply the same way by
hand is a rule a robot can apply too.
**What:** `context/crm/crm-design.md`: the motion, fit vs intent, the fit rule (8 checks in order), lifecycle stages,
the buyer-led pipeline, deal amounts with the logic shown, the properties, both robots in plain words, the tests.

## Step 2. Create the properties (the boxes)
**Why:** "Create a property before anything writes to it." Clay and robots can only fill boxes that already exist.
**Click by click:** Gear > **Properties** > object **Contact properties** > **Create property**:
| Label | Field type | Options (type exactly) | Group |
|---|---|---|---|
| ICP fit | Dropdown select | Meets criteria · Doesn't meet criteria · Needs review | ServiceTitan (Week 3) |
| ICP reason | Single-line text | | ServiceTitan (Week 3) |
| Lead channel | Dropdown select | Demo request · Outbound - office under pressure | ServiceTitan (Week 3) |
Then switch to **Deal properties** > Create property > **Lost reason** (dropdown: Price · Staying on current tool ·
Timing · No decision maker · Went quiet · Not a fit).
**Reused, not recreated:** `MQL date` (contact, date) already existed from my June lead-management build.
**Fast way:** `POST /crm/v3/properties/contacts` (and `/deals`) with the same labels and options; a property group
"ServiceTitan (Week 3)" keeps them together.
**Check:** open a contact > Actions > View all properties > search "icp": both boxes show.
**Learned:** my portal already had June boxes (lead_score, outreach_status, a text "lead_source_detail"). I left them
alone and made a new dropdown "Lead channel" instead of changing the old text box's type, because changing it would
break the June build that writes to it.

## Step 3. Create the ServiceTitan pipeline
**Why:** a buyer-led pipeline is graded. Every stage is named after what the BUYER did.
**Click by click:** Gear > **Objects** > **Deals** > **Pipelines** tab > **Create pipeline** > name `ServiceTitan`.
Stages and win %, in order:
| Stage | Win % |
|---|---|
| Discovery held | 20 |
| Operations review | 40 |
| Demo and proposal | 60 |
| Contracting | 80 |
| Closed won | 100 (closed) |
| Closed lost | 0 (closed) |
Optional: **Edit stage properties**: Amount required at "Demo and proposal", Lost reason required at "Closed lost".
**Fast way:** `POST /crm/v3/pipelines/deals` with the six stages and `metadata.probability`.
**Check:** Deals > pipeline dropdown shows "ServiceTitan" with the six stages.
**Learned:** currency stays USD (ServiceTitan sells in the US); the class guide set NGN because PaidHR is Nigerian.
On the free plan only one pipeline is allowed (my June build hit that); the trial allows more.

## Step 4. Create the lists (segments)
**Why:** "Needs review" is how a person catches what the robot can't decide. The second list is my own
existing-customer guard.
**Click by click:** CRM > **Segments** > **Create segment** > Contacts > Active:
1. `Needs review (ServiceTitan)`: filter **ICP fit is any of Needs review**.
2. `Customers who requested a demo (ServiceTitan)`: **Lifecycle stage is any of Customer** AND **Lead channel is any
   of Demo request**.
**Fast way:** `POST /crm/v3/lists` with `processingType: DYNAMIC`.
**Check:** both lists show (0 records until the tests).

## Step 5. Create the demo request form
**Why:** a form is the "front door". It is how a contact gets into HubSpot and what starts robot 1. Lola's guide:
"A workflow can only act on data that's in HubSpot", and the trigger must be **Form submission** for a form.
**Click by click:** search "forms" in the top bar > **Create form** > **Start from blank** > name
`ServiceTitan demo request`. Drag in: First name (required), Email labelled "Work email" (required), Company name,
Company website. Add **Lead channel** as a **hidden** field with default `Demo request`. Thank-you message: "Thanks.
Someone from our team will call you within one working day." Submit button: "Request a demo". **Review and update**:
check **Create new contacts for email addresses** is ON. **Update**.
**Fast way:** `POST /marketing/v3/forms/` (needed: email `validation` block, `postSubmitAction.type = thank_you`,
`createdAt`).
**My choice:** I did NOT block Gmail on the form (the class did). A Gmail sign-up is still a person who asked for a
demo; my rule sends it to "Needs review" so a human decides, instead of losing the lead at the door.

## Step 6. Build the Clay "detective" table
**Why:** Clay checks the company (country, staff, trade) and writes the answer back. Same rules as my Week 2 build.
**Where:** my Week 2 Clay workbook, new table `09 Demo requests (HubSpot ICP check)`, so the whole system lives in one
workbook.
**Click by click (Clay):**
1. **+ Add** > new table in the workbook > **Import > Webhook**. Copy the webhook URL (the HubSpot robot posts to it).
2. Send one practice record to the URL so the Webhook column appears, then add formula columns (each has a
   plain-English description):
| Column | What it does |
|---|---|
| HubSpot Record ID | `{{Webhook}}.hs_object_id`: which HubSpot contact to update |
| Email, First Name, Website Given | read from the webhook |
| Email Domain | everything after the @ |
| Personal Email? | true for gmail, yahoo, hotmail, outlook, icloud, aol, proton and similar |
| Company Domain | the website if given, else the email domain, blank if personal |
3. **Enrich Company** (cloned from my Week 2 column, input = Company Domain, run only if Company Domain is filled).
   About 1 credit. Then read from it: Company Name, Country, Employee Count, Industry, Trade Text.
4. **Exclusions Lookup** (lookup in my Week 2 Exclusions table by domain, free) > `Excluded?`.
5. **ICP Fit** and **ICP Reason**: plain formulas, checked in this order: personal email > no company data > excluded
   > not US > no staff count > outside 10-99 > not a service trade > Meets criteria. The text matches the HubSpot
   dropdown letter for letter.
**Check:** practice row O'Connor Plumbing: Meets criteria, "US HQ, 56 staff, Specialty Trade Contractors". 1 credit.
**Learned:** a webhook source only receives records once it is attached to a "source" column; my first practice post
went nowhere until that column existed.

## Step 7. Connect Clay to HubSpot and write the answer back
**Why:** "A value in a Clay column doesn't mean HubSpot has it" (Lola). The write-back is where most people got stuck.
**Click by click:**
1. Clay > Settings > **Connections** > **Create** > search HubSpot > pick **HubSpot** (NOT "HubSpot (enrich-only)") >
   **Sign in with HubSpot** > choose the account **Akison** > **Choose Account** > tick the terms box > **Connect app**
   > name it `Johnson HubSpot (Akison, course)` > **Save**.
2. In the table: **Add enrichment** > HubSpot **Update object** (not "Update record", which is Salesforce):
   Account = my connection, Object Type = **Contact**, HubSpot Object ID = **HubSpot Record ID**, Ignore blank values
   ON, Map fields: **ICP fit** <- ICP Fit, **ICP reason** <- ICP Reason. Run only if the Record ID is filled.
3. Company half (for robot 2's deal size): **Look up object** (Company, filter domain = Company Domain, limit 1) >
   formula `HubSpot Company ID` (first result's id) > **Update object** (Company: Number of employees <- Employee
   Count, Name <- Company Name).
4. Table **Auto-run ON** with **Keep existing results** ticked. Unlike Week 2, this table must run by itself the moment
   HubSpot sends a contact.
**Check (Lola's method, never trust Clay's green tick alone):** made a test contact in HubSpot, sent its record ID
through the webhook, ran the row, then opened the contact in HubSpot: **ICP fit = Meets criteria, ICP reason = "US HQ,
56 staff, Specialty Trade Contractors"**. The company O'Connor Plumbing now shows **56** employees. Clay spend: 1 credit.
**Learned:** HubSpot links a new contact to its company from the email domain automatically, so Clay can find the
company by domain and update it.

## Step 8. Unlock webhooks (second free trial)
**Why:** the robot hands each new contact to Clay with **Send a webhook**. On Sales Hub Professional that action shows a
lock: it belongs to **Data Hub Professional**. The class guide doesn't say this.
**Click by click:** Workflows > any workflow > **+** > **Data ops** > **Send a webhook** (locked) > **Start 14-day trial**
> **Get started**. Free, no card, no auto-renewal; ends about 11 Dec (covers the capstone).
**Check:** the lock icons on Custom code / Format data / Send a webhook are gone.

## Step 9. Robot 1: "ServiceTitan - 1. Demo request to MQL"
**Why:** the graded part. A contact's stage changes because a rule says so, someone owns it, and there's a next step.
**How it was built:** I learned HubSpot's exact format for each step by adding it once in a test workflow (deleted
after), then created robot 1 through the workflows API, then opened it in the editor to check every step shows without
errors. Click path to rebuild it by hand:
1. Automation > **Workflows** > **Create workflow** > **From scratch** > Contact-based. Name it.
2. **Trigger:** "When an event occurs" > **Form submission** > form is **ServiceTitan demo request**. Re-enroll **off**.
3. **+ Branch** > "One property or action output" > **Lifecycle stage**: branch **= Customer** (existing-customer guard:
   rotate owner > task "Existing customer {first name} asked for a demo" > end). Default branch "New prospect" continues.
4. **+ Data ops > Send a webhook**: Method **POST**, URL = the Clay table's webhook URL, Authentication **None**,
   **Customize request body**: `hs_object_id` -> Record ID, `email` -> Email, `firstname` -> First Name, `website` ->
   Website URL. (Key spelling matters: Clay reads `{{Webhook}}.hs_object_id`.)
5. **+ Delay** > "Event occurs" > **Property value changed** > **ICP fit** is any of the 3 values. Tick **Customize the
   period**: 0 days 0 hours **15 minutes**.
6. **+ Branch** > "One property or action output" > **ICP fit** > tick **Create a separate branch for each value**. Rename
   "None met" to **Clay did not answer**.
   - **Meets criteria:** Edit record **Lifecycle stage = Marketing Qualified Lead** > **Rotate record to owner** (users:
     me; overwrite **off**) > **Create task**: title "Call {first name} about their ServiceTitan demo", type **Call**,
     due **1 day** at 9:00, **business days only**, priority High, assign to **An existing owner of the contact** >
     **Send internal email notification** to me: "New MQL: {first name} {last name}", body with the ICP reason.
   - **Doesn't meet criteria:** Edit record **Lead status = Unqualified**.
   - **Needs review:** nothing (the "Needs review" list catches it).
   - **Clay did not answer:** rotate owner > task "Clay check didn't come back for {email}" (fail loud, never silent).
7. **Review and turn on**.
**Check:** `screenshots/robot1-demo-request-to-mql.png`. Read back by API: ON, 14 steps, start = customer guard.
**Learned:** the MQL date is stamped by HubSpot itself ("Date entered Marketing Qualified Lead") when the lifecycle
changes, so no custom date step is needed. Tokens look like `{{ enrolled_object.firstname }}`.

## Step 10. Robot 2: "ServiceTitan - 2. SQL to deal" (built by clicking in the editor)
**Why:** joins lifecycle to the pipeline: when sales says a lead is real, the deal opens itself so nothing is lost.
**Click by click:**
1. Workflows > **Create workflow** > **From scratch** > Trigger **Met filter criteria** > record type **Contact** > **Add
   criteria** > **Lifecycle stage is any of Sales Qualified Lead** > Next > Re-enroll **off** > **Save and continue**.
   Rename (pencil next to the title) to "ServiceTitan - 2. SQL to deal".
2. **+ Rotate record to owner** (users: me, overwrite **off**): added after test 6 showed a hand-made SQL can arrive with
   no owner.
3. **+ Branch** > **AND/OR logic**:
   - Branch "Already has a deal": **Number of Associated Deals is greater than 0** -> end (no duplicate deals).
   - Branch "Small (10-24 staff)": Add criteria > click **Viewing** and switch to **Companies (associated)** > **Number
     of employees is between 10 and 24**. Same for "Mid (25-49 staff)" and "Large (50-99 staff)".
   - Rename "None met" to **Size unknown**.
4. In each size branch: **+ CRM > Create record > Deal**: assign to **An existing owner of the contact**; Deal name =
   token **Company name** (from "Company: Most recently updated") + " - ServiceTitan"; pipeline and stage **Discovery held
   (ServiceTitan)**; Amount **36000 / 80000 / 160000**; associations default (the contact + all associated companies).
   Then **+ Edit record > Lifecycle stage = Opportunity**.
5. **Size unknown:** Create record > Deal with **no amount** (named after the person) > **Create task** "Confirm
   technician count for this deal" (due 1 business day, to the contact owner) > Lifecycle = Opportunity.
6. **Review and turn on** > choose **"No, only enroll contacts who meet the criteria after the workflow is turned on"**
   (my June demo contacts must not get deals) > **Skip to turn on** > **Turn on workflow**.
**Check:** `screenshots/robot2-sql-to-deal.png`; test 5 made "O'Connor Plumbing - ServiceTitan" at $160,000.
**What went wrong:** HubSpot twice showed "Something went wrong while updating your workflow" on Save; saving again
worked. Clicking a category ("CRM") in the action list toggles it closed if it's already open.

## Step 11. Test everything (see `crm-design.md` section 1.6 for the full table)
**How:** submitted the real form 4 times (fake people, real company domains, nobody outside gets anything), then set
two contacts to SQL by hand, the way a rep would. Polled HubSpot until each contact had its answer.
**Result:** 6 of 6 behaved as designed after 2 fixes (deal name with no company; task with no owner). 4 Clay credits.

## Step 12. Share and clean up
- Gear > **Users & Teams** > **Add users** > **Create new** > the instructor's email (from the Set Up HubSpot guide), seat **View-Only Seat**,
  default permissions, **Send an email invite** ticked > **Add a user**. Shows as 1 Pending until she accepts.
- Deleted my test workflow ("ZZ probe") and the write-back test contact. Kept the 4 test contacts and 2 test deals as
  evidence; the second deal was renamed "Gina (W3 test) - ServiceTitan (size unknown)".

## Step 13. Dashboard "ServiceTitan - Week 3 CRM" (reports that answer a question)
**Why:** a report should answer a question, not just count things (class guide, Part 5).
**Click by click:** Reporting > Dashboards > dashboard name dropdown > **New dashboard** > **New dashboard** (from scratch)
> name `ServiceTitan - Week 3 CRM`, "Allow everyone to view" > **Create dashboard**. Then for each report: **Add content**
> **Create report with AI** (Breeze), type what you want, check the chart and its filter, **Save** > describe it > **Add to
existing dashboard** > pick the dashboard > **Save & add**. (The same reports can be built by hand in the custom report
builder.)
| Report | Question it answers | Filter / grouping | Result on test data |
|---|---|---|---|
| Demo requests by ICP fit | Is the rule letting the right people through? | Contacts, Lead channel = Demo request, by ICP fit | 1 Meets, 1 Doesn't meet, 1 Needs review, 1 no value (the existing customer, who skips Clay by design) |
| Demo requests by lifecycle stage | How many moved forward, and where are they now? | Same contacts, by Lifecycle stage | 1 Lead, 2 Opportunity, 1 Customer |
| ServiceTitan pipeline by stage | What is in the pipeline, and where? | Deals in the ServiceTitan pipeline, by Deal stage, sum of Amount + count | Discovery held: 2 deals, $160,000 |
**Check:** each AI-made report was compared against the test table in `crm-design.md` 1.6 before saving: every number
matched. `screenshots/dashboard-servicetitan-week3.png`.
**Learned:** Breeze filters on the property you name ("Lead channel is Demo request") correctly; always read its filter
and the bars against a record you know before trusting it.
