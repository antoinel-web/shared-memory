---
agent: state-janitor
run_at: 2026-05-31T23:00:00+02:00
status: success
sources_read:
  - state/current: 4 files read
  - logs: 5 digest files read
  - activity reports: counted for registry scan (last 7 days)
  - ARCHITECTURE.md: read
outputs_written:
  - state/archive/2026-W22-pipeline-state-weekly.md: consolidation pipeline state W22 (4 sources)
  - state/archive/2026-W22-logs-weekly.md: consolidation digest logs W22 (5 sources)
  - logs/activity-state-janitor-2026-05-31.md: ce fichier
errors: null
duration_estimate: medium
---

## Run summary

**Schedule:** weekly
**Week consolidated:** 2026-W22 (2026-05-25 to 2026-05-31)
**Files read:** 4 pipeline state + 5 digests + ARCHITECTURE.md
**Files created:** 2 weekly summaries
**Files deleted:** 9 source files (4 pipeline state + 5 digests)

## Registry scan — Discrepancies found

- **bluedot-logger** : présent en activité (May 26, 28, 29) mais absent du registre → à ajouter comme Active
- **monday-digest** : présent en activité (May 25) mais absent du registre → à ajouter comme Active
- **Meeting Echo** : enregistré Active mais aucune activité depuis 2026-05-21 (>7 jours) → à vérifier (Inactive?)
- **AE Salesforce Operator** : enregistré Active mais aucune activité depuis 2026-04-28 (>7 jours) → à vérifier (Inactive?)

Tâche Tasklet créée pour validation humaine : [State Janitor] Écarts détectés dans le registre — semaine W22

## Gaps notés dans les sources

- pipeline-state-2026-05-28.md absent (jeudi de la semaine W22)
