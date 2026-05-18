---
agent: daily-data-logger
run_at: 2026-05-18T23:08:00+02:00
status: success
sources_read:
  - gmail: 4 emails lus
  - granola: 1 call lu
outputs_written:
  - drive_files: 6 fichiers écrits
  - classification_emails: 2 emails envoyés (relances)
  - mapping_log_appends: 0 règles ajoutées
pending_classifications: 2 domaines en attente (qonto.com, ouitrust.com)
errors: null
duration_estimate: medium
---

## Détail des éléments traités

### Emails (4 traités)

| Fichier | Client | Direction |
|---------|--------|-----------|
| 2026-05-18_re-external-re-fichiers-ajoutes.txt | BNP Paribas - France | inbound (jerome.palin@bnpparibas.fr) |
| 2026-05-18_nouvelle-heure-proposee-cube-bpce-analyse-ibans-externes-compromis.txt | BPCE | inbound (sarah.guigny@bpce.fr) |
| 2026-05-18_re-nouvelle-heure-proposee-cube-bpce-analyse-ibans-externes-compromis.txt | BPCE | outbound (→ sarah.guigny@bpce.fr) |
| 2026-05-18_re-cube-ai-banca-profilo-mules-testing.txt | Banca Profilo | outbound (→ antonio.gravetti@bancaprofilo.it) |

### Calls Granola (1 traité)

| Fichier | Clients | Domaines |
|---------|---------|----------|
| 2026-05-18_cube3-credit-agricole-lcl-revue-premiers-resultats.txt | Credit Agricole + LCL (multi-client) | ca-ibs.com, lcl.fr |

Note: Fichier dupliqué dans Credit Agricole/calls/ et LCL/calls/

### Calls Granola exclus (internes)
- "BNP Paribas quick sync ?" — participants uniquement Cube3 (antoinel@cube3.ai, jonathana@cube3.ai)
- "Bohdan <> Antoine" — participants uniquement Cube3

### Emails filtrés (exclusions)
- 2x Gemini Notes (gemini-notes@google.com)
- 3x Bluedot recaps (emails@bluedothq.com)
- 2x Bluedot marketing (dima@bluedothq.com)
- 2x acceptations calendrier (bnpparibas.com, ca-ibs.com)
- 1x nouvelle heure calendrier BPCE (sarah.guigny@bpce.fr acceptation)
- 1x réponse automatique OOO BPCE (corinne.JOLY@bpce.fr)
- 1x confirmation inscription ACAMS (donotreply@acams.org)
- 2x Pipedream onboarding (notifications@pipedream.com, tod@pipedream.com)
- 2x Tasklet notifications

### Classifications en attente
- **qonto.com** (thibault.mosser@qonto.com) — relance #3 envoyée ce soir
- **ouitrust.com** (jaime.gaffud@ouitrust.com) — relance #3 envoyée ce soir

### Nouveaux dossiers créés
- Credit Agricole/calls/ (nouveau sous-dossier)
- LCL/calls/ (nouveau sous-dossier)
