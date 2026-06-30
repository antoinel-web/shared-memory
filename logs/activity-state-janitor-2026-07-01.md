---
agent: state-janitor
run_at: 2026-07-01T01:00:00+02:00
status: success
schedule: monthly
sources_read:
  - state/archive: 7 weekly summary files read (W23×2, W24×2, W25×2, W26×1)
  - monthly_target: June 2026 (2026-06)
outputs_written:
  - state/archive/2026-06-pipeline-state-monthly.md: monthly pipeline consolidation (W23+W24+W25+W26, 26 deals, period 2026-06-01 to 2026-06-28)
  - state/archive/2026-06-logs-monthly.md: monthly logs consolidation (W23+W24+W25, period 2026-06-01 to 2026-06-21)
files_deleted:
  - state/archive/2026-W23-pipeline-state-weekly.md
  - state/archive/2026-W23-logs-weekly.md
  - state/archive/2026-W24-pipeline-state-weekly.md
  - state/archive/2026-W24-logs-weekly.md
  - state/archive/2026-W25-pipeline-state-weekly.md
  - state/archive/2026-W25-logs-weekly.md
  - state/archive/2026-W26-pipeline-state-weekly.md
purge_90_days: no monthly summaries older than 90 days (oldest: 2026-04, ended 2026-04-30 = 62 days ago)
orphan_weeklies_noted:
  - state/archive/2026-W13-logs-weekly.md (March 2026, no monthly summary for March — not in scope of this run)
  - state/archive/2026-W17-logs-weekly.md (April 2026, April monthly already exists — W17 may be redundant)
errors: null
duration_estimate: light
---
