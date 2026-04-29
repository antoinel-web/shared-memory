---
agent: state-janitor
run_at: 2026-04-29T19:09:00+02:00
status: partial
sources_read:
  - state/current: 2 files read (pipeline-state-2026-04-27.md, pipeline-state-2026-04-28.md)
  - logs: 10 files read (digest-2026-04-06 à 04-10 ; digest-2026-04-13 à 04-17)
  - activity reports: 9 files vu dans listings (last 7 days)
  - ARCHITECTURE.md: read
outputs_written:
  - state/archive/2026-W15-logs-weekly.md: consolidation logs semaine W15 (5 digests, 2026-04-06 à 10)
  - state/archive/2026-W16-logs-weekly.md: consolidation logs semaine W16 (5 digests, 2026-04-13 à 17)
  - state/archive/2026-W18-pipeline-state-weekly.md: consolidation pipeline W18 partielle (2 fichiers, semaine en cours)
  - logs/activity-state-janitor-2026-04-29.md: ce rapport
errors: null
duration_estimate: medium
---

## Détail du run

### Fichiers consolidés
- **Digest W15** (orphelins) : 5 fichiers → `state/archive/2026-W15-logs-weekly.md` ✓
- **Digest W16** (orphelins) : 5 fichiers → `state/archive/2026-W16-logs-weekly.md` ✓
- **Pipeline W18** (semaine en cours) : 2 fichiers → `state/archive/2026-W18-pipeline-state-weekly.md` ✓

### Fichiers supprimés après vérification
- `logs/digest-2026-04-06.md` à `logs/digest-2026-04-10.md` (5 fichiers W15)
- `logs/digest-2026-04-13.md` à `logs/digest-2026-04-17.md` (5 fichiers W16)
- `state/current/pipeline-state-2026-04-27.md` et `pipeline-state-2026-04-28.md` (2 fichiers W18)

### Raison du statut « partial »
- **Digest W17** (orphelins : 2026-04-21, 04-22, 04-23, 04-24) : non traités — limite MAX_DIGEST_FILES_PER_RUN=10 atteinte ce run (W15 + W16 = 10 fichiers).
- **Digest W18** (2026-04-27, 04-28, 04-29) : non traités pour la même raison.
- Ces fichiers seront consolidés au prochain run.

### Registre agents — écarts détectés
- **daily-data-logger** : rapport d'activité du 2026-04-28 trouvé mais agent absent du registre ARCHITECTURE.md. Tasklet task créée.
- **Pipeline Scan** : enregistré Active mais aucun rapport d'activité dans les 7 derniers jours.
- **Feedback Collector** : enregistré Active (Phase 1) mais aucun rapport d'activité dans les 7 derniers jours.
- Task Tasklet créée : `[State Janitor] Écarts détectés dans le registre — semaine W18`
