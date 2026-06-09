---
agent: deal-pulse
run_at: 2026-06-09T07:30:00+02:00
status: success
version: 2.0
sources_read:
  - salesforce: 27 opportunities
  - google_calendar: 30 events (2026-06-09 to 2026-06-16)
  - granola: 26 meetings (last_30_days)
  - deal_pulse_rules: 7 active rules
  - deal_pulse_snapshots: 120 previous snapshots loaded
outputs_written:
  - /agent/home/deal_pulse_data.json: 27 deals (HOT:1 MED:8 COLD:18)
  - state/pipeline-state-latest.md: live pipeline state (GitHub)
  - state/current/pipeline-state-2026-06-09.md: dated snapshot (GitHub)
  - deal_pulse_snapshots: 27 snapshots stored
  - deal-pulse app: dashboard refreshed
errors: null
duration_estimate: medium
---
