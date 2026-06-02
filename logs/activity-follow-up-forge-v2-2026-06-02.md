---
agent: follow-up-forge-v2
run_at: 2026-06-02T08:00:00+02:00
run_id: 20
status: success
sources_read:
  deal_pulse_github: 29 deals parsed (published_at 2026-06-02T07:35:04+02:00, 25min fresh)
  salesforce: queried per deal (multiple deals)
  gmail: searched per deal (all processed deals)
  granola: queried per deal (meeting notes retrieved for multiple deals)
  slack: queried per deal (internal channels)
  calendar: per-deal meeting checks (OOO check skipped — calendar_list_events unavailable on configured connection)
  bigquery: FAILED (apex_for_statistics not found in europe-west3, ongoing known issue)
outputs_written:
  gmail_drafts: 5 drafts created (bunq, BNP Paribas Fortis, BNL, Swan, CaixaGeral de Depositos)
  run_log: logged to DB run_log id=20
  learning_params: no changes (learning loop ran, 6 May 29 drafts all rejected, awaiting feedback axes)
  draft_log_github: logs/drafts/forge-v2-2026-06-02.md (5 entries appended by subagents)
errors:
  bigquery: table not found (known ongoing issue)
  calendar_ooo: calendar_list_events unavailable on conn_tpfxd4qqwq1f23rsxzqt — OOO check skipped
duration_estimate: heavy
notes:
  deals_processed: 29
  drafts_created: 5
  deals_stopped: 9 (BNP Paribas STOP-1 reply, SSP/Intesa/BPER/BancaProfilo STOP-2 upcoming meetings, Belfius/UniCredit/Incore STOP-3 echo windows, KBC breakup rule)
  deals_skipped: 14 (cadence: LBP/BPCE; no contacts: CreditAgricole/Treezor/Dukascopy/Lydia/BancaSella/PosteItaliane/Nickel/Deblock/SocGen/Swissquote/Qonto/Credem)
  deals_divergent: 5 (BGL POC May22 invisible, BNP Paris reply Jun1, SSP wrong domain, BNL wrong domain, CGD wrong language)
  learning_loop_finding: all 6 May29 drafts rejected — structural rewrite pattern persists
  cap_reached: false (5 of 10)
  next_run: 2026-06-05 (Friday)
---
