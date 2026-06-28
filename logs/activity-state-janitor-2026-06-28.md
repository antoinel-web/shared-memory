---
agent: state-janitor
run_at: 2026-06-28T23:00:00+02:00
status: success
sources_read:
  - state/current: 2 files read
  - logs: listed (no digest files found)
  - activity reports: listed (last 7 days scanned)
  - ARCHITECTURE.md: read
outputs_written:
  - state/archive/2026-W26-pipeline-state-weekly.md: Weekly consolidation 2026-W26 (2 pipeline state files)
  - logs/activity-state-janitor-2026-06-28.md: this report
deleted:
  - state/current/pipeline-state-2026-06-23.md: consolidated into W26 weekly
  - state/current/pipeline-state-2026-06-26.md: consolidated into W26 weekly
registry_discrepancies:
  - ae-salesforce-operator: Active en registre mais aucune activité depuis 2026-04-28 → Inactive?
  - monday-digest: actif (dernier rapport 2026-06-22) mais absent du registre → Active (à ajouter)
errors: null
duration_estimate: medium
---
