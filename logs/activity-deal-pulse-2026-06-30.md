---
agent: deal-pulse
run_at: 2026-06-30T07:30:00+02:00
status: success
version: 2.0
sources_read:
  - salesforce: 24 opportunities
  - google_calendar: 27 events (2026-06-30 to 2026-07-07)
  - granola: 20 meetings (last 30 days)
  - deal_pulse_rules: 7 active rules
  - deal_pulse_snapshots: 44 previous snapshots loaded
outputs_written:
  - /agent/home/deal_pulse_data.json: 24 deals (HOT:3 MED:7 COLD:14)
  - state/pipeline-state-latest.md: live pipeline state (GitHub)
  - state/current/pipeline-state-2026-06-30.md: dated snapshot (GitHub)
  - deal_pulse_snapshots: 24 snapshots stored
  - deal-pulse app: dashboard refreshed
errors: null
duration_estimate: medium
---
