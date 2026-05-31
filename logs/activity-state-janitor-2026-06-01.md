---
agent: state-janitor
run_at: 2026-06-01T01:00:00+02:00
status: success
sources_read:
  - state/archive: 10 weekly files read (W18–W22, pipeline + logs)
  - ARCHITECTURE.md: not read (monthly run — registry scan not executed)
outputs_written:
  - state/archive/2026-05-pipeline-state-monthly.md: Monthly pipeline consolidation May 2026 (5 weekly sources, 17 active deals, 14 exits)
  - state/archive/2026-05-logs-monthly.md: Monthly logs consolidation May 2026 (5 weekly sources, 26 digests aggregated)
files_deleted:
  - state/archive/2026-W18-pipeline-state-weekly.md
  - state/archive/2026-W18-logs-weekly.md
  - state/archive/2026-W19-pipeline-state-weekly.md
  - state/archive/2026-W19-logs-weekly.md
  - state/archive/2026-W20-pipeline-state-weekly.md
  - state/archive/2026-W20-logs-weekly.md
  - state/archive/2026-W21-pipeline-state-weekly.md
  - state/archive/2026-W21-logs-weekly.md
  - state/archive/2026-W22-pipeline-state-weekly.md
  - state/archive/2026-W22-logs-weekly.md
purge_90_days: no monthly summaries older than 90 days found (cutoff: 2026-03-03)
errors: null
duration_estimate: light
---
