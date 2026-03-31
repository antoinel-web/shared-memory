# Collaboration Protocol — Claude + Tasklet

Ce document définit comment Claude et Tasklet travaillent ensemble via la shared memory.

---

## Architecture

```
Tasklet (async, proactif, scheduler)
    ↕ lit/écrit
shared-memory repo (mémoire commune)
    ↕ lit/écrit
Claude (sync, sessions à la demande)
```

---

## Responsabilités

### Claude
- **Début de session** : lire `inbox/for-claude-*.md` (fichiers non traités)
- **Fin de session significative** : écrire dans `logs/claude-observations.md`
- **Si action async nécessaire** : créer `inbox/for-tasklet-YYYY-MM-DD.md`
- **Si Antoine doit être alerté hors session** : créer `inbox/for-antoine-YYYY-MM-DD.md` que Tasklet relaiera

### Tasklet
- **Chaque cycle** : lire `inbox/for-tasklet-*.md` et exécuter
- **Morning digest** : inclure les `inbox/for-antoine-*.md` en attente
- **Après chaque run significatif** : écrire dans `logs/tasklet-observations.md`
- **Si Claude doit savoir quelque chose** : créer `inbox/for-claude-YYYY-MM-DD.md`

---

## Apprentissage mutuel

### Ce que Claude apprend de Tasklet
- Lit `logs/tasklet-observations.md` → comprend les patterns comportementaux d'Antoine observés en async
- Lit les `inbox/for-claude-*.md` → reçoit le contexte récent que Tasklet a collecté

### Ce que Tasklet apprend de Claude
- Lit `logs/claude-observations.md` → intègre les feedbacks et corrections issus des sessions
- Lit les `inbox/for-tasklet-*.md` → reçoit des instructions spécifiques post-session

### Ce qu'Antoine arbitre
- Tout `[AMEND]` dans `logs/` nécessite une approbation explicite avant application
- Les fichiers `inbox/for-antoine-*.md` sont des escalades — pas des notifications routinières

---

## Signal d'escalade vers Antoine

Utiliser `inbox/for-antoine-YYYY-MM-DD.md` uniquement si :
- Une décision ne peut pas être prise sans lui
- Un deal a un signal fort (réponse prospect, stall critique, deadline imminente)
- Un conflit entre les observations de Claude et Tasklet nécessite arbitrage

---

## Principe

La shared memory n'est pas un journal. C'est un système nerveux.
Chaque écriture doit être utile à l'autre agent ou à Antoine.
Le silence est préférable au bruit.
