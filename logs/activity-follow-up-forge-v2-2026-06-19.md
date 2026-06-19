---
agent: follow-up-forge-v2
run_at: 2026-06-19T08:00:00+02:00
run_id: 25
status: success
sources_read:
  deal_pulse_github: 22 deals (published_at 2026-06-19T07:34:31+0200, fresh, 26min old)
  gmail: queried for 10 dispatched deals
  granola: queried for Swan, Bunq, CGD
  google_calendar: queried for Swan, BNP Paribas, Intesa, Deblock, BPCE
  salesforce: not queried this run (gmail_contact_priority_over_sf)
  slack: queried for Swan, Bunq, La Banque Postale, BPCE
  bigquery: FAILED (apex_for_statistics not found in europe-west3 - persistent)
outputs_written:
  gmail_drafts: 3 drafts created (Swan, BNP Paribas, CGD)
  run_log: logged to DB run_log id=25
  learning_params: no change (67 pending feedback records with null rejection_axis)
  divergence_notifications: sent for Bunq, La Banque Postale, SSP (critical - deal rejected)
  feedback_email: sent for 2 rejected drafts from run Jun 16 (BNP Paribas draft deleted, Bunq draft still present)
  draft_log: 3 entries in logs/drafts/forge-v2-2026-06-19.md (pushed by subagents)
errors:
  bigquery: apex_for_statistics not found in europe-west3 (persistent)
  calendar_ooo: calendar_list_events not available on conn_tpfxd4qqwq1f23rsxzqt - OOO check skipped
  learning_loop: feedback loop stalled - 67 unanswered feedback emails, no rejection axes
  swan_language_mismatch: Deal Pulse language=EN but comms are FR - corrected in draft
duration_estimate: heavy
deals_dispatched: 10
deals_drafted: 3 (Swan, BNP Paribas, CGD)
deals_stopped: 5 (Deblock STOP-2, Intesa STOP-2, Banca Profilo STOP-2, BPCE STOP-2, SSP STOP-1)
deals_divergent: 3 (Bunq wrong champion, La Banque Postale wrong email_out, SSP deal rejected/dead)
deals_skipped: 12 (COLD tier)
---
