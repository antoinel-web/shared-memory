---
agent: daily-data-logger
run_at: 2026-05-29T23:00:00+02:00
status: success
sources_read:
  - gmail: 1 email lu
  - bluedot: via webhook temps réel (hors scope logger nocturne)
outputs_written:
  - drive_files: 1 fichier écrit
  - classification_emails: 0 emails envoyés
  - mapping_log_appends: 0 règles ajoutées
pending_classifications: 0 domaines en attente
errors: null
duration_estimate: light
---

## Détail du run — 2026-05-29

### Emails traités

| # | Direction | Client | Domaine | Sujet | Fichier Drive |
|---|-----------|--------|---------|-------|---------------|
| 1 | outbound  | Incore Bank | incorebank.ch | meeting | `Clients/Incore Bank/emails/2026-05-29_meeting.txt` |

### Emails exclus (filtrés)
- `notifications@app.outreach.io` — voicemail notification (outil)
- `donotreply@acams.org` — confirmation participation ACAMS event
- `anne-lise.escoffre@bnpparibas.com` — invitation calendrier acceptée ("Acceptée : CUBE AI <> BNP Group")
- `gemini-notes@google.com` × 2 — notes Gemini
- `emails@bluedothq.com` × 2 — recaps Bluedot (gérés par webhook temps réel)
- `events@swoogo.com` (GASA) — confirmation inscription événement
- `notifications@agents.tasklet.ai` × 3 — notifications outils internes
- Drafts Gmail × 5 — brouillons non envoyés (BGL, BPER, Intesa Sanpaolo, Bunq, CA/LCL)

### Classifications en attente
Aucune — tous les domaines précédents ont reçu une réponse d'Antoine.
