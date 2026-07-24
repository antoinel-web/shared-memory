---
agent: daily-data-logger
run_at: 2026-07-24T23:00:00+02:00
status: partial
sources_read:
  - gmail: 1 email traité (5 threads trouvés, 4 exclus — 1 notification Tasklet, 2 auto-replies LinkedIn OOO, 1 retenu Banca Profilo)
  - bluedot: via webhook temps réel (hors scope logger nocturne)
outputs_written:
  - drive_files: 1 fichier écrit
  - classification_emails: 0 emails envoyés
  - mapping_log_appends: 0 règles ajoutées
pending_classifications: 0 domaines en attente
errors: Gmail bodyFull retourné vide pour message 19f95cd483d997a8 (Re: CUBE AI <> Banca Profilo - Mules Testing) — enregistré avec snippet uniquement
duration_estimate: light
---

## Détail des emails traités

### Retenu
- **Banca Profilo** (bancaprofilo.it) — outbound — `Re: CUBE AI <> Banca Profilo - Mules Testing` (2026-07-24)
  - Fichier : `Clients/Banca Profilo/emails/2026-07-24_re-cube3-banca-profilo-mules-testing.txt`
  - ⚠️ Corps email vide retourné par l'API Gmail — contenu partiel (snippet uniquement)

### Exclus
- Tasklet notification (Slack Signals digest)
- 2× auto-reply OOO d'Antoine vers LinkedIn bounce addresses
