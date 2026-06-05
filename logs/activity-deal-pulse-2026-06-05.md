---
agent: deal-pulse
run_at: "2026-06-05T07:30:00+02:00"
status: success
version: "2.0"
sources_read:
  - salesforce: 29 opportunities
  - google_calendar: 27 events (2026-06-05 to 2026-06-12)
  - granola: 27 meetings (last 30 days)
  - deal_pulse_rules: 7 active rules
  - deal_pulse_snapshots: 29 previous snapshots loaded
outputs_written:
  - /agent/home/deal_pulse_data.json: 29 deals (HOT:3 MED:7 COLD:19)
  - state/pipeline-state-latest.md: live pipeline state (GitHub)
  - state/current/pipeline-state-2026-06-05.md: dated snapshot (GitHub)
  - deal_pulse_snapshots: 29 snapshots stored
  - deal-pulse app: dashboard refreshed
errors: null
duration_estimate: medium
---
