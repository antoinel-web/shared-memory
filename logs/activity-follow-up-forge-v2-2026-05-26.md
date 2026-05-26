---
agent: follow-up-forge-v2
run_at: 2026-05-26T08:00:00+02:00
status: success
run_id: 18
sources_read:
  deal_pulse_github: 30 deals parsed (published_at 2026-05-26T07:30:00+02:00, fresh)
  salesforce: queried per deal via HTTP (multiple deals)
  gmail: searched per deal (all 30 deals processed)
  granola: queried per deal (multiple meetings retrieved)
  slack: queried per deal (internal channels)
  calendar: OOO check + per-deal meeting checks (multiple)
  bigquery: FAILED (apex_for_statistics not found in europe-west3, ongoing known issue)
outputs_written:
  gmail_drafts: 8 drafts created
  run_log: logged to DB run_log id=18
  learning_params: no changes (no new feedback replies from Antoine)
  draft_log_github: logs/drafts/forge-v2-2026-05-26.md (entries appended by subagents per deal)
errors:
  bigquery: table not found (known ongoing issue)
  run_log_update: minor SQL syntax error on first attempt, resolved on retry
  learning_loop: all 10 May22 drafts marked rejected (never sent by Antoine)
duration_estimate: heavy
notes:
  deals_processed: 30
  drafts_created: 8
  deals_stopped: 7 (BNP Paribas STOP-3, Belfius STOP-2, UniCredit STOP-2, Incore STOP-2, BNP Paribas Fortis STOP-3, Swan STOP-3, Banca Profilo STOP-3)
  deals_skipped: 13 (cadence constraints, no contacts, internal sync, existing drafts)
  deals_divergent: 2 (BPER Banca active deal invisible in Deal Pulse, KBC breakup already spent)
  cap_reached: false (8 of 10)
  pipeline_observations: 7 deals in Meeting Echo window (heavy meeting week), multiple cold deals lack contacts in Deal Pulse
---
