---
agent: deal-pulse
run_at: "2026-07-14T07:30:00+02:00"
status: success
version: 2.0
sources_read:
  - salesforce: 25 opportunities
  - google_calendar: 40 events (2026-07-14 to 2026-07-21)
  - granola: 19 meetings (2026-06-15 to 2026-07-13)
  - deal_pulse_rules: 7 active rules
  - deal_pulse_snapshots: 24 previous snapshots loaded
outputs_written:
  - /agent/home/deal_pulse_data.json: 25 deals (HOT:5 MED:7 COLD:13)
  - state/pipeline-state-latest.md: live pipeline state (GitHub)
  - state/current/pipeline-state-2026-07-14.md: dated snapshot (GitHub)
  - deal_pulse_snapshots: 25 snapshots stored
errors: null
duration_estimate: medium
---
