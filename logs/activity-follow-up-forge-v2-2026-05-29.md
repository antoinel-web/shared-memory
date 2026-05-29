---
agent: follow-up-forge-v2
run_at: 2026-05-29T08:00:00+02:00
run_id: 19
status: success
sources_read:
  deal_pulse_github: 31 deals parsed (published_at 2026-05-29T07:30:00+02:00, 30min fresh)
  salesforce: queried per deal via HTTP (multiple deals, some SF IDs missing in Deal Pulse)
  gmail: searched per deal (all processed deals)
  granola: queried per deal (multiple meetings retrieved)
  slack: queried per deal (internal channels)
  calendar: OOO check (no OOO found) + per-deal meeting checks
  bigquery: FAILED (apex_for_statistics not found in europe-west3, ongoing known issue)
outputs_written:
  gmail_drafts: 6 drafts created
  run_log: logged to DB run_log id=19
  learning_params: no changes (learning loop ran, no new feedback axes received)
  draft_log_github: logs/drafts/forge-v2-2026-05-29.md (entries by subagents per deal created)
errors:
  bigquery: table not found (known ongoing issue)
  learning_loop: 3 rejected drafts from May 26 (Treezor, Lydia, Credem), 6 sent-edited (full rewrites by Antoine)
duration_estimate: heavy
notes:
  deals_processed: 31
  drafts_created: 6
  deals_stopped: 5 (Incore STOP-2 meeting today, UniCredit STOP-2 meeting today, Belfius STOP-3 meeting May 28, Swan STOP-1 prospect replied 14h ago, Banca Profilo STOP-3 5-day window)
  deals_skipped: 15 (BNL no contacts, KBC breakup rule, BPCE/LBP cadence June 5, Dukascopy cadence June 15, Deblock cadence June 4, SSP internal sync, Poste Italiane/CCF/OuiTrust/Swissquote/Nickel/Qonto/Banca Sella/SocGen cold no signal)
  deals_divergent: 5 deals with Deal Pulse data gaps (BNP Fortis, CaixaGeral, Swan, Banca Profilo minor, BPER minor)
  learning_loop_finding: all 6 sent drafts from May 26 were fully rewritten by Antoine (structural pattern)
  cap_reached: false (6 of 10)
  next_run: 2026-06-02 (Tuesday)
---
