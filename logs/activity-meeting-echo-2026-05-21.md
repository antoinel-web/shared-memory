---
agent: meeting-echo
run_at: 2026-05-21T16:21:00+02:00
status: success
sources_read:
  - granola: 1 meeting
  - gmail: 4 threads
  - slack: 0 canaux (aucun canal client trouvé)
  - drive: 1 fichier transcript (Bluedot)
  - calendar: 1 événement
  - bigquery: NOT_USED (table introuvable)
outputs_written:
  - gmail_draft: Intesa Sanpaolo — Intro & next steps
errors: BigQuery table production-20230612:apex_for_statistics.actual_financial_institutions not found
duration_estimate: medium
---
---
agent: meeting-echo
run_at: 2026-05-21T16:26:00+02:00
status: success
sources_read:
  - granola: 1 transcript (CUBE AI <> BNP Paribas Fortis - Mule Intelligence, May 20)
  - gmail: 5 threads (including Bluedot recap)
  - slack: FAILED (no client channel found)
  - drive: 1 tracker spreadsheet found
  - calendar: 1 événement (May 20 17:00-17:30)
  - bigquery: NOT_USED
outputs_written:
  - gmail_draft: RE: BNP Paribas Fortis — Intro & next steps
errors: null
duration_estimate: medium
---
