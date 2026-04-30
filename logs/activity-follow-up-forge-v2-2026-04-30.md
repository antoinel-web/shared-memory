---
agent: follow-up-forge-v2
run_at: 2026-04-30T08:18:00+02:00
status: failure
sources_read:
  - deal_pulse_github: ABORTED (published_at 2026-04-28T07:30:00 — 48h stale, threshold 24h)
  - salesforce: not reached
  - gmail: not reached
  - granola: not reached
  - slack: not reached
  - calendar: 9 events checked (OOO check passed)
  - bigquery: not reached
outputs_written:
  - gmail_drafts: 0 drafts created
  - run_log: logged to DB run_log id=7
  - learning_params: no change
errors: deal_pulse_stale — published_at 2026-04-28T07:30:00 is 48h old (max 24h). Alert email sent to owner.
duration_estimate: light
---
