---
agent: ae-salesforce-operator
run_at: 2026-07-21T09:00:00+02:00
trigger: pipeline-scan
status: success
sources_read:
  - slack: 26 messages
  - gmail: 19 threads
  - granola: 4 queries (ICCREA, CGD, BNP/Intesa/Swan/Deblock, Bunq/BPER/CA/Spuerkeess)
  - calendar: 14 events (past 7d) + 35 events (next 30d)
  - salesforce: 25 opps queried
outputs_written:
  - salesforce: 4 opps updated (52 fields), 1 new opp created (Spuerkeess)
  - slack: nudge posted to #sales-inbox
  - github: deal-signals.json updated on main
errors: null
duration_estimate: heavy
---
