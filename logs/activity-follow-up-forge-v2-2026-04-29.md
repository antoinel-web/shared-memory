---
agent: follow-up-forge-v2
run_at: 2026-04-29T08:15:00+02:00
status: success
sources_read:
  - deal_pulse_github: 29 deals
  - salesforce: 10 opportunities (targeted)
  - gmail: 14 deals searched
  - granola: 10 deals searched
  - slack: 6 channels searched
  - calendar: 5 deals searched
  - bigquery: FAILED (apex_for_statistics not found in europe-west3, recurring)
outputs_written:
  - gmail_drafts: 10 drafts created
  - run_log: logged to DB run_log id=6
  - learning_params: no change (insufficient pattern data)
  - github_activity_report: logs/activity-follow-up-forge-v2-2026-04-29.md
errors: BigQuery table apex_for_statistics not found in europe-west3 (recurring — no quantitative metrics)
duration_estimate: heavy
---
