---
agent: daily-data-logger
run_at: 2026-05-20T23:16:00+02:00
status: success
sources_read:
  - gmail: 1 email lu
  - granola: 2 calls lus (transcripts récupérés)
  - bluedot: 3 call files déjà présents en Drive via webhook
outputs_written:
  - drive_files: 1 fichier écrit (email CGD)
  - classification_emails: 0 emails envoyés
  - mapping_log_appends: 0 règles ajoutées
pending_classifications: 0 domaines en attente
errors: null
duration_estimate: light
---

## Détail du run

### Emails traités (1)

| Direction | Client | Sujet | Fichier Drive |
|-----------|--------|-------|---------------|
| outbound | Caixa Geral de Depositos | Re: CUBE AI - Prevenção de fraude precoce com IA | Clients/Caixa Geral de Depositos/emails/2026-05-20_re-cube-ai-prevencao-de-fraude-precoce-com-ia.txt |

### Calls traités (3)

| Client | Titre | Source | Statut |
|--------|-------|--------|--------|
| BNP Paribas Fortis | CUBE AI <> BNP Paribas Fortis - Mule Intelligence (23min) | Bluedot + Granola | Fichier déjà présent via webhook Bluedot — pas de doublon |
| BNP Paribas - France | CUBE AI <> BNPP : Mule Accounts (38min) | Bluedot uniquement | Fichier déjà présent via webhook Bluedot |
| Intesa Sanpaolo | Intesa Sanpaolo <> CUBE AI - Mule Intelligence (33min) | Bluedot + Granola | Fichier déjà présent via webhook Bluedot — pas de doublon |

### Emails exclus (filtrés)

- Notifications Bluedot (emails@bluedothq.com) × 5 — outil
- Notification Granola (notifications@mail.granola.ai) — outil
- Pipedream marketing — newsletter
- Tasklet Slack Signals — outil interne
- Email interne Antoine → Einaras (e@cube3.ai) — domaine @cube3.ai
- Brouillon Antoine → Crédit Agricole (draft: true) — non envoyé

### Classifications en attente

Aucune nouvelle classification requise. Tous les domaines rencontrés aujourd'hui sont déjà mappés :
- cgd.pt → Caixa Geral de Depositos (domain-mapping.txt)
- bnpparibasfortis.com → BNP Paribas Fortis (mapping-log.txt)
- bnpparibas.fr → BNP Paribas - France (domain-mapping.txt)
- intesasanpaolo.com → Intesa Sanpaolo (domain-mapping.txt)
---
agent: daily-data-logger
run_at: 2026-05-21T23:05:00+02:00
status: success
sources_read:
  - gmail: 1 email lu
  - bluedot: via webhook temps réel (hors scope logger nocturne)
outputs_written:
  - drive_files: 1 fichier écrit (email CGD — doublon du run précédent)
  - classification_emails: 0 emails envoyés
  - mapping_log_appends: 0 règles ajoutées
pending_classifications: 0 domaines en attente
errors: doublon fichier Drive — email CGD du 2026-05-20 déjà écrit par le run précédent (2026-05-20T23:16)
duration_estimate: light
---

## Détail du run (2ème run du jour — 2026-05-21T23:05+02:00)

### Note
Ce run est un second run pour la date 2026-05-20. Le premier run (2026-05-20T23:16) avait déjà traité tous les emails et calls de la journée. Ce run a trouvé les mêmes données et a créé un doublon du fichier CGD en Drive (non suppressible par règle absolue).

### Emails traités (1)

| Direction | Client | Sujet | Fichier Drive |
|-----------|--------|-------|---------------|
| outbound | Caixa Geral de Depositos | Re: CUBE AI - Prevenção de fraude precoce com IA | Clients/Caixa Geral de Depositos/emails/2026-05-20_re-cube-ai-prevencao-de-fraude-precoce-com-ia.txt (doublon) |

### Classifications en attente

Aucune nouvelle classification requise.
