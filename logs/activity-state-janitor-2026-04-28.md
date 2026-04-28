---
agent: state-janitor
run_at: 2026-04-28T13:38:00+02:00
status: partial
sources_read:
  - state/current: 1 file read (pipeline-state-2026-04-24.md; 2026-04-27 and 2026-04-28 are current week, skipped)
  - logs: 8 digest files read (digest-2026-03-29, digest-2026-03-30 through 2026-04-05)
  - activity reports: 5 files listed (ae-salesforce-operator x1, deal-pulse x1, follow-up-forge-v2 x3)
  - ARCHITECTURE.md: read
outputs_written:
  - state/archive/2026-W17-pipeline-state-weekly.md: pipeline state summary for W17 (26 deals, 1 source file)
  - state/archive/2026-W13-logs-weekly.md: logs summary for W13 (1 source file)
  - state/archive/2026-W14-logs-weekly.md: logs summary for W14 (7 source files)
  - logs/activity-state-janitor-2026-04-28.md: this report
errors: "Partial run: digest weeks W15, W16, W17 (22 files) NOT processed — MAX_DIGEST_FILES_PER_RUN (10) reached after W13+W14. Will be consolidated in next run."
duration_estimate: medium
---

## Détail du run

### Périmètre traité
- **Schedule:** weekly
- **TODAY:** 2026-04-28 (semaine courante: 2026-W18, lun 2026-04-27 → dim 2026-05-03)
- **Pipeline orphelins consolidés:** 1 fichier (2026-W17)
- **Digests orphelins consolidés:** 8 fichiers sur 22 (2026-W13: 1, 2026-W14: 7)
- **Digests restants (report semaine prochaine):** W15 (5 fichiers), W16 (5 fichiers), W17 digest (4 fichiers)

### Registre — Écarts détectés
- `ae-salesforce-operator` : actif (rapport d'activité 2026-04-28) mais **absent du registre** → tâche créée pour ajout
- `state-janitor` : statut "To build" mais première exécution effective → tâche créée pour mise à jour statut vers "Active"
- `pipeline-scan` : statut "Active" mais **aucun rapport d'activité dans les 7 derniers jours** → tâche créée pour vérification statut
- `meeting-echo` : statut "Active" mais **aucun rapport d'activité dans les 7 derniers jours** → tâche créée pour vérification statut

### Fichiers source supprimés
- state/current/pipeline-state-2026-04-24.md ✅
- logs/digest-2026-03-29.md ✅
- logs/digest-2026-03-30.md ✅
- logs/digest-2026-03-31.md ✅
- logs/digest-2026-04-01.md ✅
- logs/digest-2026-04-02.md ✅
- logs/digest-2026-04-03.md ✅
- logs/digest-2026-04-04.md ✅
- logs/digest-2026-04-05.md ✅
