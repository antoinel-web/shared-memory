---
agent: follow-up-forge-v2
run_at: 2026-07-07T08:00:00+02:00
status: success
sources_read:
  deal_pulse_github: 25 deals parsed (published_at 2026-07-07T05:38:12+00:00, fresh — ~2h20 old)
  salesforce: queried via deal processor (Bunq)
  gmail: searched per deal (1 processor dispatched)
  granola: queried (granola_meeting_bunq_20260506 found — signal used)
  slack: queried (Jira AO-738 tenant request found — signal used)
  calendar: checked for STOP-2 (ICCREA meeting Jul 9 confirmed)
  bigquery: not queried (no deal requiring BigQuery signal this run)
outputs_written:
  gmail_drafts: 1 draft created (Bunq — bridge_handoff, EU IBAN list delivery Jul 10)
  run_log: logged to DB run_log id=30
  draft_log_github: logs/drafts/forge-v2-2026-07-07.md (written by deal processor, commit 9eef2f42)
  learning_params: no changes (learning loop — no threshold rules triggered; LBP Jul 3 draft marked rejected)
errors: null
stops:
  - Banca ICCREA: STOP-2 (meeting Jul 9 in 2 days)
skips:
  - BNP Paribas: freeze rule (wait_for_inbound_signal)
  - BPCE: cadence (7d since Jun 30, Tier B min 10d) + meeting echo Jul 6
  - La Banque Postale: cadence (4d since Jul 3 draft) + no_checkin_during_pilot rule
  - Deblock: min_inter_email_gap 14d (last contact Jul 1, eligible Jul 15)
  - CGD: cadence (7d since Jun 30, Tier B min 10d, eligible Jul 10)
  - Score & Secure Payment: feedback pending on Jun 30 rejected draft
  - Swan: COLD Tier C, 11d since Jun 26, min 20d
  - 17 COLD deals: no substantive signal trigger
divergences: 2 minor (Bunq — Deal Pulse has null last_interactions; actuals Jul 1 email_out + May 15 email_in — data freshness lag, not blocking)
duration_estimate: light
---
