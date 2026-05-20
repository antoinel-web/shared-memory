---
agent: meeting-echo
run_at: 2026-05-20T13:10:00+02:00
status: success
sources_read:
  - granola: 1 meeting
  - gmail: 17 threads
  - slack: 0 canaux (aucun canal client trouvé)
  - drive: 5 fichiers
  - calendar: 1 événement
  - bigquery: NOT_USED (table not found)
outputs_written:
  - gmail_draft: RE: Cube3 <> Credit Agricole / LCL : revue premiers résultats.
errors: BigQuery table production-20230612:apex_for_statistics.actual_financial_institutions not found — non bloquant, chiffres issus du transcript
duration_estimate: medium
---
---
agent: meeting-echo
run_at: 2026-05-20T12:57:00+02:00
status: success
sources_read:
  - granola: 1 meeting (transcript null, notes via Bluedot recap)
  - gmail: 20 threads
  - slack: FAILED (aucun canal client trouvé)
  - drive: 5+ fichiers dont Mutual Action Plan
  - calendar: 1 événement
  - bigquery: NOT_USED
outputs_written:
  - gmail_draft: RE: Fichiers ajoutés
errors: null
duration_estimate: medium
---
