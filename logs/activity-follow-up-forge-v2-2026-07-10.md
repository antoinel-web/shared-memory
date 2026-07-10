---
agent: follow-up-forge-v2
run_at: 2026-07-10T08:00:00+02:00
status: success
sources_read:
  deal_pulse_github: 25 deals parsed (published_at 2026-07-10T05:41:24Z, fresh ~2h20)
  salesforce: not queried (contacts resolved via Gmail/Granola)
  gmail: searched for 4 active deals (CGD, Bunq, BIL, Spuerkeess)
  granola: queried for BIL (granola_bil_poc_debrief_2026-07-03 found)
  slack: queried for Bunq (DEV-4684, new-tenants-prod) + BIL + Spuerkeess
  calendar: checked OOO (clear) + deal meetings (CGD Jul 16, Spuerkeess Jul 17, ICCREA Jul 9 echo)
  bigquery: FAILED (apex_for_statistics not found in europe-west3 - ongoing)
outputs_written:
  gmail_drafts: 3 drafts created (Bunq delivery_confirmation EN, BIL bridge_handoff FR, Spuerkeess bridge_handoff FR)
  run_log: logged to DB run_log id=31
  draft_log_github: logs/drafts/forge-v2-2026-07-10.md (commits 8beb6dec + 723944fc + 0ce01f82)
  learning_params: no changes this run
stops:
  - CGD: STOP-1 (inbound reply Jul 9 from champion - substantive scope pivot)
  - IntesaSanpaolo: STOP-2 (meeting Jul 15 in 2 business days)
  - ICCREA: Meeting Echo active (Jul 9 meeting, 5-day wait until Jul 16)
  - BPCE: Meeting Echo active (Jul 6, 5-day wait until Jul 13)
skips:
  - BNP Paribas: freeze_rule
  - La Banque Postale: no_checkin_during_pilot
  - Deblock: min_inter_email_gap_14d (eligible Jul 15)
  - Swan: COLD min 20d
  - 15 COLD deals: no signal trigger
divergences:
  - CGD: last_interactions null in Deal Pulse, actual email_in 2026-07-09, email_out 2026-07-02
  - Bunq: last_interactions null, actual email_out 2026-07-08, email_in 2026-05-15
  - BIL: new deal, contacts identified (not in Deal Pulse yet)
errors: BigQuery apex_for_statistics not found in europe-west3 (ongoing systemic issue)
duration_estimate: medium
---
