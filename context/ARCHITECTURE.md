# Agent Architecture — Reference Document
*Canonical source of truth for the multi-agent system. All agents read this.*
*Last updated: 2026-04-29*

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
- **Meeting Echo** — Transforms Granola meeting transcripts into follow-up email drafts. Writes: Gmail drafts. **Must log every draft produced** in `logs/drafts/meeting-echo-YYYY-MM-DD.md` for Feedback Collector consumption.
- **Feedback Collector** — Compares agent proposals (logged drafts) with Antoine's actual actions (Gmail sent). Categorizes deltas and writes structured feedback. Phase 1: emails only (Forge v2 + Meeting Echo). Phase 2 auto-detected: presentations + Deal Pulse corrections. Reads: `logs/drafts/` (GitHub), Gmail sent. Writes: `state/feedback-log-latest.md` (14-day sliding window), `logs/feedback-trends.md` (append-only trend aggregation). Schedule: daily, evening.

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
| Feedback log | Deltas between agent proposals and Antoine's real actions (14-day sliding window) | Active |

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
- **Follow-Up Forge v2** — Reads Deal Pulse + `state/feedback-log-latest.md` (to learn from past deltas), enriches via Gmail/Granola/Calendar, produces contextual follow-up email drafts. Writes: Gmail drafts. **Must log every draft produced** in `logs/drafts/forge-v2-YYYY-MM-DD.md` for Feedback Collector consumption.
- **AE Salesforce Operator** — Triggered by Pipeline Scan. Reads Slack, Gmail, Granola, Calendar, Salesforce. Updates Salesforce opportunities and activity tasks, posts Slack nudges. Writes: Salesforce (opps + tasks), Slack (#sales-inbox).

**Future agents (reserved slots):**
- **Coach** — Pre-meeting briefs with compiled context + call analysis.
- **Débloqueur** — Reactivation strategies for stalled deals.

**Rules for exécutants:**
- Always read `state/pipeline-state-latest.md` as primary input.
- Always read `state/feedback-log-latest.md` before producing output, to learn from recent deltas.
- Check `published_at` in the header. Apply your own staleness tolerance (e.g. Forge v2: reject if > 24h).
- Enrich with primary sources (Gmail, Granola, Calendar) for the details needed to produce your deliverable. The panneau carries what's needed to DECIDE. You carry what's needed to ACT.
- Never write to Salesforce. Never write to `state/`. Errors found go to `inbox/for-deal-pulse-YYYY-MM-DD.md`.
- Signal feedback by writing to `inbox/for-feedback-collector-YYYY-MM-DD.md` or by leaving traces in Gmail (sent vs draft delta).

### Gardien
Maintains system hygiene. No business logic, no client-facing output.

**Current agent:**
- **State Janitor** — Consolidates daily state files into weekly summaries, weekly into monthly, cleans archives older than 90 days. Scans agent registry for discrepancies. Runs weekly (Sunday 23h) and monthly (1st of month).

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
├── feedback-log-latest.md          (Feedback Collector publishes, 14-day sliding window)
│                                    (Forge v2 + Meeting Echo read before each run)
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
├── for-enhancer-YYYY-MM-DD.md      (breaking changes, anomalies)
└── deal-pulse/                     (Antoine's explicit corrections on Deal Pulse output)
```

**Rules:**
- Use only for exceptions, escalations, and error reports.
- Never use for regular data flow (that goes through `state/`).
- Any agent can write. Only the named recipient reads.
- `inbox/deal-pulse/` is a special folder: Antoine writes corrections, Feedback Collector reads (Phase 2).

### logs/ — Historical record
Observations, digests, execution traces. Append-only. No agent depends on logs for its core workflow — logs are for debugging and for the Enhancer.

```
logs/
├── drafts/                          (agent output logging for Feedback Collector)
│   ├── forge-v2-YYYY-MM-DD.md      (Forge v2 logs each draft produced)
│   ├── meeting-echo-YYYY-MM-DD.md  (Meeting Echo logs each draft produced)
│   └── presentations/              (Phase 2: Claude project logs presentation iterations)
│       └── [client]-YYYY-MM-DD.md
├── feedback-trends.md              (append-only trend aggregation from Feedback Collector)
├── activity-[agent]-YYYY-MM-DD.md  (execution reports, one per agent per day)
└── ...
```

**Draft log format** (for Forge v2 and Meeting Echo):
Each draft logged must include: agent name, timestamp, recipient(s), subject, content summary. No verbatim client data — summaries only. These logs are the "proposed" side of the Feedback Collector's comparison.

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
| Meeting Echo | Collecteur | Active | Granola | Gmail drafts, logs/drafts/meeting-echo-YYYY-MM-DD.md |
| Feedback Collector | Collecteur | Active (Phase 1) | logs/drafts/ (GitHub), Gmail sent, [Phase 2: presentations logs, inbox/deal-pulse/] | state/feedback-log-latest.md, logs/feedback-trends.md |
| Deal Pulse | Panneau | Active | Salesforce | state/pipeline-state-latest.md |
| Follow-Up Forge v2 | Exécutant | Active | state/pipeline-state-latest.md, state/feedback-log-latest.md, Gmail, Granola, Calendar | Gmail drafts, logs/drafts/forge-v2-YYYY-MM-DD.md |
| AE Salesforce Operator | Exécutant | Active | Slack, Gmail, Granola, Calendar, Salesforce | Salesforce (opps + tasks), Slack (#sales-inbox) |
| Coach | Exécutant | Future | state/pipeline-state-latest.md, Granola, Calendar, logs/feedback-trends.md | Slack or Gmail |
| Débloqueur | Exécutant | Future | state/pipeline-state-latest.md, Salesforce history | Gmail drafts |
| State Janitor | Gardien | Active | state/current/, state/archive/, logs/ | state/archive/, logs/ |
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
