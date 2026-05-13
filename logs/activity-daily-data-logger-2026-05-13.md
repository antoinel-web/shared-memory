---
agent: daily-data-logger
run_at: 2026-05-13T23:07:00+02:00
status: success
sources_read:
  - gmail: 5 emails lus
  - granola: 1 call lu
outputs_written:
  - drive_files: 6 fichiers écrits
  - classification_emails: 0 emails envoyés
  - mapping_log_appends: 0 règles ajoutées
pending_classifications: 0 domaines en attente
errors: null
duration_estimate: light
---

## Détail des fichiers écrits

### Emails

| Fichier | Client | Direction | Dossier Drive |
|---------|--------|-----------|---------------|
| 2026-05-13_fichiers-ajoutes_1032.txt | NL BNP Paribas IT | inbound | Clients/NL BNP Paribas IT/emails/ |
| 2026-05-13_fichiers-ajoutes_1927.txt | NL BNP Paribas IT | outbound | Clients/NL BNP Paribas IT/emails/ |
| 2026-05-13_re-societe-generale-intro-prochaines-etapes.txt | Société Générale | outbound | Clients/Société Générale/emails/ |
| 2026-05-13_re-cube-ai-prevencao-de-fraude-precoce-com-ia.txt | Caixa Geral de Depositos | outbound | Clients/Caixa Geral de Depositos/emails/ |
| 2026-05-13_re-declined-cube-ai-incore-catch-up.txt | Incore Bank | outbound | Clients/Incore Bank/emails/ |

### Calls

| Fichier | Client | Dossier Drive |
|---------|--------|---------------|
| 2026-05-13_internal-prep-meeting-bnp-head-cybermonitoring.txt | NL BNP Paribas IT | Clients/NL BNP Paribas IT/calls/ |

## Nouveaux dossiers créés

- `Clients/NL BNP Paribas IT/` (+ sous-dossiers emails/, calls/)
- `Clients/Incore Bank/` (+ sous-dossier emails/)

## Emails exclus (filtres)

- Réponses/acceptations calendrier (BGL, BNP Paribas Fortis × 2, Incore OOO auto-reply)
- Notifications outils : Tasklet × 2, Jira × 2, Atlassian × 1
- Newsletter : France FinTech
- Gemini Notes (interne)
- Auto-replies OOO d'Antoine (× 5)
- Draft Banca Sella (non envoyé)
- Bounce ACAMS (mail delivery subsystem)

## Note classification call Granola

Le call "Internal : prep meeting with BNP's Head Of CyberMonitoring" est un meeting interne (participants : @cube3.ai uniquement). Classifié sous **NL BNP Paribas IT** par inférence du titre (référence directe à BNP, domaine bnpparibas.com).
