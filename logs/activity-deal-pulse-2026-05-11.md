---
agent: deal-pulse
run_at: 2026-05-11T16:49:00+02:00
status: success
version: 2.0
sources_read:
  - salesforce: 30 opportunities
  - google_calendar: 25 events (May 11-18, 2026)
  - granola: 62 meetings (last 30 days)
  - deal_pulse_rules: 7 active rules
  - deal_pulse_snapshots: 0 previous snapshots loaded
outputs_written:
  - /agent/home/deal_pulse_data.json: 30 deals (HOT:3 MED:9 COLD:18)
  - state/pipeline-state-latest.md: live pipeline state (GitHub)
  - state/current/pipeline-state-2026-05-11.md: dated snapshot (GitHub)
  - deal_pulse_snapshots: 30 snapshots stored
  - deal-pulse app: dashboard refreshed
errors: null
duration_estimate: medium
---
