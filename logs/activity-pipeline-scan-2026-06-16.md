---
agent: ae-salesforce-operator
run_at: 2026-06-16T09:00:00+02:00
trigger: pipeline-scan
status: success
sources_read:
  - slack: 52 messages
  - gmail: 15 threads
  - granola: 3 queries
  - calendar: 28 events (past 7d + upcoming 14d)
  - salesforce: 22 opps queried
outputs_written:
  - salesforce: 8 opps updated, 6 activity tasks created
  - slack: nudge posted to #sales-inbox
  - github: deal-signals.json updated
errors: Competitors_MP__c picklist value 'Other' rejected by SF (Isabel opp) — field left blank
duration_estimate: heavy
---
