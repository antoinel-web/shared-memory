---
agent: state-janitor
run_at: 2026-06-14T23:00:00+02:00
status: success
sources_read:
  - state/current: 2 files read (pipeline-state-2026-06-09.md, pipeline-state-2026-06-12.md)
  - logs: 5 digest files read (digest-2026-06-08 to digest-2026-06-12)
  - activity reports: 24 files scanned (last 7 days, 2026-06-08 to 2026-06-14)
  - ARCHITECTURE.md: read
outputs_written:
  - state/archive/2026-W24-pipeline-state-weekly.md: consolidation pipeline semaine W24 (2 sources, 27 deals → 24 deals, évolutions HOT/COLD)
  - state/archive/2026-W24-logs-weekly.md: consolidation digests semaine W24 (5 sources, mode routine stable)
  - logs/activity-state-janitor-2026-06-14.md: rapport d'activité
errors: null
duration_estimate: medium

## Détail des opérations

- Semaine traitée : 2026-W24 (2026-06-08 Lun → 2026-06-14 Dim)
- Orphelins : aucun (tous les fichiers datés sont dans la semaine courante)
- Pipeline state files consolidés : 2/7 (sous MAX_PIPELINE_STATE_FILES_PER_RUN)
- Digest files consolidés : 5/10 (sous MAX_DIGEST_FILES_PER_RUN)
- Fichiers source supprimés : 7 (2 pipeline + 5 digest)
- Circuit breaker : non déclenché (0 erreurs GitHub)

## Registre agents — écarts détectés

3 écarts détectés — tâche Tasklet créée (id: at_hpn9eg99zzgw0k6bgdx5) :
1. **bluedot-logger** : actif (activité les 2026-06-08, 06-09, 06-11) mais absent du registre → proposer ajout status Active
2. **monday-digest** : actif (activité le 2026-06-08) mais absent du registre → proposer ajout status Active
3. **AE Salesforce Operator** : enregistré Active mais aucune activité depuis 2026-04-28 (>7j) → proposer passage Inactive?
---
