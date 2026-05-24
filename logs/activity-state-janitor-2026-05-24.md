---
agent: state-janitor
run_at: 2026-05-24T23:01:00+02:00
status: success
sources_read:
  - state/current: 2 files read
  - logs: 5 digest files read
  - activity reports: 24 files scanned (last 7 days)
  - ARCHITECTURE.md: read
outputs_written:
  - state/archive/2026-W21-pipeline-state-weekly.md: weekly pipeline summary W21 (2 sources, 29 deals)
  - state/archive/2026-W21-logs-weekly.md: weekly logs summary W21 (5 digest sources)
  - logs/activity-state-janitor-2026-05-24.md: this activity report
errors: null
duration_estimate: medium
---

## Détail du run

### Périmètre
- Schedule: weekly
- Today: 2026-05-24 (dimanche, semaine ISO W21)
- Semaine courante: 2026-05-18 (lundi) → 2026-05-24 (dimanche)

### Fichiers consolidés

**Pipeline state (state/current/):**
- pipeline-state-2026-05-19.md → consolidé
- pipeline-state-2026-05-22.md → consolidé
- Orphelins: aucun

**Digests (logs/):**
- digest-2026-05-18.md → consolidé
- digest-2026-05-19.md → consolidé
- digest-2026-05-20.md → consolidé
- digest-2026-05-21.md → consolidé
- digest-2026-05-22.md → consolidé

### Fichiers supprimés
7 fichiers source supprimés après vérification des summaries.

### Registre des agents — écarts détectés
- **bluedot-logger**: actif (3 reports: 20-22 mai) mais absent du registre → Tasklet task créée
- **gtm-sync**: actif (1 report: 17 mai) mais absent du registre → Tasklet task créée
- **monday-digest**: actif (1 report: 18 mai) mais absent du registre → Tasklet task créée
- **AE Salesforce Operator**: registré Active mais aucune activité depuis 2026-04-28 (26 jours) → flag Inactive? → Tasklet task créée

Tasklet task créée: [State Janitor] Écarts détectés dans le registre — semaine W21
