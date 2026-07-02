---
agent: daily-data-logger
run_at: 2026-07-02T23:00:00+02:00
status: success
sources_read:
  - gmail: 6 emails traités
  - bluedot: via webhook temps réel (hors scope logger nocturne)
outputs_written:
  - drive_files: 7 fichiers écrits
  - classification_emails: 0 emails envoyés
  - mapping_log_appends: 0 règles ajoutées
pending_classifications: 0 domaines en attente
errors: null
duration_estimate: light
---

## Détail des fichiers écrits

| Client | Fichier | Type | Direction |
|--------|---------|------|-----------|
| BGL BNP Luxembourg | 2026-07-02_re-cube-ai-bgl-bnp-paribas-comptes-mules_0554.txt | email | inbound |
| BGL BNP Luxembourg | 2026-07-02_re-cube-ai-bgl-bnp-paribas-comptes-mules_1052.txt | email | outbound |
| ICCREA Banca | 2026-07-02_re-iccrea-cube-ai-f2f-meeting.txt | email | outbound |
| Swan | 2026-07-02_re-swan-intro-next-steps.txt | email | outbound |
| Xpollens | 2026-07-02_re-xpollens-compte-rendu-prochaines-etapes.txt | email | outbound |
| Caixa Geral de Depositos | 2026-07-02_re-cube-ai-prevencao-de-fraude-precoce.txt | email | outbound |
| BPCE - Banque Populaire Caisse d'Epargne | 2026-07-02_re-analyse-comptes-mules-bpce.txt | email | outbound |

## Emails exclus (filtrés)

- Bluedot notifications (SWAN call, Xpollens call, internal 1:1) → géré via webhook temps réel
- Granola newsletter → marketing
- Tasklet notifications (Slack Signals, Meeting Sniper) → tool notifications
- Calendar accept/decline (ICCREA Carolina, BIL Amaury Guns) → sujets contenant Accepted/Accettato
- hotmail.com (Paolo Battiston) → domaine [IGNORED] per mapping-log
- Thread "Refusée : BPCE <> CUBE AI" → sujet = déclin calendrier

## Notes

- Aucun domaine inconnu détecté aujourd'hui — 0 classification email envoyé
- natixis.com inclus dans le fichier BPCE (Louis Murat en destinataire) → domaine déjà mappé BPCE dans mapping-log
- BIL folder: confirmé résolu dans le run du 2026-06-30
