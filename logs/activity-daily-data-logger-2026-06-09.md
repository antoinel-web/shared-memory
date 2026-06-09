---
agent: daily-data-logger
run_at: 2026-06-09T23:00:00+02:00
status: success
sources_read:
  - gmail: 0 emails clients (7 threads inspectés, tous exclus)
  - bluedot: via webhook temps réel (hors scope logger nocturne)
outputs_written:
  - drive_files: 0 fichiers écrits
  - classification_emails: 0 emails envoyés
  - mapping_log_appends: 0 règles ajoutées
pending_classifications: 0 domaines en attente
errors: null
duration_estimate: light
---

## Détail des exclusions

### Emails inbound non-cube3.ai
| Expéditeur | Domaine | Raison exclusion |
|---|---|---|
| Bluedot (emails@bluedothq.com) | bluedothq.com | Notification tool — call Credem géré via webhook |
| Jira (jira@cube-web3.atlassian.net) | cube-web3.atlassian.net | Notification Jira |
| BioCatch (marketing@biocatch.com) | biocatch.com | Newsletter/marketing |
| GASA (events@swoogo.com) | swoogo.com | Transactionnel (ticket conférence) |
| Tasklet (notifications@agents.tasklet.ai) | agents.tasklet.ai | Notification outil |
| Air France (check-in@service-airfrance.com) | airfrance.com | Transactionnel (check-in vol) |
| Don F.-X. Dallot (fxdallot@csm.fr) | csm.fr | Email personnel (livret de messe Inès & Antoine) |

### Emails outbound antoinel@cube3.ai
| Destinataire | Sujet | Raison exclusion |
|---|---|---|
| antoinel@cube3.ai | 📅 Events Briefing — 9 juin 2026 | Email à soi-même (agent automatisé) |

## Appels (Bluedot)
- Call Credem détecté dans la boîte Gmail : "(17) CUBE AI <> Credem - Mule intelligence" (28 min, 09/06/2026)
- Traitement hors scope logger — géré en temps réel via webhook Bluedot

## Résolution classifications en attente
- eneba.com : résolu — ajouté dans domain-mapping.txt le 2026-06-02 (Eneba). Pas de relance nécessaire.
- iccrea.bcc.it, qonto.com, ouitrust.com, isabelgroup.eu : tous résolus (confirmés dans mapping-log.txt)
