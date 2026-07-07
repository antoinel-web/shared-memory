---
agent: deal-pulse
run_at: 2026-07-07T07:30:00+02:00
status: success
version: 2.0
sources_read:
  - salesforce: 25 opportunities
  - google_calendar: 30 events (2026-07-07 to 2026-07-14)
  - granola: 18 meetings (last_30_days)
  - deal_pulse_rules: 7 active rules
  - deal_pulse_snapshots: 24 previous snapshots loaded
outputs_written:
  - /agent/home/deal_pulse_data.json: 25 deals (HOT:3 MED:5 COLD:17)
  - state/pipeline-state-latest.md: live pipeline state (GitHub)
  - state/current/pipeline-state-2026-07-07.md: dated snapshot (GitHub)
  - deal_pulse_snapshots: 25 snapshots stored
  - deal-pulse app: dashboard refreshed
errors: null
duration_estimate: medium
---
