---
agent: ae-salesforce-operator
run_at: 2026-05-26T09:00:00+02:00
trigger: pipeline-scan
status: success
sources_read:
  - slack: 12 messages
  - gmail: 40 threads scanned
  - granola: 4 meetings
  - calendar: 14 events (7d past + 14d future)
  - salesforce: 16 opps queried
outputs_written:
  - salesforce: 16 opps updated, 0 activity tasks created
  - slack: nudge sent via DM (sales-inbox not found in public channels)
  - github: deal-signals.json pushed to data/
errors: null
duration_estimate: heavy
---
