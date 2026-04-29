# Follow-Up Forge v2 — System Design

*Version: 1.0 — 23 April 2026*

---

## 1. Architecture Overview

Follow-Up Forge v2 is a **daily automated agent** that produces Gmail draft follow-up emails for Antoine Lecouturier's active deals. It runs as a scheduled trigger on this Tasklet agent, orchestrated through subagents for modularity and context efficiency.

### Core Principle
> **Always lead with evidence.** No "just checking in" — every draft brings concrete value, or silence is the deliverable.

### Execution Model
```
┌─────────────────────────────────────────────────────────────┐
│                    DAILY TRIGGER (8:15 CET)                 │
│                  Weekdays only (Mon-Fri)                    │
└───────────────────────────┬─────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│              ORCHESTRATOR (Main Agent)                       │
│  • Receives trigger event                                   │
│  • Checks Antoine OOO status (Google Calendar)              │
│  • Calls Deal Pulse for actionable deals                    │
│  • Dispatches per-deal processing to subagent               │
│  • Tracks daily cap (max 10 drafts)                         │
│  • Runs J+1 learning loop (feedback analysis)               │
│  • Sends notifications on errors/divergences                │
└───────────────────────────┬─────────────────────────────────┘
                            │
              ┌─────────────┼──────────────┐
              ▼             ▼              ▼
     ┌──────────────┐ ┌──────────┐ ┌─────────────┐
     │ Deal Process │ │ Learning │ │ Notification│
     │  Subagent    │ │ Subagent │ │  (built-in) │
     │ (per deal)   │ │ (J+1)    │ │             │
     └──────────────┘ └──────────┘ └─────────────┘
```

---

## 1b. Multi-Agent Architecture

```
[ARCHITECTURE]
Role: Exécutant
Reads: Deal Pulse report (current: direct call; future: state/pipeline-state-latest.md), Gmail (threads, sent history, drafts), Granola (recent meetings), Salesforce (read-only cross-check: MEDDPICC, champion, next steps), Slack (client channels), Google Calendar (meetings, OOO, holidays), Google Drive (deal trackers, briefing_hebdo), BigQuery (anonymized metrics)
Writes: Gmail drafts (follow-up emails — never sent directly)
Never touches: Salesforce (no write), state/ (no write), inbox/ (no write)
Feedback: Deltas between draft produced and email actually sent by Antoine are captured by the Feedback Collector agent. Errors found in Deal Pulse data go to inbox/for-deal-pulse-YYYY-MM-DD.md
Depends on: Deal Pulse (staleness tolerance: 24h — if report older than 24h, email Antoine + abort run)
Architecture ref: https://github.com/antoinel-web/shared-memory/blob/main/context/ARCHITECTURE.md
```

---

## 2. Connections & Tools Inventory

### 2.1 Connection Map

| Service | Connection ID | Type | Role in Forge v2 |
|---|---|---|---|
| **Gmail** | `conn_k0t1758ymakkc329yyvp` | Integration | Read sent emails + threads; Create drafts; Read drafts for learning loop |
| **Granola** | `conn_a6kdbz33vpd2ed5np1g9` | Integration | Read meeting transcripts for next-step cross-check |
| **Salesforce** | `conn_gezqj3ebq09rrpzsmey8` | Direct API (HTTP) | Read-only cross-check (MEDDPICC fields, champion, next steps) |
| **Slack** | `conn_3c0rm7dd5dh7864y39jd` | Integration | Read client channels for internal context |
| **Google Calendar** | `conn_tpfxd4qqwq1f23rsxzqt` | Integration | Read meetings (upcoming/past), OOO events, holidays |
| **Google Drive** | `conn_v1mbff73hy3vh3f013he` | Integration | Read deal trackers, POC results, briefing_hebdo files |
| **BigQuery** | `conn_3n3cpcawq8wtjcevtnw6` | Integration | Read anonymized metrics from `production-20230612.apex_for_statistics.actual_financial_institutions` |

### 2.2 Tools to Activate

| Connection | Tools Needed |
|---|---|
| **Gmail** | `gmail_search_threads`, `gmail_get_threads`, `gmail_get_messages`, `gmail_create_draft`, `gmail_list_drafts`, `gmail_get_draft` |
| **Granola** | `query_granola_meetings`, `list_meetings`, `get_meetings`, `get_meeting_transcript` |
| **Salesforce** | `remote_http_call` (single tool, used for SOQL queries) |
| **Slack** | `slack_list_channels`, `slack_search_messages`, `slack_get_conversation_history` |
| **Google Calendar** | `google_calendar_search_events` |
| **Google Drive** | `google_drive_search_documents`, `google_drive_get_document`, `google_drive_get_spreadsheet` |
| **BigQuery** | `bigquery_run_query`, `bigquery_get_table_schema` |

**Total: 19 tools across 7 connections.**

---

## 3. Deal Pulse Integration

### ⚠️ OPEN QUESTION — Critical Path

Deal Pulse is described as an **existing Tasklet agent** that serves as the primary data source. We need to clarify how Forge v2 reads from Deal Pulse.

**Possible mechanisms (need Antoine's input):**
1. **Shared Google Sheet/Doc** — Deal Pulse writes its daily output to a Google Sheet that Forge v2 reads via Google Drive
2. **Shared filesystem** — Deal Pulse writes JSON/MD files to a known path in Google Drive
3. **Salesforce as proxy** — Deal Pulse's output is reflected in Salesforce fields, so Forge v2 reads from Salesforce directly
4. **Agent-to-agent messaging** — Deal Pulse emails its output to a known address that Forge v2 reads

**Expected payload per deal from Deal Pulse:**
```json
{
  "deal_name": "BNP × CUBE",
  "salesforce_id": "006xxxxxxxxxxxx",
  "tier": "A",
  "stage_meddpicc": "Business Case & Executive Alignment",
  "next_steps": [
    {
      "description": "Livrer bench Tier-1 pour comité du 10 avril",
      "target_date": "2026-04-10",
      "owner": "Antoine"
    }
  ],
  "last_interactions": {
    "email_out": "2026-04-03",
    "email_in": "2026-04-01",
    "meeting": "2026-03-28",
    "slack": null
  },
  "champion": "Jérôme Palin",
  "stakeholders_recent": ["Adeline Tertulien"],
  "risks": ["deadline_proche"],
  "language": "FR",
  "priority_rank": 1
}
```

---

## 4. Database Schema

The SQL database tracks three core concerns: **draft tracking** (for learning loop), **learning parameters** (adaptive layer), and **signal usage** (non-redundancy).

### 4.1 Draft Tracking
```sql
CREATE TABLE drafts (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  run_date TEXT NOT NULL,           -- YYYY-MM-DD
  deal_name TEXT NOT NULL,
  salesforce_id TEXT,
  tier TEXT NOT NULL,               -- A/B/C/D
  intention TEXT NOT NULL,           -- micro_nudge/next_step_chasing/bridge_handoff/delivery_confirmation/preview_tease/pattern_match_tease
  draft_gmail_id TEXT,               -- Gmail draft ID for tracking
  thread_id TEXT,                    -- Gmail thread ID if reply
  to_addresses TEXT,                 -- JSON array
  cc_addresses TEXT,                 -- JSON array
  subject TEXT,
  body_preview TEXT,                 -- First 200 chars
  language TEXT,                     -- FR/EN/other
  status TEXT DEFAULT 'created',     -- created / sent / sent_edited / rejected / pending
  status_checked_at TEXT,            -- Last time we checked status
  sent_body TEXT,                    -- Final sent body (if edited) for delta analysis
  created_at TEXT DEFAULT (datetime('now'))
);
```

### 4.2 Learning Parameters (Adaptive Layer)
```sql
CREATE TABLE learning_params (
  param_key TEXT PRIMARY KEY,        -- e.g. 'cadence_tier_a_min_days', 'length_tier_b_max_lines'
  param_value TEXT NOT NULL,          -- JSON value
  updated_at TEXT DEFAULT (datetime('now')),
  update_reason TEXT                  -- e.g. 'feedback: too many rejections on Tier B'
);

-- Default values to seed:
-- cadence_tier_a_min_days: 3
-- cadence_tier_a_max_per_week: 2
-- cadence_tier_b_min_days: 10
-- cadence_tier_c_min_days: 20
-- cadence_tier_d_min_days: 20
-- length_tier_a: {"min": 5, "max": 8}
-- length_tier_b: {"min": 3, "max": 5}
-- length_tier_c: {"min": 3, "max": 4}
-- length_tier_d: {"min": 3, "max": 5}
-- intention_priority_order: [1,2,3,4,5,6]
-- style_greeting_fr: "Bonjour"
-- style_closing_fr: "Bien à vous,\nAntoine"
-- style_greeting_en: "Dear"
-- style_closing_en: "Best,\nAntoine"
```

### 4.3 Signal Usage Tracking (Non-Redundancy)
```sql
CREATE TABLE signal_usage (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  signal_source TEXT NOT NULL,        -- briefing_hebdo / bigquery / drive / slack
  signal_id TEXT NOT NULL,            -- Unique identifier for the signal (e.g. briefing date + signal index)
  deal_name TEXT NOT NULL,
  used_date TEXT NOT NULL,
  created_at TEXT DEFAULT (datetime('now')),
  UNIQUE(signal_id, deal_name)        -- A signal can only be used once per deal
);
```

### 4.4 Feedback Tracking
```sql
CREATE TABLE feedback (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  draft_id INTEGER REFERENCES drafts(id),
  feedback_date TEXT NOT NULL,
  rejection_axis TEXT,                -- too_flat / bad_timing / bad_recipient / bad_intention / style_off / other
  feedback_email_sent_at TEXT,
  response_received_at TEXT,
  created_at TEXT DEFAULT (datetime('now'))
);
```

### 4.5 Run Log
```sql
CREATE TABLE run_log (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  run_date TEXT NOT NULL,
  run_type TEXT DEFAULT 'scheduled',   -- scheduled / manual
  deals_received INTEGER DEFAULT 0,
  deals_processed INTEGER DEFAULT 0,
  drafts_created INTEGER DEFAULT 0,
  deals_stopped TEXT,                  -- JSON: [{deal, stop_reason}]
  deals_skipped TEXT,                  -- JSON: [{deal, skip_reason}]
  deals_divergent TEXT,                -- JSON: [{deal, divergences}]
  errors TEXT,                         -- JSON array of error messages
  started_at TEXT,
  completed_at TEXT
);
```

---

## 5. Subagent Architecture

### 5.1 `deal-processor.md` — Per-Deal Processing Subagent

**Input (via payload):**
- Deal data from Deal Pulse (JSON)
- Today's date, run context
- Learning parameters (current cadences, style prefs)
- Already-used signals for this deal (from `signal_usage` table)

**Responsibilities:**
1. **Cadence check** — Is a draft allowed today per tier rules?
2. **Stop conditions** — Check all 6 stop conditions (Gmail for prospect reply <48h, Calendar for meeting ≤3 days, etc.)
3. **Double-check Deal Pulse** — Verify last touch, champion, language, next step against Gmail/Granola
4. **Intention selection** — Pick the best intention from the priority-ordered list
5. **Material gathering** — Collect substantive content (Drive, BigQuery, briefing_hebdo, Slack)
6. **Tier A material validation** — At least 1 of 5 elements must exist
7. **Style matching** — Scan Antoine's recent emails for greeting/closing/tone
8. **Draft generation** — Compose the email and create a Gmail draft
9. **Return result** — JSON with draft details or skip/stop reason

**Output:**
```json
{
  "action": "draft_created" | "stopped" | "skipped" | "divergence_detected",
  "deal_name": "...",
  "stop_reason": "STOP-2: meeting in 2 days" | null,
  "skip_reason": "cadence not met" | null,
  "divergences": [...] | null,
  "draft": {
    "gmail_draft_id": "...",
    "thread_id": "...",
    "to": [...],
    "cc": [...],
    "subject": "...",
    "intention": "preview_tease",
    "language": "FR",
    "body_preview": "..."
  },
  "signals_used": [{"signal_id": "...", "source": "briefing_hebdo"}]
}
```

### 5.2 `learning-loop.md` — J+1 Feedback Analysis Subagent

**Input (via payload):**
- List of drafts from previous day(s) with `status = 'created'` or `status = 'pending'`

**Responsibilities:**
1. **Check draft status** — For each draft, look up in Gmail:
   - Draft still exists as draft → if <48h old, mark `pending`; if ≥48h, mark `rejected`
   - Draft was sent (message found in Sent with matching thread) → mark `sent` or `sent_edited`
   - Draft deleted (not found anywhere) → mark `rejected`
2. **Delta analysis** — For sent_edited drafts, compare draft body vs sent body, log deltas
3. **Compile rejected drafts** — Group by day, generate feedback email if ≥1 rejected
4. **Parse feedback responses** — If Antoine replied to a previous feedback email, extract axes and update `learning_params`

**Output:**
```json
{
  "drafts_checked": 5,
  "sent": 3,
  "sent_edited": 1,
  "rejected": 1,
  "feedback_email_sent": true | false,
  "params_updated": [{"key": "cadence_tier_b_min_days", "old": "10", "new": "14"}]
}
```

---

## 6. Workflow — Detailed Step-by-Step

### Step 0: Pre-flight (Orchestrator)
1. Check if today is a weekday (Mon-Fri)
2. Check Google Calendar for "OOO Antoine" events today → if OOO, **abort silently** (no drafts proposing action during absence — but note: drafts can still be created, they just shouldn't propose actions during OOO windows)
3. Check for French public holidays today → if holiday, **abort silently**
4. Run the **learning loop** first (J+1 analysis of yesterday's drafts)

### Step 1: Fetch Deal Pulse Data
1. Read Deal Pulse output (mechanism TBD — see §3)
2. If empty or unavailable → **email Antoine** + abort
3. Parse deal list, ordered by Deal Pulse priority

### Step 2: Per-Deal Processing Loop
For each deal (in priority order), up to cap of 10 drafts:
1. Dispatch to `deal-processor` subagent
2. Collect result
3. If `draft_created` → increment draft counter, log to `drafts` table, log signals to `signal_usage`
4. If `stopped` or `skipped` → log to `run_log`
5. If `divergence_detected` → collect for batch notification email
6. If draft counter = 10 → stop processing, log remaining deals as skipped

### Step 3: Post-Processing
1. Send divergence notification email to Antoine (if any divergences collected)
2. Write final `run_log` entry
3. Done — **no digest, no recap notification** (Antoine checks drafts on his own)

---

## 7. Trigger Configuration

```
Type: schedule
Cron: 15 8 * * 1-5  (8:15 AM, Mon-Fri)
Timezone: Europe/Paris (Biarritz)
Invocation message: "Daily Follow-Up Forge v2 run"
```

### Manual Override
Antoine can also trigger a run manually by sending a message in chat like:
```
forge [deal_name]
```
This triggers processing for a single named deal, bypassing Deal Pulse's daily list but respecting all stop conditions and cadence rules.

---

## 8. Calendar Intelligence

### Holiday Detection
The agent maintains awareness of:
- **French public holidays** (hardcoded list, updated yearly)
- **Prospect country holidays** — detected via email domain TLD or language:
  - `.pt` → Portugal, `.uk` → UK, `.de` → Germany, etc.
  - FR language + `.com` → assume France
  - EN language + `.com` → assume UK/US (both checked)
- **French school holidays** (zones A/B/C) — for avoiding call proposals during major vacation periods

### Implementation
- Store holiday calendars as JSON files in `/agent/home/follow-up-forge-v2/holidays/`
- Check Google Calendar for "OOO" events for Antoine
- Parse prospect emails for OOO auto-replies
- All date proposals in drafts use **absolute dates only** ("avant le 25 avril", "week of April 28")

---

## 9. Style Matching System

### Style Profile
Built from scanning Antoine's 20 most recent sent emails:
- **Greeting patterns** per language (FR: "Bonjour [prénom]", EN: "Dear [name]")
- **Closing patterns** per language (FR: "Bien à vous,\nAntoine", EN: "Best,\nAntoine")
- **Tu/Vous per recipient** — learned from observed patterns
- **Length distribution** — average lines per tier
- **Voss techniques** — labels and calibrated questions observed
- **Structure** — bullets vs prose preference

### Style Cache
Stored in `/agent/home/follow-up-forge-v2/style_profile.json` and refreshed weekly (or when learning loop detects style drift from sent vs draft deltas).

---

## 10. File Structure

```
/agent/home/follow-up-forge-v2/
├── SYSTEM_DESIGN.md              ← This document
├── config.json                    ← Runtime config (caps, flags)
├── style_profile.json             ← Antoine's email style profile
├── holidays/
│   ├── fr_public_holidays.json
│   ├── school_holidays_fr.json
│   └── intl_holidays.json
└── logs/                          ← Daily run logs (optional, DB is primary)

/agent/subagents/
├── forge-deal-processor.md        ← Per-deal processing subagent
└── forge-learning-loop.md         ← J+1 feedback analysis subagent
```

---

## 11. Error Handling Summary

| Scenario | Action |
|---|---|
| Deal Pulse unavailable/empty | **Email Antoine** + abort run |
| briefing_hebdo absent | Continue without external triggers; Tier D goes silent |
| Gmail thread not found | Treat as first email; add internal comment to draft |
| Granola empty | Continue; cross-check via Gmail only |
| BigQuery inaccessible | Fallback to Drive metrics; no new bench numbers |
| Divergence Deal Pulse vs terrain | **Email Antoine** with divergence details; skip deal |
| Cap 10 drafts reached | Stop processing; log skipped deals |
| Meeting Echo conflict (≤5 days) | Wait; Forge resumes after 5 business days |
| Connection auth failure | **Email Antoine** with error details; create task to retry |

---

## 12. Open Questions — Need Antoine's Input

### 🔴 Critical (Blocks Implementation)

**Q1: How does Forge v2 read Deal Pulse output?**
Deal Pulse is an existing Tasklet agent. How does it expose its daily actionable deal list?
- Google Sheet? Google Doc? JSON file in Drive? Salesforce custom view? Email?
- What's the exact format/location?

**Q2: Is Deal Pulse in this same Tasklet workspace or a separate agent?**
If same workspace, we could potentially use shared filesystem. If separate, we need an external data exchange mechanism.

### 🟡 Important (Affects Quality)

**Q3: Granola — which meetings to query?**
The spec says "last meeting with the client." How do we identify which Granola meetings belong to which deal? By attendee email? By meeting title containing the deal/company name?

**Q4: Meeting Echo — how to detect if it was triggered?**
The spec references "Meeting Echo" as another agent that handles post-meeting milestone recaps. How do we detect whether Meeting Echo has already run for a given meeting? Is there a label, a draft pattern, or a flag somewhere?

**Q5: briefing_hebdo file location in Drive**
What's the exact folder path or naming convention? The spec says `briefing_hebdo_[YYYY-MM-DD].md` — is this in a specific Drive folder?

**Q6: Style — tutoiement/vouvoiement per recipient**
Is there an existing reference, or should Forge v2 learn entirely from email scan? Any recipients who should default to vous?

### 🟢 Nice to Have (Can Default and Iterate)

**Q7: Slack channel naming convention for deals**
Is it always `#[client-name]` or are there variations? (e.g., `#deal-bnp`, `#cube-bnp`)

**Q8: CC patterns — who are Jonathan, Slay, Ugo?**
The spec mentions these as typical CC. Are their full emails: `jonathana@cube3.ai`, `slayh@cube3.ai`, `ugol@cube3.ai`? Should we always CC them or learn from patterns?

**Q9: BigQuery schema**
We'll need to explore `production-20230612.apex_for_statistics.actual_financial_institutions` to understand available columns for anonymized metrics. Permission to query the schema?

---

## 13. Implementation Roadmap

Once open questions are resolved:

| Phase | Scope | Effort |
|---|---|---|
| **Phase 1** | Core pipeline: Trigger → Deal Pulse read → Cadence filter → Stop conditions → Gmail draft creation | ~2 sessions |
| **Phase 2** | Double-check layer: Gmail thread verification, Granola cross-check, Salesforce validation | ~1 session |
| **Phase 3** | Material enrichment: Drive (deal trackers, briefing_hebdo), BigQuery metrics, Slack context | ~1 session |
| **Phase 4** | Style matching: Email scan, tu/vous learning, Voss technique matching | ~1 session |
| **Phase 5** | Learning loop: J+1 analysis, feedback emails, adaptive parameter updates | ~1 session |
| **Phase 6** | Calendar intelligence: Holidays, OOO detection, school vacations, prospect absence | ~1 session |
| **Phase 7** | Testing & tuning: End-to-end dry runs, edge cases, cap handling | ~1 session |

**Estimated total: 7-8 sessions to full production.**

---

*This document is the blueprint. Nothing gets built until Antoine validates the architecture and resolves the open questions.*
