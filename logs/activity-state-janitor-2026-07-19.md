---
agent: state-janitor
run_at: 2026-07-19T23:00:00+02:00
status: success
sources_read:
  - state/current: 2 files read
  - logs: directory listed (no digest files found)
  - activity reports: 18 files read (last 7 days)
  - ARCHITECTURE.md: read
outputs_written:
  - state/archive/2026-W29-pipeline-state-weekly.md: weekly pipeline consolidation (2 sources, 25 deals)
  - logs/activity-state-janitor-2026-07-19.md: this report
errors: null
duration_estimate: medium
---

### Run summary

- Schedule: weekly (2026-W29 — 2026-07-13 to 2026-07-19)
- Pipeline state files consolidated: 2 (pipeline-state-2026-07-14.md, pipeline-state-2026-07-17.md)
- Digest files: 0 found — no logs-weekly summary produced
- Orphan files: 0 (all files were in current week W29)
- Source files deleted: 2 (after successful verification)
- Registry discrepancies: 5 flagged (2 new agents, 3 potentially inactive) — Tasklet task created (at_xqt20ktkprcaw9fj9q0e)

### Registry scan (W29)

**New agents detected (not in ARCHITECTURE.md registry):**
- `bluedot-logger` — active reports: 2026-07-13, 2026-07-17
- `monday-digest` — active report: 2026-07-13

**Registered as Active but no activity in last 7 days:**
- `meeting-echo` — no activity log in window 2026-07-13 to 2026-07-19
- `follow-up-forge-v2` — no activity log in window (most recent found: 2026-06-05)
- `ae-salesforce-operator` — no activity log in window (most recent found: 2026-04-28)
