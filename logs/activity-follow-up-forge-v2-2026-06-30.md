---
agent: follow-up-forge-v2
run_at: 2026-06-30T08:00:00+02:00
status: success
sources_read:
  deal_pulse_github: 24 deals parsed (published_at 2026-06-30T05:35:22+02:00, fresh)
  salesforce: queried per deal (token refresh per call)
  gmail: searched per deal (6 processors)
  granola: queried per deal (6 processors)
  slack: searched per deal (6 processors)
  calendar: checked for OOO + STOP-2 (7 events found today)
  google_drive: searched per deal (6 processors)
  bigquery: FAILED (apex_for_statistics table not found in europe-west3 — ongoing known issue)
outputs_written:
  gmail_drafts: 5 drafts created (BPCE, Banca ICCREA, CGD, bunq, SSP)
  run_log: logged to DB run_log id=28
  draft_log_github: logs/drafts/forge-v2-2026-06-30.md appended per draft
  learning_params: no changes (learning loop ran, no threshold rules triggered)
errors: |
  Swan draft #111 (bridge_handoff, Jun 26) detected as rejected by learning loop — feedback email sent to Antoine.
  BigQuery apex_for_statistics table not found — no quantitative metrics in drafts (ongoing).
  Tier A bridge_handoff trop_plat pattern flagged (quality note, no param change).
stops:
  - Deblock: STOP-2 (meeting today 2026-06-30 17:30)
  - Swan: STOP-2 (meeting 2026-07-02)
  - XPollens: STOP-2 (meeting 2026-07-02)
skips:
  - La Banque Postale: cadence not met (7d since Jun 23, min 10d for Tier B)
  - BNP Paribas: freeze rule (wait_for_inbound_signal)
  - 14 COLD deals: no substantive signal trigger
duration_estimate: heavy
---
