---
agent: follow-up-forge-v2
run_at: 2026-05-15T08:03:00+02:00
status: success
sources_read:
  deal_pulse_github: 29 deals
  salesforce: queried per deal
  gmail: queried per deal
  granola: queried per deal
  slack: queried per deal
  calendar: 5 events checked (OOO + deal meetings)
  bigquery: FAILED (apex_for_statistics not found — ongoing)
outputs_written:
  gmail_drafts: 9 drafts created
  run_log: logged to DB run_log id=15
  learning_params: no change (0 new rules)
  draft_logs: appended to logs/drafts/forge-v2-2026-05-15.md
errors:
  - bigquery table not found (apex_for_statistics, ongoing since run 1)
  - deal_pulse last_interactions all null (ongoing)
  - belfius deal status divergence (pivoted to Isabel Group track, Deal Pulse not updated)
duration_estimate: heavy
---
