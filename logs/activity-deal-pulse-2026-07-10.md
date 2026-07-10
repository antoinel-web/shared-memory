---
agent: deal-pulse
run_at: 2026-07-10T07:30:00+02:00
status: success
version: "2.0"
sources_read:
  - salesforce: 25 opportunities
  - google_calendar: 34 events (2026-07-10 to 2026-07-17)
  - granola: 19 meetings (last 30 days)
  - deal_pulse_rules: 7 active rules
  - deal_pulse_snapshots: 24 previous snapshots loaded
outputs_written:
  - /agent/home/deal_pulse_data.json: 25 deals (HOT:2 MED:8 COLD:15)
  - state/pipeline-state-latest.md: live pipeline state (GitHub)
  - state/current/pipeline-state-2026-07-10.md: dated snapshot (GitHub)
  - deal_pulse_snapshots: 25 snapshots stored
errors: null
duration_estimate: medium
---
