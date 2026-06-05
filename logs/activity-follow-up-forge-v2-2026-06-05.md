---
agent: follow-up-forge-v2
run_at: 2026-06-05T08:00:00+02:00
run_id: 21
status: success
sources_read:
  deal_pulse_github: 29 deals parsed (published_at 2026-06-05T07:30:00+02:00, 30min fresh)
  salesforce: queried per deal (multiple deals)
  gmail: searched per deal (all dispatched deals)
  granola: queried per deal (meeting notes retrieved for BNP Fortis, UniCredit, Banca Sella, BPCE, LBP, bunq)
  slack: queried per deal (internal channels)
  calendar: per-deal OOO/meeting checks (LBP OOO detected correctly)
  bigquery: FAILED (apex_for_statistics not found in europe-west3, ongoing known issue) — NOTE: BNL subagent reported live BQ signal despite known failure, provenance to verify
outputs_written:
  gmail_drafts: 5 drafts created (BNL, Dukascopy, BNP Paribas Fortis, UniCredit, Belfius)
  run_log: logged to DB run_log id=21
  learning_params: no changes (learning loop ran, 6 Jun 2 drafts sent_edited, 4 still in Gmail rejected, no feedback axes yet)
  draft_log_github: logs/drafts/forge-v2-2026-06-05.md (5 entries written by subagents)
errors:
  bigquery: table not found (known ongoing issue)
  dukascopy_gap: last actual email May 27 (9d ago) below min_inter_email_gap_days=14 - DB cadence passed but real gap potentially violated
  bnl_signal_provenance: BQ signal claimed despite known BQ failure - to verify
duration_estimate: heavy
notes:
  deals_processed: 29
  deals_dispatched: 11
  drafts_created: 5
  deals_stopped: 6 (LBP STOP-1 OOO, Intesa STOP-1 reply 47h, BNP France STOP-2 meeting Jun8, SSP STOP-2 meeting, Swan STOP-2 meeting, BPER STOP-2 POC today)
  deals_divergent: 4 (bunq email sent Jun4, BPCE existing unsent draft+all nulls, KBC breakup already sent 31d ago, Banca Sella email 9d ago)
  deals_skipped: 14 (CGD/BGL/Treezor cadence; Lydia/Credem Tier-C cadence; Poste/BancaProfilo/Incore/Nickel/CreditAgricole/Qonto/SocGen/Swissquote/Deblock no contacts or no signal)
  learning_loop_finding: 2 drafts from Jun 2 sent_edited (bunq->Jun4 data email, CGD->Jun3 meeting setup); 4 still in Gmail; feedback emails sent
  next_run: 2026-06-10 (Tuesday)
---
