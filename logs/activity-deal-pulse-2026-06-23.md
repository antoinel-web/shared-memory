---
agent: deal-pulse
run_at: 2026-06-23T07:30:00+02:00
status: success
version: "2.0"
sources_read:
  - salesforce: 24 opportunities
  - google_calendar: 30 events (Jun 23–30)
  - granola: 22 meetings (last 30 days)
  - deal_pulse_rules: 7 active rules
  - deal_pulse_snapshots: 27 previous snapshots loaded
outputs_written:
  - /agent/home/deal_pulse_data.json: 24 deals (HOT:3 MED:6 COLD:15)
  - state/pipeline-state-latest.md: live pipeline state (GitHub)
  - state/current/pipeline-state-2026-06-23.md: dated snapshot (GitHub)
  - deal_pulse_snapshots: 24 snapshots stored
errors: null
duration_estimate: medium
---
