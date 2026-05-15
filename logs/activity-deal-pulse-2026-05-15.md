---
agent: deal-pulse
run_at: "2026-05-15T07:37:00+02:00"
status: success
version: "2.0"
sources_read:
  - salesforce: 29 opportunities
  - google_calendar: 30 events (May 15-22)
  - granola: 57 meetings (last 30 days)
  - deal_pulse_rules: 7 active rules
  - deal_pulse_snapshots: 0 previous snapshots loaded
outputs_written:
  - /agent/home/deal_pulse_data.json: 29 deals (HOT:4 MED:9 COLD:16)
  - state/pipeline-state-latest.md: live pipeline state (GitHub)
  - state/current/pipeline-state-2026-05-15.md: dated snapshot (GitHub)
  - deal_pulse_snapshots: 29 snapshots stored
  - deal-pulse app: dashboard refreshed
errors: null
duration_estimate: medium
---
