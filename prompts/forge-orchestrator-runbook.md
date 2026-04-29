# Follow-Up Forge v2 — Orchestrator Runbook

This file is read by the main agent at the start of every scheduled trigger run.
Follow these steps exactly, in order. Do not skip steps.

**Config file:** `/agent/home/follow-up-forge-v2/config.json`
**Holidays:** `/agent/home/follow-up-forge-v2/holidays/fr_public_holidays.json`
**Deal processor subagent:** `/agent/subagents/forge-deal-processor.md`
**Learning loop subagent:** `/agent/subagents/forge-learning-loop.md`
**Owner:** antoinel@cube3.ai

---

## Pre-Run Setup

Read the config file. Extract:
- `run.draft_cap_per_run` (default 10)
- `run.dry_run` (if true, create NO drafts — just log what would have been created)
- `deal_pulse.github_repo_owner`, `deal_pulse.github_repo`, `deal_pulse.github_path`
- `notifications.alert_email`

Initialize a run_log entry in the DB:
```sql
INSERT INTO run_log (run_date, run_type, started_at) 
VALUES ('[today]', 'scheduled', datetime('now'))
```
Save the run_id for use throughout.

---

## Step 0: Pre-flight Checks

### 0a. Day of week check
Today must be Monday-Friday. If weekend → **abort silently** (no log entry, trigger shouldn't fire but check anyway).

### 0b. French public holiday check
Read `/agent/home/follow-up-forge-v2/holidays/fr_public_holidays.json`.
If today's date is in the list for the current year → **abort silently**.
Log to run_log: `{"abort_reason": "French public holiday"}` then complete the run_log entry.

### 0c. Antoine OOO check
Search Google Calendar for today's events:
- `conn_tpfxd4qqwq1f23rsxzqt__google_calendar_search_events`
- start: today 00:00, end: today 23:59
- Look for events with title containing "OOO", "Out of Office", "Congé", "Absent", or "Vacances"
- If found → **abort silently**. Log abort reason.

### 0d. Run learning loop first
Run the learning loop subagent (`/agent/subagents/forge-learning-loop.md`) with payload:
```json
{"today": "[YYYY-MM-DD]", "owner_email": "antoinel@cube3.ai"}
```
Collect the result. If learning loop errors, log them but continue with the main run.

---

## Step 1: Fetch Deal Pulse Data

### 1a. Read from GitHub (primary mechanism)
Fetch the Deal Pulse pipeline state file from GitHub:
- Use `conn_dpcxmh9hg0wppated7kq__github_get_file_content`
- owner: `antoinel-web`
- repo: `shared-memory`
- repoPath: `state/pipeline-state-latest.md`

If the file is not found (repo may be private) → **send alert email** to Antoine and **abort**:

Email subject: `[Forge v2] Deal Pulse file not found — run aborted`
Body: "Could not read state/pipeline-state-latest.md from antoinel-web/shared-memory. The repository may be private — ensure the Tasklet GitHub app is installed and has access. Follow-Up Forge v2 did not run today."

### 1b. Staleness check
The file has a YAML front-matter block at the top with a `published_at` field. Parse it:
```
---
published_at: 2026-04-24T08:00:00+02:00
...
---
```
Parse the `published_at` timestamp. If it is older than `deal_pulse.staleness_hours` (24h) from now → **send alert email** to Antoine and **abort**:

Email subject: `[Forge v2] Deal Pulse report stale — run aborted`
Body: "The Deal Pulse pipeline state is more than 24 hours old (published_at: [timestamp]). Follow-Up Forge v2 did not run today. Please ensure Deal Pulse publishes its state before 8:15 AM."

### 1c. Parse deal list
Parse the markdown content (after the YAML header) to extract the list of deals. The file should contain structured deal data (JSON blocks, YAML sections, or a Markdown table).

Expected fields per deal:
- deal_name, salesforce_id, tier, stage_meddpicc, next_steps, last_interactions, champion, stakeholders_recent, risks, language, priority_rank, contact_emails, company_domain

If the document format doesn't match expectations → **email Antoine** with a parsing error and abort.

Update run_log: `deals_received = N`

---

## Step 2: Load Learning Params & Style Profile

### 2a. Load learning params from DB
```sql
SELECT param_key, param_value FROM learning_params
```
Build a key-value map.

### 2b. Load style profile
Read `/agent/home/follow-up-forge-v2/style_profile.json` if it exists.
Check the `last_updated` field — if older than 7 days or file doesn't exist, pass `null` (the deal processor will rebuild it).

### 2c. Load used signals per deal
For each deal, pre-load signals already used in the last 30 days:
```sql
SELECT signal_id, signal_source, deal_name FROM signal_usage 
WHERE used_date > date('now', '-30 days')
```
Group by deal_name for easy lookup.

---

## Step 3: Per-Deal Processing Loop

Process deals in `priority_rank` order. Stop when `drafts_created >= draft_cap_per_run`.

For each deal:

### 3a. Dispatch to deal processor subagent
Run `/agent/subagents/forge-deal-processor.md` with payload:
```json
{
  "deal": { ...deal object... },
  "today": "[YYYY-MM-DD]",
  "learning_params": { ...params map... },
  "already_used_signals": [ ...signals for this deal... ],
  "style_profile": { ...or null... },
  "run_id": [run_id]
}
```

### 3b. Handle result

**If `action = "draft_created"`:**
- Increment `drafts_created` counter
- Insert into drafts table:
```sql
INSERT INTO drafts (run_date, deal_name, salesforce_id, tier, intention, draft_gmail_id, thread_id, to_addresses, cc_addresses, subject, body_preview, language, status)
VALUES ('[today]', '[deal_name]', '[sf_id]', '[tier]', '[intention]', '[gmail_draft_id]', '[thread_id]', '[to_json]', '[cc_json]', '[subject]', '[body_preview]', '[language]', 'created')
```
- Insert used signals:
```sql
INSERT OR IGNORE INTO signal_usage (signal_source, signal_id, deal_name, used_date)
VALUES ('[source]', '[signal_id]', '[deal_name]', '[today]')
```

**If `action = "stopped"`:**
- Append to `deals_stopped` list: `{"deal": deal_name, "stop_reason": stop_reason}`
- Do NOT increment draft counter

**If `action = "skipped"`:**
- Append to `deals_skipped` list: `{"deal": deal_name, "skip_reason": skip_reason}`
- Do NOT increment draft counter

**If `action = "divergence_detected"`:**
- Append to `deals_divergent` list: `{"deal": deal_name, "divergences": divergences}`
- Do NOT create a draft for this deal

### 3c. Cap check
If `drafts_created >= draft_cap_per_run` → stop the loop, add all remaining deals to `deals_skipped` with reason "daily cap reached".

---

## Step 4: Post-Processing

### 4a. Divergence notification
If `deals_divergent` is non-empty → send email to Antoine:

Subject: `[Forge v2] Divergences détectées — [N] deal(s) nécessitent vérification`

Body:
```
Bonjour Antoine,

Follow-Up Forge a détecté des divergences entre les données Deal Pulse et la réalité terrain pour [N] deal(s). Aucun brouillon n'a été créé pour ces deals.

[For each divergent deal:]
**[deal_name]**
[For each divergence:]
- [field]: Deal Pulse dit "[dp_value]", terrain montre "[actual_value]"

Action requise : Veuillez vérifier et corriger les données dans Deal Pulse.

Forge v2
```

### 4b. Update run_log
```sql
UPDATE run_log SET
  deals_processed = [N],
  drafts_created = [N],
  deals_stopped = '[json]',
  deals_skipped = '[json]',
  deals_divergent = '[json]',
  errors = '[json]',
  completed_at = datetime('now')
WHERE id = [run_id]
```

### 4c. Done
No digest, no recap. Antoine checks drafts on his own.

---

## Step 6 — Activity Report (ALWAYS LAST)

After all other steps are complete (including learning loop), write an activity report to:
`logs/activity-follow-up-forge-v2-YYYY-MM-DD.md` in repo `antoinel-web/shared-memory`, branch `main`.

**If the file already exists for today** (check is implicit — just append with `---` separator using `github_push_to_branch`).

**Format (YAML front-matter block):**
```
---
agent: follow-up-forge-v2
run_at: [ISO-8601 timestamp]
status: success | partial | failure
sources_read:
  - deal_pulse_github: [nb deals OR FAILED]
  - salesforce: [nb opportunities OR FAILED]
  - gmail: [nb threads searched OR FAILED]
  - granola: [nb meetings OR FAILED]
  - slack: [nb channels OR FAILED]
  - calendar: [nb events OR FAILED]
  - bigquery: FAILED (apex_for_statistics not found until fixed)
outputs_written:
  - gmail_drafts: [n] drafts created
  - run_log: logged to DB run_log id=[n]
  - learning_params: [n updates OR no change]
errors: null OR short description
duration_estimate: light | medium | heavy
---
```

**Rules:**
- Never include client names, email addresses, or deal details
- Only execution metadata
- If GitHub push fails → log the error in run_log.errors and continue (non-blocking)
- Use `conn_dpcxmh9hg0wppated7kq` → `github_push_to_branch`, branch `main`

---

## Error Handling

| Scenario | Action |
|---|---|
| Deal Pulse not found in GitHub | Email Antoine + abort |
| Deal Pulse stale (>24h) | Email Antoine + abort |
| Deal Pulse parse error | Email Antoine + abort |
| OOO detected | Abort silently |
| Holiday | Abort silently |
| Deal processor subagent error | Log error, skip that deal, continue |
| Learning loop error | Log error, continue with main run |
| Gmail connection error | Log error, skip affected deals |
| Draft cap reached | Stop loop, log remaining as skipped |
| Connection auth 401 | Email Antoine with error + create task to retry |

---

## Manual Run ("forge [deal_name]")

When Antoine sends a message like `forge BNP` or `forge [deal name]`:
1. Extract the deal name
2. Skip Steps 0b (holiday) and check 0c (OOO) — run anyway since Antoine triggered it manually
3. Search Deal Pulse for that specific deal only
4. Run the deal processor for that single deal
5. Report back in chat: what was created or why it was stopped/skipped

---

## Connection IDs Reference

- Gmail: `conn_k0t1758ymakkc329yyvp`
- Granola: `conn_a6kdbz33vpd2ed5np1g9`
- Salesforce: `conn_gezqj3ebq09rrpzsmey8`
- Slack: `conn_3c0rm7dd5dh7864y39jd`
- Google Calendar: `conn_tpfxd4qqwq1f23rsxzqt`
- Google Drive: `conn_v1mbff73hy3vh3f013he`
- BigQuery: `conn_3n3cpcawq8wtjcevtnw6`
- GitHub: `conn_dpcxmh9hg0wppated7kq`
