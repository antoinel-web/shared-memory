---
agent: state-janitor
run_at: 2026-05-03T23:10:00+02:00
status: success
sources_read:
  state/current: 2
  logs: 10
  activity_reports: 25
  ARCHITECTURE.md: read
outputs_written:
  - state/archive/2026-W17-logs-weekly.md: créé (4 digests W17 consolidés)
  - state/archive/2026-W18-logs-weekly.md: créé (6 digests W18 consolidés)
  - state/archive/2026-W18-pipeline-state-weekly.md: mis à jour (résumé partiel Apr 27-28 complété avec Apr 30 + May 1)
files_deleted:
  - logs/digest-2026-04-21.md
  - logs/digest-2026-04-22.md
  - logs/digest-2026-04-23.md
  - logs/digest-2026-04-24.md
  - logs/digest-2026-04-27.md
  - logs/digest-2026-04-28.md
  - logs/digest-2026-04-29.md
  - logs/digest-2026-04-30.md
  - logs/digest-2026-05-01.md
  - logs/digest-2026-05-02.md
  - state/current/pipeline-state-2026-04-30.md
  - state/current/pipeline-state-2026-05-01.md
registry_scan:
  status: no_discrepancies
  active_agents_detected: [ae-salesforce-operator, daily-data-logger, deal-pulse, feedback-collector, follow-up-forge-v2, meeting-echo, pipeline-scan, state-janitor]
  new_agents: none
  inactive_agents: none
  status_upgrades: none
errors: null
duration_estimate: medium
---

## Run summary

Run hebdomadaire du 2026-05-03 (Dimanche W18, fin de semaine).

### Consolidations effectuées

**Orphans W17 (2026-04-20 → 2026-04-26) :**
- 4 fichiers digest consolidés → `state/archive/2026-W17-logs-weekly.md`
- Aucun fichier pipeline-state pour W17.

**Semaine courante W18 (2026-04-27 → 2026-05-03) :**
- 6 fichiers digest consolidés → `state/archive/2026-W18-logs-weekly.md`
- 2 fichiers pipeline-state (Apr 30, May 1) consolidés et fusionnés avec le résumé partiel existant → `state/archive/2026-W18-pipeline-state-weekly.md` (mis à jour, 4 sources au total)

### Changement notable détecté en W18
- BGL (BNP Luxembourg) : avancement de stage Pre-Opportunity → AVT entre le 2026-04-28 et le 2026-04-30.

### Registre des agents
- 8 agents actifs détectés dans les rapports d'activité des 7 derniers jours.
- Tous enregistrés et marqués Active dans ARCHITECTURE.md.
- Aucune discordance à signaler.

### Nettoyage
- 10 digests supprimés (W17 : 4, W18 : 6).
- 2 fichiers pipeline-state supprimés (Apr 30, May 1).
- Total : 12 fichiers source supprimés après vérification des résumés.
