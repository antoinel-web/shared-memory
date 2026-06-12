---
agent: follow-up-forge-v2
run_at: 2026-06-12T08:00:00+02:00
run_id: 23
status: success
sources_read:
  deal_pulse_github: 24 deals (published_at 2026-06-12T07:30:00+02:00, fresh, 30min old)
  salesforce: not queried (no salesforce_ids in Deal Pulse payload)
  gmail: queried for all 17 dispatched deals (contact resolution, thread lookup, stop checks)
  granola: queried for multiple deals (Swan, Intesa, BPCE, BNP, bunq)
  slack: queried for BNP Paribas, bunq, BPCE, BGL, LBP
  calendar: queried for OOO check + deal stop checks (STOP-2, STOP-3)
  bigquery: FAILED (apex_for_statistics not found in europe-west3)
outputs_written:
  gmail_drafts: 7 drafts created (Banca Profilo, Groupe BPCE, BGL BNP Paribas, La Banque Postale, Swan, BNP Paribas, bunq)
  run_log: logged to DB run_log id=23
  learning_params: no change (learning loop ran, 0 feedback axes available)
  divergence_email: sent for Qonto (1 deal)
  feedback_email: sent for 6 rejected drafts from run Jun 5
  draft_log: 7 entries appended to logs/drafts/forge-v2-2026-06-12.md
errors:
  bigquery: apex_for_statistics table not found in europe-west3 (persistent, ongoing)
  deal_pulse_data_gaps: last_interactions null for all deals (contacts resolved via Gmail each run)
  bnl: contacts not found via Gmail (domain bnlmail.com unknown — HOT deal blocked)
duration_estimate: heavy
deals_stopped: 2 (Intesa Sanpaolo STOP-3, CaixaGeral de Depositos STOP-3)
deals_skipped: 14 (8 no-contact, 6 cadence)
deals_divergent: 1 (Qonto)
---
