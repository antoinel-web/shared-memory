---
agent: ae-salesforce-operator
run_at: 2026-05-29T09:00:00+02:00
trigger: pipeline-scan
status: success
sources_read:
  - slack: 19 messages
  - gmail: 40 threads
  - granola: 8 meetings
  - calendar: 17 events
  - salesforce: 31 opps queried
  - drive: 25 files
outputs_written:
  - salesforce: 13 opps updated, 0 activity tasks created
  - slack: nudge posted to #sales-inbox
  - github: deal-signals.json pushed to main
errors: null
duration_estimate: heavy
---
