---
agent: deal-pulse
run_at: 2026-07-21T07:30:00+02:00
status: success
version: "2.0"
sources_read:
  - salesforce: 25 opportunities
  - google_calendar: 24 events (Jul 21-28 2026)
  - granola: 17 meetings (last 30 days)
  - deal_pulse_rules: 7 active rules
  - deal_pulse_snapshots: 50 previous snapshots loaded
outputs_written:
  - /agent/home/deal_pulse_data.json: 25 deals (HOT:2 MED:9 COLD:14)
  - state/pipeline-state-latest.md: live pipeline state (GitHub)
  - state/current/pipeline-state-2026-07-21.md: dated snapshot (GitHub)
  - deal_pulse_snapshots: 25 snapshots stored
  - deal-pulse app: dashboard refreshed
errors: null
duration_estimate: medium
---
