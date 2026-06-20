---
agent: daily-data-logger
run_at: 2026-06-20T23:00:00+02:00
status: success
sources_read:
  - gmail: 2 emails lus (0 retenus — 1 Jira notification, 1 email personnel)
  - bluedot: via webhook temps réel (hors scope logger nocturne)
outputs_written:
  - drive_files: 8 fichiers écrits (backlog pending files résolus)
  - classification_emails: 2 emails envoyés (1 relance iwf.org.uk, 1 nouveau BNP Group)
  - mapping_log_appends: 0 règles ajoutées
pending_classifications: 2 domaines en attente (iwf.org.uk, BNP Group)
errors: null
duration_estimate: medium
---

## Détail des fichiers écrits

### BPCE - Banque Populaire Caisse d'Epargne
- `emails/2026-06-11_re-cube-ai-bpce-strategie.txt` — email tfactory.fr (inbound+outbound, classification résolue 2026-06-16)
- `emails/2026-06-15_re-cube-ai-bpce-strategie.txt` — email tfactory.fr outbound (classification résolue 2026-06-16)
- `calls/2026-06-12_t-factory-cube-ai.txt` — call T-Factory × CUBE AI via Bluedot (classification résolue 2026-06-16)

### BNL BNP Paribas IT
- `emails/2026-06-18_en-amont-de-notre-meeting-du-23-juin.txt` — email outbound Antoine → équipe BNP avant réunion 23 juin
- `emails/2026-06-18_reponse-automatique-en-amont-meeting-23-juin.txt` — réponse automatique Jérôme Palin (absent)

### SSP - Score & Secure Payments
- `calls/2026-06-08_cube-ai-demo-plateforme.txt` — call Bluedot demo plateforme avec Gilles (SSP)

### Isabel Group
- `calls/2026-06-12_cube-ai-isabel-belgium-anti-fraud.txt` — call Bluedot Isabel Group Belgium anti-fraud initiative

### Xpollens
- `calls/2026-06-18_cube-ai-x-xpollens-comptes-mules.txt` — call Bluedot Xpollens comptes mules

## Classifications en attente
- **iwf.org.uk** : 5e relance envoyée (Penny Tyler, GASA Summit, depuis 2026-06-16)
- **BNP Group call 2026-06-08** : 1ère demande envoyée (Anne-Lise, entité BNP non identifiée)

## Emails du jour 2026-06-20
- Aucun email client retenu.
- Exclusions : 1 newsletter Jira (Atlassian), 1 email personnel (antoinelecouturier@gmail.com — livrets de messe).
