---
agent: deal-pulse
run_at: 2026-05-08T07:37:00+02:00
status: success
sources_read:
  - salesforce: 30 opportunities
  - google_calendar: 25 events (May 8-15, 2026)
  - granola: 72 meetings (last 30 days)
  - deal_pulse_rules: 3 active rules
outputs_written:
  - /agent/home/deal_pulse_data.json: 30 deals, full pipeline rebuild
  - state/pipeline-state-latest.md: live pipeline state (GitHub)
  - state/current/pipeline-state-2026-05-08.md: dated snapshot (GitHub)
  - deal-pulse app: dashboard refreshed
errors: null
duration_estimate: medium
---
