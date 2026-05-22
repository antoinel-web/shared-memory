---
agent: ae-salesforce-operator
run_at: 2026-05-22T09:00:00+02:00
trigger: pipeline-scan
status: success
sources_read:
  - slack: 12 messages
  - gmail: 7 threads
  - granola: 4 meetings
  - calendar: 40+ events
  - salesforce: 29 opps queried
outputs_written:
  - salesforce: 12 opps updated, 5 activity tasks created
  - slack: nudge posted as DM (no #sales-inbox channel found)
  - github: deal-signals.json pushed to main
errors: nudge_log insert failed (schema mismatch)
duration_estimate: heavy
---
