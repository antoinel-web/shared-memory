---
agent: daily-data-logger
run_at: 2026-05-25T23:01:00+02:00
status: success
sources_read:
  - gmail: 0 emails clients (3 emails filtrés : 1 acceptation calendrier UniCredit, 2 notifications Tasklet)
  - bluedot: via webhook temps réel (hors scope logger nocturne)
outputs_written:
  - drive_files: 0
  - classification_emails: 0
  - mapping_log_appends: 0
pending_classifications: 0
errors: null
duration_estimate: light
---

### Notes
- Journée sans activité client email (dimanche de Pentecôte, jour férié en France/Belgique)
- Emails filtrés : acceptation calendrier UniCredit (`CUBE AI <> Unicredit: Data Protection Q&As`), 2 notifications Tasklet
- Aucun email sortant d'Antoine détecté aujourd'hui
- Toutes les classifications précédentes sont résolues (qonto.com, ouitrust.com, isabelgroup.eu, etc.)
- Calls gérés en temps réel via webhook Bluedot
