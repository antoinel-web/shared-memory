---
agent: deal-pulse
run_at: 2026-05-26T14:03:00+02:00
status: success
version: 2.0
sources_read:
  - salesforce: 30 opportunities
  - google_calendar: 29 events (May 26 – Jun 2)
  - granola: 49 meetings (last 30 days)
  - deal_pulse_rules: 7 active rules
  - deal_pulse_snapshots: 30 previous snapshots loaded
outputs_written:
  - /agent/home/deal_pulse_data.json: 30 deals (HOT:2 MED:8 COLD:20)
  - state/pipeline-state-latest.md: live pipeline state (GitHub)
  - state/current/pipeline-state-2026-05-26.md: dated snapshot (GitHub)
  - deal_pulse_snapshots: 30 snapshots stored
  - deal-pulse app: dashboard refreshed
errors: null
duration_estimate: medium
---
