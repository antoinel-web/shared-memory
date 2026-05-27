---
agent: deal-pulse
run_at: 2026-05-27T15:57:00+02:00
status: success
version: "2.0"
sources_read:
  - salesforce: 30 opportunities
  - google_calendar: 31 events (May 27 – Jun 3)
  - granola: 48 meetings (last 30 days)
  - deal_pulse_rules: 7 active rules
  - deal_pulse_snapshots: 30 previous snapshots loaded
outputs_written:
  - /agent/home/deal_pulse_data.json: 30 deals (HOT:3 MED:8 COLD:19)
  - state/pipeline-state-latest.md: live pipeline state (GitHub)
  - state/current/pipeline-state-2026-05-27.md: dated snapshot (GitHub)
  - deal_pulse_snapshots: 30 snapshots stored
  - deal-pulse app: dashboard refreshed
errors: null
duration_estimate: medium
---
