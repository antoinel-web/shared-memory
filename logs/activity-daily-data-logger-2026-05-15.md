---
agent: daily-data-logger
run_at: 2026-05-15T23:07:00+02:00
status: partial
sources_read:
  - gmail: 6 emails lus
  - granola: 0 calls (aucun meeting enregistré aujourd'hui)
outputs_written:
  - drive_files: 4 fichiers écrits
  - classification_emails: 2 emails envoyés
  - mapping_log_appends: 0 règles ajoutées
pending_classifications: 2 domaines en attente (qonto.com, ouitrust.com)
errors: Granola vide (aucun meeting le 2026-05-15). 2 domaines inconnus envoyés en classification à Antoine.
duration_estimate: light
---

## Détail des fichiers écrits

| Fichier | Client | Dossier Drive |
|---------|--------|---------------|
| 2026-05-15_re-cube-bunq-friday-3pm_1136.txt | Bunq | Clients/Bunq/emails/ |
| 2026-05-15_re-cube-bunq-friday-3pm_1823.txt | Bunq | Clients/Bunq/emails/ |
| 2026-05-15_re-deblock-intro-next-steps.txt | Deblock | Clients/Deblock/emails/ |
| 2026-05-15_re-cube-ai-banca-sella.txt | Banca Sella | Clients/Banca Sella/emails/ |

## Classifications en attente

- `qonto.com` → email envoyé à Antoine (sujet: CUBE × Qonto — pilote 3 mois)
- `ouitrust.com` → email envoyé à Antoine (sujet: OuiTrust × CUBE — encore d'actualité ?)

## Emails exclus (filtrés)

- Thomas Glaus (incorebank.ch): acceptation calendrier → excluded (calendar invite)
- Jira notifications (cube-web3.atlassian.net): outil interne → excluded
- Bluedot recap: notification outil → excluded
- ACFE Training: marketing → excluded
- Tasklet notifications: outil interne → excluded
- OOO auto-replies (multiple): réponses automatiques → excluded
- Drafts non envoyés (6): BNP Paribas Fortis, BNL, BPCE, Swan, Swissquote, Intesa Sanpaolo → excluded (non envoyés)
