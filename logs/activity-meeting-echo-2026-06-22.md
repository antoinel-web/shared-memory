---
agent: meeting-echo
run_at: 2026-06-22T11:30:00+02:00
status: success
sources_read:
  - granola: FAILED (aucune réunion Deblock trouvée — fallback Bluedot)
  - gmail: 20 threads
  - slack: 0 canaux pertinents
  - drive: 23 fichiers
  - calendar: 5 événements
  - bigquery: NOT_USED
outputs_written:
  - gmail_draft: RE: Analyse comptes mules BPCE
errors: Granola no transcript found for BPCE meeting — Bluedot recap used as fallback
duration_estimate: heavy
---
---
agent: meeting-echo
run_at: 2026-06-22T18:28:00+02:00
status: success
sources_read:
  - granola: FAILED (no meeting found — Bluedot recap used as fallback)
  - gmail: 10 threads
  - slack: 0 canaux pertinents
  - drive: 23 fichiers
  - calendar: 1 événement pertinent
  - bigquery: NOT_USED
outputs_written:
  - gmail_draft: Deblock — Intro & next steps
errors: null
duration_estimate: medium
---
