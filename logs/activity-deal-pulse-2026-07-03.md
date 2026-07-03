---
agent: deal-pulse
run_at: 2026-07-03T05:30:00+00:00
status: success
version: 2.0
sources_read:
  - salesforce: 25 opportunities
  - google_calendar: 24 events (Jul 3–10, 2026)
  - granola: 21 meetings (last 30 days)
  - deal_pulse_rules: 7 active rules
  - deal_pulse_snapshots: 22 previous snapshots loaded
outputs_written:
  - /agent/home/deal_pulse_data.json: 25 deals (HOT:1 MED:7 COLD:17)
  - state/pipeline-state-latest.md: live pipeline state (GitHub)
  - state/current/pipeline-state-2026-07-03.md: dated snapshot (GitHub)
  - deal_pulse_snapshots: 25 snapshots stored
errors: null
duration_estimate: medium
---
