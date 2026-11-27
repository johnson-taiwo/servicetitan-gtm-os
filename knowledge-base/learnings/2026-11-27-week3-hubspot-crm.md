# Learning: HubSpot lifecycle workflows with a Clay check (Week 3, 27 Nov 2026)

- **Webhooks need Data Hub Professional,** not Sales Hub Professional. Start both free trials before building.
- **The trigger must match how the record arrives:** form submission for a form, property changed for Clay,
  segment membership for an import.
- **Never trust Clay's "Object updated".** Open the HubSpot record and check the property has the value.
- **A record can skip a stage.** Test 6 sent a "Needs review" contact straight to SQL; it had no owner, and its deal
  had no name (no company). Fix: every workflow that creates work makes sure an owner exists first, and names deals
  from something every record has.
- **Guard existing customers first,** or their demo requests get scored as new leads.
- **Rule for next time:** write the expected result for each test before running it, and include one edge case
  per branch.
