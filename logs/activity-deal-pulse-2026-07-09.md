---
agent: deal-pulse
run_at: 2026-07-09T16:34:06.513789+00:00
status: success
version: "2.0"
sources_read:
  - salesforce: 25 opportunities
  - google_calendar: 33 events (2026-07-09 to 2026-07-16)
  - granola: 19 meetings (last 30 days)
  - deal_pulse_rules: 7 active rules
  - deal_pulse_snapshots: 47 previous snapshots loaded
outputs_written:
  - /agent/home/deal_pulse_data.json: 25 deals (HOT:3 MED:11 COLD:11)
  - state/pipeline-state-latest.md: live pipeline state (GitHub)
  - state/current/pipeline-state-2026-07-09.md: dated snapshot (GitHub)
  - deal_pulse_snapshots: 25 snapshots stored
errors: null
duration_estimate: medium
---
