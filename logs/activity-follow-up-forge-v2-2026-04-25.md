---
agent: follow-up-forge-v2
run_at: 2026-04-25T00:21:48+02:00
status: success
sources_read:
  - deal_pulse_github: 26 deals
  - salesforce: 26 opportunities
  - gmail: 15 deals searched
  - granola: 8 deals searched
  - slack: 8 channels searched
  - calendar: 6 deals searched
  - bigquery: FAILED (apex_for_statistics not found in europe-west3)
outputs_written:
  - gmail_drafts: 8 drafts created
  - run_log: logged to DB run_log id=1
  - learning_params: no change (first run)
errors: BigQuery table apex_for_statistics not found in europe-west3 — no quantitative metrics available
duration_estimate: heavy
---
