---
agent: deal-pulse
run_at: 2026-06-02T07:30:00+02:00
status: success
version: 2.0
sources_read:
  - salesforce: 29 opportunities
  - google_calendar: 33 events (Jun 2–9, 2026)
  - granola: 34 meetings (last 30 days)
  - deal_pulse_rules: 7 active rules
  - deal_pulse_snapshots: 29 previous snapshots loaded
outputs_written:
  - /agent/home/deal_pulse_data.json: 29 deals (HOT:3 MED:8 COLD:18)
  - state/pipeline-state-latest.md: live pipeline state (GitHub)
  - state/current/pipeline-state-2026-06-02.md: dated snapshot (GitHub)
  - deal_pulse_snapshots: 29 snapshots stored
errors: null
duration_estimate: medium
---
