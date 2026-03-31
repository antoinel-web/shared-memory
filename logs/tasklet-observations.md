# Tasklet Observations Log

Fichier de log des observations issues des agents Tasklet.
Lu par Claude en début de session. Alimenté par Tasklet en continu.

## Format

```
[OBSERVE] date: YYYY-MM-DD | task: <description> | outcome: success/partial/failure | detail: <one phrase>
[FEEDBACK] date: YYYY-MM-DD | pattern: <comportement observé chez Antoine> | instruction: <ce que Tasklet doit faire différemment>
[SIGNAL] date: YYYY-MM-DD | account: <nom> | event: <ce qui s'est passé> | action_needed: yes/no
```

## Règle
Ne logger que les sessions avec un signal utile. Pas les runs routiniers sans anomalie.

---

## Entrées

_(Tasklet commence à alimenter ce fichier dès le prochain cycle)_
