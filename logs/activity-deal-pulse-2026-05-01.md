---
agent: deal-pulse
run_at: 2026-05-01T07:31:00+02:00
status: success
sources_read:
  - salesforce: 29 opportunities
  - google_calendar: 32 events (2026-05-01 to 2026-05-08)
  - granola: 67 meetings (last 30 days)
  - deal_pulse_rules: 3 active rules
outputs_written:
  - /agent/home/deal_pulse_data.json: 29 deals, full pipeline rebuild
  - state/pipeline-state-latest.md: live pipeline state (GitHub)
  - state/current/pipeline-state-2026-05-01.md: dated snapshot (GitHub)
  - deal-pulse app: dashboard refreshed
errors: null
duration_estimate: medium
---
