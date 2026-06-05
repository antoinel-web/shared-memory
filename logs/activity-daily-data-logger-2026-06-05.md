---
agent: daily-data-logger
run_at: 2026-06-05T23:01:00+02:00
status: success
sources_read:
  - gmail: 4 emails lus
  - bluedot: via webhook temps réel (hors scope logger nocturne)
outputs_written:
  - drive_files: 4 fichiers écrits
  - classification_emails: 0 emails envoyés
  - mapping_log_appends: 0 règles ajoutées
pending_classifications: 0
errors: null
duration_estimate: light
---

## Détail des fichiers écrits

### BNL BNP Paribas IT / emails
- `2026-06-05_presentation-de-la-solution-cube-ai.txt` — inbound de jerome.palin@bnpparibas.com, sujet : Présentation de la solution CUBE AI

### BPER Banca / emails
- `2026-06-05_r-bper-intro-next-steps_0800.txt` — inbound de davide.belloni@bper.it, sujet : R: BPER — Intro & next steps (métriques POC : 90% détection on-bank, 1.3M€ off-bank)
- `2026-06-05_re-bper-intro-next-steps_1808.txt` — outbound de antoinel@cube3.ai, sujet : Re: BPER — Intro & next steps (follow-up post-call, annonce business case)

### Swan / emails
- `2026-06-05_re-swan-intro-next-steps.txt` — outbound de antoinel@cube3.ai, sujet : Re: Swan — Intro & next steps (récap POC : 87 comptes, 14 alertes avant systèmes Swan, ~900K€ évités)

## Emails filtrés (exclus)
- 3 notifications Bluedot (gérées via webhook)
- 2 notes Gemini
- 2 notifications Tasklet
- 1 déclin calendrier Google (salle Saint-Maur)
- 1 acceptation calendrier (Luc Drye/Swan)
- 1 email enquête Navan
- 3 brouillons non envoyés (BGL, UniCredit, BNP Paribas Fortis/Belfius)

## Domaines pendants
- Aucun. Le domaine eneba.com (classification envoyée le 2026-06-01) a été ajouté directement dans domain-mapping.txt → Eneba.
