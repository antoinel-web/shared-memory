# Claude Observations Log

Fichier de log des observations et feedbacks issus des sessions Claude Desktop.
Lu par Tasklet via le morning-memory-sync. Mis à jour par Claude en fin de session significative.

## Format

```
[OBSERVE] date: YYYY-MM-DD | task: <description> | outcome: success/partial/failure | detail: <one phrase>
[FEEDBACK] date: YYYY-MM-DD | pattern: <ce qu'Antoine a corrigé ou redemandé> | instruction: <ce que Claude doit faire différemment>
[SKILL] date: YYYY-MM-DD | skill: <nom> | observation: <ce qui marche bien ou mal>
```

**Règle** : ne logger que les sessions avec un signal utile. Pas les échanges conversationnels routiniers.

---

## Entrées

[OBSERVE] date: 2026-03-31 | task: setup claude-observations.md + shared memory loop | outcome: success | detail: Antoine a confirmé que Tasklet tourne (morning-memory-sync actif, digests présents), gap identifié = claude-observations.md inexistant malgré instruction dans profile.md — fichier créé et loop activée.

[FEEDBACK] date: 2026-03-31 | pattern: Claude ne loggait pas ses sessions malgré l'instruction dans profile.md | instruction: en fin de toute session produisant un output significatif (deck, email, analyse, chart, agent design), écrire une entrée OBSERVE dans ce fichier via GitHub tool.
