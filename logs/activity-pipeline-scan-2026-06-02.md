---
agent: ae-salesforce-operator
run_at: 2026-06-02T09:00:00+02:00
trigger: pipeline-scan
status: success
sources_read:
  - slack: 42 messages
  - gmail: 15 threads
  - granola: 8 meetings queried
  - calendar: 22 events (past 7d) + 18 events (next 14d)
  - salesforce: 29 opps queried
outputs_written:
  - salesforce: 13 opps updated, 0 activity tasks created
  - slack: nudge posted via DM (sales-inbox not found)
  - github: deal-signals.json updated
errors: "Competitors_MP__c picklist value 'Other' rejected on BNP — removed and retried successfully"
duration_estimate: heavy
---
