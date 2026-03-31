# Inbox — Canal de communication inter-agents

Ce dossier est le canal de communication entre Claude, Tasklet, et Antoine.

## Conventions de nommage

| Fichier | Créé par | Lu par | Usage |
|---|---|---|---|
| `for-claude-YYYY-MM-DD.md` | Tasklet | Claude (début de session) | Questions, contexte, tâches à traiter |
| `for-tasklet-YYYY-MM-DD.md` | Claude | Tasklet | Instructions, tâches async, follow-ups |
| `for-antoine-YYYY-MM-DD.md` | Claude ou Tasklet | Antoine (via digest Tasklet) | Alertes, décisions nécessaires, blocages |

## Règles

- **Claude** : en début de session significative, lire les fichiers `for-claude-*.md` non traités. En fin de session, marquer comme traité (`[DONE]` en première ligne) ou archiver.
- **Tasklet** : lire `for-tasklet-*.md` à chaque cycle. Relayer `for-antoine-*.md` dans le digest matinal.
- **Rétention** : les fichiers inbox de plus de 7 jours peuvent être archivés dans `inbox/archive/`.
- **Pas de spam** : ne créer un fichier inbox que si une action ou décision est réellement nécessaire de l'autre côté.
