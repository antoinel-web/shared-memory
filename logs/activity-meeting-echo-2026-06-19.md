---
agent: meeting-echo
run_at: 2026-06-19T16:58:00+02:00
status: success
sources_read:
  - granola: FAILED (no meeting found for this client)
  - gmail: 20 threads found, Bluedot recap used as transcript source
  - slack: 0 canaux trouvés pour ce client
  - drive: 4 fichiers trouvés
  - calendar: 1 événement trouvé
  - bigquery: NOT_USED
outputs_written:
  - gmail_draft: "Xpollens — Compte-rendu & prochaines étapes"
errors: Granola — aucun transcript trouvé pour ce client (fallback Bluedot utilisé)
duration_estimate: medium
---
---
agent: meeting-echo
run_at: 2026-06-19T19:00:00+02:00
status: success
sources_read:
  - granola: FAILED (no meeting found for ICCREA)
  - gmail: 19 threads
  - slack: NOT_FOUND
  - drive: 13 fichiers trouvés
  - calendar: 1 événement trouvé
  - bigquery: NOT_USED
outputs_written:
  - gmail_draft: "ICCREA Banca — Intro & next steps"
errors: Granola — no transcript found for ICCREA (fallback: Drive Bluedot recap + Gmail context)
duration_estimate: medium
---
---
agent: meeting-echo
run_at: 2026-06-19T21:00:00+02:00
status: success
sources_read:
  - granola: FAILED (no meeting found for BPCE)
  - gmail: 20 threads found, Bluedot recap used as transcript source
  - slack: 0 canaux trouvés pour ce client
  - drive: 50 fichiers trouvés (logs emails, présentations, données POC)
  - calendar: 1 événement trouvé
  - bigquery: NOT_USED
outputs_written:
  - gmail_draft: "RE: Analyse comptes mules BPCE"
errors: Granola — aucun transcript trouvé pour BPCE (fallback Bluedot utilisé)
duration_estimate: medium
---
