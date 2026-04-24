# Agent Architecture — Reference Document
*Canonical source of truth for the multi-agent system. All agents read this.*
*Last updated: 2026-04-24*

---

## System overview

This system is a collection of AI agents deployed in Tasklet, working together to support Antoine Lecouturier's daily workflow as Account Executive at Cube3/Cube AI. The agents communicate through shared infrastructure, not directly with each other.

### Design principles

1. **Salesforce is the structured truth.** MEDDPICC, stages, amounts, contacts — all live in Salesforce. No agent contradicts it without human validation.
2. **The panneau de signalisation informs, it does not command.** Deal Pulse publishes pipeline state. Consumer agents decide what to do with it. No centralized orchestration.
3. **Feedback flows upward.** Signals from Antoine's real actions (modified drafts, manual SF edits, ignored recommendations) are captured and fed back into the system.
4. **Human validation before external action.** No agent sends an email, writes to Salesforce, or contacts a prospect without Antoine's explicit approval.
5. **One author per state file.** Each file in `state/` has exactly one agent that writes it. Readers never modify what they read.

---

## Roles

### Collecteurs
Go to the field (Gmail, Granola, Calendar, Slack, LinkedIn...), extract relevant data, write to bases de vérité.

**Current agents:**
- **Pipeline Scan** — Scans Gmail + Granola + Calendar + Slack weekly, scores MEDDPICC, proposes Salesforce updates. Writes: Salesforce (after validation).
- **Meeting Echo** — Transforms Granola meeting transcripts into follow-up email drafts. Writes: Gmail drafts.
- **Feedback Collector** *(to build)* — Compares agent proposals with Antoine's actual actions. Reads: Gmail sent, Salesforce audit trail, agent outputs. Writes: `state/feedback-log-latest.md`.

**Rules for collecteurs:**
- Always cite the source (permalink, message-id, event-id) for every piece of data written.
- Never write to `state/` except the Feedback Collector writing to its own log.
- Internal-only interactions (no external participant) are silently ignored.

### Bases de vérité
Long-term memory. Three stores, each with a distinct scope:

| Store | Scope | Status |
|---|---|---|
| Salesforce | Structured client data (MEDDPICC, stages, amounts, contacts) | Active |
| Client database | Unstructured context (transcripts, emails, meeting notes) | Future |
| Feedback log | Deltas between agent proposals and Antoine's real actions | To build |

### Panneau de signalisation
Reads bases de vérité, publishes a consolidated view of pipeline state. Does NOT give orders — agents downstream decide autonomously what to act on.

**Current agent:**
- **Deal Pulse** — Publishes daily pipeline report. Reads: Salesforce. Writes: `state/pipeline-state-latest.md` + `state/current/pipeline-state-YYYY-MM-DD.md`.

**Rules for the panneau:**
- Publish what you produce today, verbatim. No enforced schema beyond the mandatory header.
- Every publication starts with the standard header (see Communication rules below).
- One panneau agent per state file. If a second consolidated view is needed, it gets its own file.

### Exécutants
Read the panneau, enrich with primary sources, produce a deliverable. Each exécutant decides autonomously which deals to act on based on the panneau + its own rules.

**Current agents:**
- **Follow-Up Forge v2** — Reads Deal Pulse, enriches via Gmail/Granola/Calendar, produces contextual follow-up email drafts. Writes: Gmail drafts.

**Future agents (reserved slots):**
- **Coach** — Pre-meeting briefs with compiled context.
- **Débloqueur** — Reactivation strategies for stalled deals.

**Rules for exécutants:**
- Always read `state/pipeline-state-latest.md` as primary input.
- Check `published_at` in the header. Apply your own staleness tolerance (e.g. Forge v2: reject if > 24h).
- Enrich with primary sources (Gmail, Granola, Calendar) for the details needed to produce your deliverable. The panneau carries what's needed to DECIDE. You carry what's needed to ACT.
- Never write to Salesforce. Never write to `state/`. Errors found go to `inbox/for-deal-pulse-YYYY-MM-DD.md`.
- Signal feedback by writing to `inbox/for-feedback-collector-YYYY-MM-DD.md` or by leaving traces in Gmail (sent vs draft delta).

### Gardien
Maintains system hygiene. No business logic, no client-facing output.

**Current agent:**
- **State Janitor** *(to build)* — Consolidates daily state files into weekly summaries, weekly into monthly, cleans archives older than 90 days.

**Rules for gardiens:**
- Operate only on `state/` and `logs/`.
- Never read or write Salesforce.
- Never produce client-facing output.

### Observateur
Reads everything (state, inbox, feedback log, logs), produces improvement proposals for Antoine. Never acts, never writes to operational systems.

**Future agent:**
- **Enhancer** *(to build later)* — Detects patterns in feedback, proposes prompt amendments, calibration changes, architecture evolution.

**Rules for observateurs:**
- Read-only on all operational systems.
- Write proposals only to `inbox/for-antoine-YYYY-MM-DD.md`.
- Never modify another agent's prompt, state file, or output.

---

## Communication rules

### state/ — Published state (pull-based)
```
state/
├── pipeline-state-latest.md        (Deal Pulse publishes, everyone reads)
├── feedback-log-latest.md          (Feedback Collector publishes, Enhancer reads)
├── current/                        (dated daily copies, audit only)
│   ├── pipeline-state-YYYY-MM-DD.md
│   └── feedback-log-YYYY-MM-DD.md
└── archive/                        (weekly/monthly summaries by State Janitor)
    ├── YYYY-WNN-weekly-summary.md
    └── YYYY-MM-monthly-summary.md
```

**Mandatory header** for every file in `state/`:
```yaml
---
published_by: [agent-name]
published_at: [ISO-8601 with timezone]
source_freshness:
  [source]: [timestamp or FAILED]
schema_version: 1.0
---
```

**Rules:**
- One author per file. No exceptions.
- `*-latest.md` is always the current version. Consumers read only this.
- Dated files in `current/` are immutable once written.
- State Janitor handles archival and cleanup.

### inbox/ — Point-to-point messages (push-based)
```
inbox/
├── for-antoine-YYYY-MM-DD.md       (escalations, proposals, alerts)
├── for-deal-pulse-YYYY-MM-DD.md    (error reports from consumers)
├── for-feedback-collector-YYYY-MM-DD.md  (signals from exécutants)
└── for-enhancer-YYYY-MM-DD.md      (breaking changes, anomalies)
```

**Rules:**
- Use only for exceptions, escalations, and error reports.
- Never use for regular data flow (that goes through `state/`).
- Any agent can write. Only the named recipient reads.

### logs/ — Historical record
Observations, digests, execution traces. Append-only. No agent depends on logs for its core workflow — logs are for debugging and for the Enhancer.

---

## Agent identity card template

Every agent prompt MUST include this section:

```
[ARCHITECTURE]
Role: [Collecteur / Exécutant / Gardien / Observateur]
Reads: [list of sources: state files, Salesforce, Gmail, Granola, etc.]
Writes: [list of destinations: Salesforce, Gmail drafts, state/, inbox/]
Never touches: [explicit exclusions]
Feedback: [how this agent's output feeds back into the system]
Depends on: [upstream agents or state files, with staleness tolerance]
Architecture ref: https://github.com/antoinel-web/shared-memory/blob/main/context/ARCHITECTURE.md
```

---

## Agent registry

| Agent | Role | Status | Reads | Writes |
|---|---|---|---|---|
| Pipeline Scan | Collecteur | Active | Gmail, Granola, Calendar, Slack, Salesforce | Salesforce (after validation) |
| Meeting Echo | Collecteur | Active | Granola | Gmail drafts |
| Feedback Collector | Collecteur | To build | Gmail sent, SF audit, agent outputs | state/feedback-log-latest.md |
| Deal Pulse | Panneau | Active | Salesforce | state/pipeline-state-latest.md |
| Follow-Up Forge v2 | Exécutant | Active | state/pipeline-state-latest.md, Gmail, Granola, Calendar | Gmail drafts |
| Coach | Exécutant | Future | state/pipeline-state-latest.md, Granola, Calendar | Slack or Gmail |
| Débloqueur | Exécutant | Future | state/pipeline-state-latest.md, Salesforce history | Gmail drafts |
| State Janitor | Gardien | To build | state/current/, state/archive/ | state/archive/ |
| Enhancer | Observateur | Future | state/, inbox/, logs/, feedback-log | inbox/for-antoine |

---

## Evolution policy

### Adding a new agent
1. Classify it in an existing role during Phase 1c of the build protocol.
2. Add its `[ARCHITECTURE]` identity card to the prompt.
3. Update the Agent Registry table above.
4. If it writes to `state/`, create its dedicated file (one author per file rule).

### Changing the architecture
- **Minor** (new agent in existing role, new state file): update this document + registry. No approval needed beyond Antoine's normal agent validation.
- **Major** (new role, changed communication rules, changed bases de vérité): requires explicit discussion in a dedicated chat. Update this document, the project protocol, and notify all affected agents via `inbox/`.

### Deprecating an agent
1. Mark as "Deprecated" in the registry.
2. Stop its Tasklet schedule.
3. Its state files remain readable for 30 days, then State Janitor archives them.
4. Remove from registry after 30 days.
