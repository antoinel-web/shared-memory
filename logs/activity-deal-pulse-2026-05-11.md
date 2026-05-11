---
agent: deal-pulse
run_at: 2026-05-11T15:43:00+02:00
status: success
sources_read:
  - salesforce: 30 opportunities
  - google_calendar: 27 events (2026-05-11 to 2026-05-18)
  - granola: 62 meetings (last 30 days)
  - deal_pulse_rules: 3 active rules
outputs_written:
  - /agent/home/deal_pulse_data.json: 30 deals, full pipeline rebuild
  - state/pipeline-state-latest.md: live pipeline state (GitHub)
  - state/current/pipeline-state-2026-05-11.md: dated snapshot (GitHub)
  - deal-pulse app: dashboard refreshed
errors: null
duration_estimate: medium
---
