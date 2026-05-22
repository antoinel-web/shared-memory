---
agent: follow-up-forge-v2
run_at: 2026-05-22T08:00:00+02:00
status: success
run_id: 17
sources_read:
  deal_pulse_github: 29 deals parsed
  salesforce: queried per deal (7-8 deals)
  gmail: searched per deal (10 deals processed)
  granola: queried per deal (multiple meetings retrieved)
  slack: queried per deal (internal channels)
  calendar: 1 OOO check + per-deal meeting checks
  bigquery: FAILED (apex_for_statistics not found in europe-west3, known issue)
outputs_written:
  gmail_drafts: 10 drafts created
  run_log: logged to DB run_log id=17
  learning_params: no changes (no new feedback replies)
  draft_log_github: logs/drafts/forge-v2-2026-05-22.md (10 entries appended by subagents)
errors:
  bigquery: table not found (known ongoing issue)
  bnl_thread_reply_blocked: 2 ghost draft replies blocking thread reply creation
duration_estimate: heavy
notes:
  deals_processed: 13
  drafts_created: 10
  deals_stopped: 1 (Credit Agricole - Meeting Echo active)
  deals_skipped: 5 (3 meetings today, 2 min-gap constraints)
  deals_divergent: 3 (Belfius channel pivot MAJOR, Banca Sella contact email, BPCE last interaction stale)
  cap_reached: true
---
