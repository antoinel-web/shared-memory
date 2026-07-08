---
agent: daily-data-logger
run_at: 2026-07-08T23:00:00+02:00
status: success
sources_read:
  - gmail: 8 emails lus
  - bluedot: via webhook temps réel (hors scope logger nocturne)
outputs_written:
  - drive_files: 8 fichiers écrits
  - classification_emails: 0 emails envoyés
  - mapping_log_appends: 0 règles ajoutées
pending_classifications: 0 domaines en attente
errors: null
duration_estimate: light
---

## Emails traités — 2026-07-08

| Client | Fichier Drive | Direction | Domaine |
|--------|--------------|-----------|---------|
| ICCREA Banca | 2026-07-08_automatic-reply-iccrea-cube-ai-f2f-meeting.txt | inbound | iccrea.bcc.it |
| ICCREA Banca | 2026-07-08_re-iccrea-cube-ai-f2f-meeting_1249.txt | outbound | iccrea.bcc.it |
| ICCREA Banca | 2026-07-08_re-iccrea-cube-ai-f2f-meeting_2037.txt | inbound | iccrea.bcc.it |
| Deblock | 2026-07-08_re-deblock-x-cube-ai.txt | outbound | deblock.com |
| LBP - La Banque Postale | 2026-07-08_vous-ai-je-perdus.txt | outbound | labanquepostale.fr |
| Banca Sella | 2026-07-08_re-cube-ai-banca-sella.txt | outbound | sella.it |
| Bunq | 2026-07-08_re-first-mule-test-50-accounts.txt | outbound | bunq.com |
| Swan | 2026-07-08_re-swan-intro-next-steps.txt | outbound | swan.io |

## Exclusions appliquées

- Gemini notes (gemini-notes@google.com) — outil interne
- Outseer Webinars (info@outseer.com) — newsletter/marketing
- Jira notification (jira@cube-web3.atlassian.net) — notification outil
- Bluedot Daily Q3 Standup (emails@bluedothq.com) — internal call summary
- Tasklet Slack Signals — notification outil interne
- Email interne Deblock review (à jonathana@cube3.ai, slayh@cube3.ai uniquement)
