---
agent: ae-salesforce-operator
run_at: 2026-06-09T09:00:00+02:00
trigger: pipeline-scan
status: success
sources_read:
  - slack: 40+ messages searched
  - gmail: 12 threads
  - granola: 3 meetings queried
  - calendar: 14 events (past 7d) + 12 events (next 2w)
  - salesforce: 9 opps queried
outputs_written:
  - salesforce: 9 opps updated (91 fields total), 0 activity tasks
  - slack: nudge posted to #sales-inbox
  - github: deal-signals.json pushed to main
errors: Competitors_MP__c "Other" not in restricted picklist - field skipped
duration_estimate: heavy
---
