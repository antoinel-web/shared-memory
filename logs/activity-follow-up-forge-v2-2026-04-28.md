---
agent: follow-up-forge-v2
run_at: 2026-04-28T08:18:21+02:00
status: success
sources_read:
  - deal_pulse_github: 26 deals
  - salesforce: 26 opportunities
  - gmail: 19 deals searched
  - granola: 10 deals searched
  - slack: 10 channels searched
  - calendar: 7 deals searched
  - bigquery: FAILED (apex_for_statistics not found in europe-west3)
outputs_written:
  - gmail_drafts: 10 drafts created
  - run_log: logged to DB run_log id=5
  - learning_params: 2 updates (gmail_contact_priority_over_sf, unanswered_email_followup_max_days)
errors: BigQuery table apex_for_statistics not found in europe-west3 (recurring)
duration_estimate: heavy
---
