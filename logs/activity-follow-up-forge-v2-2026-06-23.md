---
agent: follow-up-forge-v2
run_at: 2026-06-23T08:00:00+02:00
run_id: 26
status: success
sources_read:
  deal_pulse_github: 24 deals (published_at 2026-06-23T07:30:00+0200, fresh, 30min old)
  gmail: queried for Bunq, La Banque Postale
  granola: queried for Bunq
  google_calendar: queried for Bunq, La Banque Postale
  salesforce: not queried (gmail_contact_priority_over_sf)
  slack: not queried this run
  bigquery: FAILED (apex_for_statistics not found in europe-west3 - persistent)
  calendar_ooo: SKIPPED (calendar_list_events not available on conn_tpfxd4qqwq1f23rsxzqt - persistent)
outputs_written:
  gmail_drafts: 1 draft validated (La Banque Postale - pre-existing Jun 18 draft)
  run_log: logged to DB run_log id=26
  learning_params: no change (20 pending feedback records with null rejection_axis)
  divergence_notifications: 2 (Bunq email_out stale, SSP deal rejected but active in DP)
  draft_log: 1 entry in logs/drafts/forge-v2-2026-06-23.md (pushed by subagent)
errors:
  bigquery: apex_for_statistics not found in europe-west3 (persistent)
  calendar_ooo: calendar_list_events not available on conn_tpfxd4qqwq1f23rsxzqt (persistent)
  learning_loop_feedback: 20 pending feedback records with null rejection_axis - stalled
  bnp_paribas_draft_rejected: draft r-2715319952461502155 still in Gmail after 4 days, feedback email sent
duration_estimate: light
deals_dispatched: 2 (Bunq, La Banque Postale)
deals_drafted: 1 (La Banque Postale)
deals_blocked_by_cadence: 1 (Bunq - last email Jun 22, next window Jul 6)
deals_stopped: 5 (Swan STOP-2 Jun24, BNP Paribas STOP-2 Jun23, Deblock STOP-2 Jun25/29, Intesa STOP-2 Jun24, BPCE STOP-2 Jun23)
deals_divergent: 2 (Bunq email_out stale, SSP deal rejected)
deals_skipped: 16 (CGD gap 4d, 15 COLD)
---
