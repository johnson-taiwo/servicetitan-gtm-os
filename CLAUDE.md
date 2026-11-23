# CLAUDE.md - ServiceTitan GTM Context OS

## Company
ServiceTitan (servicetitan.com): software that runs residential and commercial trade contractors (HVAC, plumbing,
electrical, roofing and more). Filled in after research: see context/docs/research/company.md.

## How this folder works
- `context/docs/research/` raw facts, one file per topic, every fact with a source and date. Read before any GTM task.
- `context/icp/` ICP, personas, signal matrix. Read before targeting, scoring or copy.
- `context/positioning/` positioning, value proposition, competitors. Read before messaging.
- `context/messaging/` core message, pillars, facts copy may use. Read before writing copy.
- `context/brand-kit/` tone of voice and key phrases. Read before writing copy.
- `context/docs/` GTM system architecture and case studies. Read before designing a workflow.
- `knowledge-base/playbooks/` method docs the skills read. Read when a skill points to them.
- `knowledge-base/learnings/` what worked and what didn't. Add a note after every build.
- `meetings/` call and class notes, one file per meeting.
- `output/campaigns/` one folder per workflow: spec, Clay build plan, results.
- `tools/` scripts and tools, each with its own README.
- `weekly-priorities/` one file per ISO week, linked to that week's work log.
- `work-log/` one file per ISO week, every change timestamped.

## Rules
- Cite a source (link and date) for every company fact. Mark our own reading INFERRED and first-hand experience
  "Source: practitioner".
- Never invent numbers, customers or quotes. If it can't be found, write "Not found".
- No em dashes in any copy.
- Ask before sending or publishing anything, and before any step that spends credits.
- No lead data, API keys or .env files in this repo.
- Log every change in this week's work log.

## Tools
- **Claude Code** in VS Code, with **GitHub** (gh CLI) for version control.
- **Clay** (Treford workspace): searches, enrichment, AI columns, routing. Rex (`tools/rex/`, unofficial) lets Claude work in it.
- **HubSpot** (week 3), **n8n** (week 4), a sequencer for sending (Smartlead or Clay Sequencer; nothing is sent from here).
- Free public data used for trade contractors: city permit portals, NOAA storm events, state licence boards, SBA loan data.
- Skills (`.claude/skills/`, from Bolaji Anifowose's GTM starter kit, licences in THIRD-PARTY.md):
  research to copy `/campaign-strategy` `/signal-matrix` `/build-workflow-spec` `/campaign-copy` `/miro-gtm-flowchart`;
  Clay `/clay-build-plan` `/build-clay` `/prompt-builder` `/campaign-contract` `/design-clay-table-json` `/import-clay-table`
  `/save-clay-schema` `/calculate-costs` `/workspace-cost-audit` `/export-tables` `/campaign-qa`;
  research and sending `/crawl4ai` `/agent-browser` `/inbox-placement-test`.
