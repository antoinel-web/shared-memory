---
agent: ae-salesforce-operator
run_at: 2026-07-03T09:00:00+02:00
trigger: pipeline-scan
status: success
sources_read:
  - slack: 60+ messages across 8 searches
  - gmail: 40 threads scanned
  - granola: 0 meetings (no external meetings this week in Granola)
  - calendar: 15 events (past 7d) + upcoming events fetch failed
  - salesforce: 25 opps queried
outputs_written:
  - salesforce: 6 opps updated (Xpollens full MEDDPICC, Deblock full MEDDPICC, BPCE NextStep+RedFlags, Swan NextStep+RedFlags, ICCREA NextStep+RedFlags, BNP NextStep+RedFlags), 4 activity tasks created
  - slack: nudge posted to #sales-inbox
  - github: deal-signals.json updated on main
errors: null
duration_estimate: heavy
---
