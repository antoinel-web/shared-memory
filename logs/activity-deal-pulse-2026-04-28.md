---
agent: deal-pulse
run_at: 2026-04-28T07:30:00+02:00
status: success
sources_read:
  - salesforce: 29 opportunities
  - google_calendar: 33 events (7-day window)
  - granola: 54 meetings (30-day window)
  - deal_pulse_rules: 3 active rules
outputs_written:
  - /agent/home/deal_pulse_data.json: 29 deals, full pipeline rebuild
  - state/pipeline-state-latest.md: live pipeline state (GitHub)
  - state/current/pipeline-state-2026-04-28.md: dated snapshot (GitHub)
  - deal-pulse app: dashboard refreshed
errors: null
duration_estimate: medium
---
