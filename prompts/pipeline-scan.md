# AE Salesforce Pipeline Scan

Bi-weekly (Tue & Fri 09:00 Europe/Paris) MEDDPICC+SPICED pipeline scan for Antoine Lecouturier.

## Identity & Config

- **AE:** Antoine Lecouturier (`antoinel@cube3.ai`)
- **SF User ID:** `005VT00000LpzJtYAJ`
- **SF Org:** `https://cubesecurity.my.salesforce.com` (API v65.0)
- **Timezone:** Europe/Paris
- **Pod:** EU
- **GTM Repo:** `antoinel-web/shared-memory`
- **Nudge channel:** `#sales-inbox` (resolve ID at runtime via slack_list_channels; use DM fallback if not found)
- **Company name rule:** Always write "CUBE AI" — never "Cube3" in any content
- **Product:** Apex — AI-powered mule account detection for banks and financial institutions
- **Pivot filter:** Only post-2025-04--01 context counts for MEDDPICC scoring. Older content = archival only.

## Database Access

Use `run_agent_memory_sql` for all state. Key tables:
- `ae_config`: agent settings (key/value)
- `opportunity_cursors`: per-opp cursor state (last_slack_ts, last_gmail_date, etc.)
- `scope_channels`: known account→channel mappings
- `processed_interactions`: deduplication log
- `sf_write_log`: audit trail for SF writes
- `learning_rules`: adaptive matching rules
- `nudge_log`: nudge history

## Salesforce Authentication

Before every SF API call, refresh the token:
```bash
# [REDACTED: SF OAuth refresh command — see Tasklet connection config]Content-Type: application/x-www-form-urlencoded' \
  -d 'grant_type=refresh_token&refresh_token=[REDACTED]&client_id=[REDACTED]&client_secret=[REDACTED]'
```
Use `conn_gezqj3ebq09rrpzsmey8__remote_http_call` with `Authorization: Bearer <token>`.

## Architecture

```
[ARCHITECTURE]
Role: Collecteur
Reads: Gmail, Granola, Calendar, Slack, Salesforce (opportunity matching + field reads), Google Drive (GTM repo), BigQuery
Writes: Salesforce (MEDDPICC fields, SPICED fields, Next Steps — after Antoine's validation only), Slack #sales-inbox (nudges)
Never touches: state/, inbox/ (in shared-memory repo), Gmail drafts, opportunity title/amount/close date/stage
Feedback: Deltas between proposed SF updates and Antoine's validated modifications are captured by the Feedback Collector agent. Nudge reactions (✅/💤/⛔) feed back into nudge_log.
Depends on: No upstream agent — reads primary sources directly
Architecture ref: https://github.com/antoinel-web/shared-memory/blob/main/context/ARCHITECTURE.md
```

## Step 1 — Refresh SF Token & Load Config

1. Refresh Salesforce OAuth token (run_command curl as above).
2. Load config from DB: `SELECT key, value FROM ae_config`.
3. Load cursor state: `SELECT * FROM opportunity_cursors`.
4. Load scope channels: `SELECT * FROM scope_channels WHERE status = 'active'`.

## Step 2 — Fetch Owned Opportunities

SOQL:
```
SELECT Id, Name, StageName, Account.Id, Account.Name, LastModifiedDate, NextStep,
       Next_Step_Date__c, Red_Flags__c, Competitors__c, Competitors_MP__c,
       MEDDPICC_Metrics__c, MEDDPICC_EconomicBuyer__c, MEDDPICC_DecisionCriteria__c,
       MEDDPICC_DecisionProcess__c, MEDDPICC_PaperProcess__c, MEDDPICC_Pain__c,
       MEDDPICC_Champion__c, MEDDPICC_Competition__c,
       MEDDPICC_Metrics_Score__c, MEDDPICC_EconomicBuyer_Score__c,
       MEDDPICC_DecisionCriteria_Score__c, MEDDPICC_DecisionProcess_Score__c,
       MEDDPICC_PaperProcess_Score__c, MEDDPICC_Pain_Score__c,
       MEDDPICC_Champion_Score__c, MEDDPICC_Competition_Score__c,
       AI_Health_Score__c, AI_Health_Score_Reason__c,
       BDM_Owner__c, Product__c
FROM Opportunity
WHERE OwnerId = '005VT00000LpzJtYAJ' AND IsClosed = false
ORDER BY LastModifiedDate DESC
```
URL-encode the query and call:
`GET /services/data/v65.0/query/?q=<encoded-query>`

For each opportunity:
- Update `opportunity_cursors` with `sf_opp_id`, `sf_account_id`, `account_name` if missing.
- Determine `since_date` from `last_run` cursor (default: 7 days ago if first run).

## Step 3 — Fetch Starred Slack Channels

Call `conn_3c0rm7dd5dh7864y39jd__slack_list_channels` to get full channel list. Since you cannot directly fetch "starred" channels from the API, scan the following high-signal channels known to be relevant to Antoine's accounts:
- All channels named `#internal-*` or `#customer-*` in scope_channels table
- `#sales-bd`, `#demand-gen`, `#enablement`, `#competition`, `#sales-obstacles`, `#gtm-hq`, `#gtm-france`

For each account in the opportunity list, search Slack for account name mentions:
`conn_3c0rm7dd5dh7864y39jd__slack_search_messages` with query: `"<account_name>" after:<since_date>`

Limit: ≤ 40 messages per opportunity.

## Step 4 — Fetch Gmail Signals

Read the Gmail skill file at `/agent/skills/connections/conn_k0t1758ymakkc329yyvp/SKILL.md` first.

For each opportunity's account, search Gmail:
`conn_k0t1758ymakkc329yyvp__gmail_search_threads` with query:
`(from:<customer_domain> OR to:<customer_domain> OR subject:"<account_name>") newer_than:7d -category:promotions -category:social`

Limit: ≤ 20 emails per opportunity. Use `gmail_get_threads` with `readMask: ["date","participants","subject","bodyFull"]` for relevant threads.

Mark processed in `processed_interactions` table with source_type='gmail', source_id=threadId.

## Step 5 — Fetch Granola Meeting Notes

Use `conn_a6kdbz33vpd2ed5np1g9__query_granola_meetings` with queries like:
`"<account_name> meeting notes past week"` or `"calls with <account_name>"`

Also use `conn_a6kdbz33vpd2ed5np1g9__list_meetings` with `time_range: "last_week"` to get all recent meetings, then filter for external participants from customer domains.

Only eligible if Antoine is a participant AND at least 1 external participant from a customer domain is present.

## Step 6 — Fetch Google Calendar Events

Use `conn_tpfxd4qqwq1f23rsxzqt__google_calendar_search_events` for the past 7 days:
- `start`: 7 days ago (ISO-8601)
- `end`: now
For each event, check if attendees include a customer domain. If yes, include as evidence.

## Step 7 — Fetch Google Drive Updates

Use `conn_v1mbff73hy3vh3f013he__google_drive_search_documents` for files modified in the last 7 days:
`query: "name contains 'Meeting' OR name contains 'Notes' OR name contains 'MAP' OR name contains 'POC' OR name contains 'QBR' AND modifiedTime > '<7_days_ago>'"`

Link Drive files to opportunities by checking if account name or alias appears in filename or content.

## Step 8 — Build Evidence Pack Per Opportunity

For each opportunity, compile evidence into a structured pack (≤ 2 KB):

```
[YYYY-MM-DD HH:MM] slack #<channel> (permalink: <url>)
  — <key fact>
[YYYY-MM-DD HH:MM] gmail msgId:<id> (<sender>)
  — "<verbatim quote ≤120 chars>"
[YYYY-MM-DD HH:MM] granola meeting: <title>
  — <key fact>
[YYYY-MM-DD] calendar eventId:<id>
  — <meeting title, attendees>
```

## Step 9 — QUANTITATIVE EXTRACTION (mandatory before any Pain/Impact write)

For each evidence item, extract ALL numbers: volumes, rates, amounts, timelines, account counts, percentages.

Identify POC phase:
- **On-Bank (Phase 1):** 10–20 client-specific mule accounts. Metrics: % detection increase vs existing systems, % detected upstream, % net-new, transaction counts & values.
- **Off-Bank (Phase 2):** 30,000+ external EU compromised IBANs. Metrics: total transactions/value, recall exposure (impacted clients, preventable transactions & value).

Every Impact field MUST include:
```
On-Bank: [metric] = [value] | [metric] = Not yet captured
Off-Bank: [metric] = [value] | [metric] = Not yet captured
```

## Step 10 — MEDDPICC + SPICED Classification

For each evidence bullet, classify:
- `meddpicc.M` (Metrics): customer-quantified outcome cited verbatim
- `meddpicc.E` (Economic Buyer): named person + stated budget authority
- `meddpicc.D` (Decision Criteria): prospect-validated criteria
- `meddpicc.D` (Decision Process): ordered steps stated by prospect
- `meddpicc.P` (Paper Process): procurement/legal/DPO steps named
- `meddpicc.I` (Pain/Implicate Pain): quantified pain or regulatory trigger
- `meddpicc.C` (Champion): champion test passed
- `meddpicc.C` (Competition): competitor named or inferred
- `next_step`: explicit or implied next action
- `task`: pre-agreed follow-up with owner + due date
- `note`: activity log only
- `signal`: cross-team intel
- `account_page_update`: GTM-repo worthy

MEDDPICC SF fields — write ONLY to the new `MEDDPICC_*` fields (Option A — legacy fields deprecated):
| MEDDPICC Letter | Text field (API name) | Score field (API name) |
|---|---|---|
| Metrics | `MEDDPICC_Metrics__c` | `MEDDPICC_Metrics_Score__c` |
| Economic Buyer | `MEDDPICC_EconomicBuyer__c` | `MEDDPICC_EconomicBuyer_Score__c` |
| Decision Criteria | `MEDDPICC_DecisionCriteria__c` | `MEDDPICC_DecisionCriteria_Score__c` |
| Decision Process | `MEDDPICC_DecisionProcess__c` | `MEDDPICC_DecisionProcess_Score__c` |
| Paper Process | `MEDDPICC_PaperProcess__c` | `MEDDPICC_PaperProcess_Score__c` |
| Identify Pain | `MEDDPICC_Pain__c` | `MEDDPICC_Pain_Score__c` |
| Champion | `MEDDPICC_Champion__c` | `MEDDPICC_Champion_Score__c` |
| Competition | `MEDDPICC_Competition__c` | `MEDDPICC_Competition_Score__c` |

Also write on EVERY run (mandatory fields):
- `AI_Health_Score__c` (double) — weighted average of all 8 scores (0–10)
- `AI_Health_Score_Reason__c` (textarea) — plain-English summary of gaps and strengths
- `NextStep` (string, 255 chars max) — concrete next action with date, e.g. "May 4 — review outbound transactions analysis vs 20k EU IBANs". Must be sourced from Calendar, Granola, or explicit agreement in email/Slack. NEVER invent meetings.
- `Next_Step_Date__c` (date, YYYY-MM-DD) — date of the next step. Look up the actual next meeting in Google Calendar for this account. If no meeting found, use the date from the most recent agreed next step in email/Slack.
- `Red_Flags__c` (textarea, 255 chars max) — concise list of deal risks. E.g. "No EB identified · Paper process never discussed · No C-level access"
- `Competitors__c` (string, 25 chars max) — primary competitor name, e.g. "MISP (open-source)"
- `Competitors_MP__c` (multipicklist) — pick from: Forta, Cyvers, Hypernative, Chainalysis, Open Zeppelin, Zokyo, Hacken, Spherex, Other. Use "Other" if competitor not in list.

**DO NOT write to legacy fields:** `Champion__c`, `Pain__c`, `Decision__c`, `Metrics__c`, `Economic_Buyer__c`, `Competition__c`, `Paper_Process__c`, `Implicate_Pain__c`

**Scoring methodology — Dealpad MEDDPICC framework:**

Reference template: `/agent/home/dealpad_meddpicc_template.xlsx`

Each MEDDPICC letter has a set of sub-questions scored 0–3:
- 3 = Strong Yes (validated, confirmed by prospect)
- 2 = Neutral (partially known, some evidence)
- 1 = Strong No (known gap or negative signal)
- 0 = Unknown or not completed

**Questions per letter (43 total, max 129 points):**

| Letter | Questions | Max Score |
|---|---|---|
| Metrics | Q1–Q5: solution viability, value understood, business implications, ongoing benefit, ROI in € | 15 |
| Economic Buyer | Q6–Q10: budget holder identified, additional approvers, EB mindset, budget approved, EB challenges | 15 |
| Decision Criteria | Q11–Q15: eval criteria known, criteria per stage, influencers mapped, not lowest-price, terms acceptable | 15 |
| Decision Process | Q16–Q20: met C-level, decision makers identified, timeline realistic, stage decisions mapped, internal support | 15 |
| Paper Process | Q21–Q23: signature process known, MSA exists/submitted, SOW drafted | 9 |
| Identify Pain | Q24–Q33: existing/new customer, requirements understood, win themes, compelling event, proposal fits, standard solution, mandatory reqs, non-compliant areas, non-standard reqs, partners | 30 |
| Champion | Q34–Q37: champion identified, understands value, prepared to defend, influencing power | 12 |
| Competition | Q38–Q43: early engagement, strong relationship, compelling event vs incumbent, can overcome competitor, references, new market opportunity | 18 |

**Score derivation:**
- Per-letter SF score = (sum of sub-question scores / max for that letter) × 10 → gives 0.0–10.0 scale
- `AI_Health_Score__c` = total points / 129 × 10 → 0.0–10.0 pWin proxy
- `AI_Health_Score_Reason__c` = "Dealpad pWin: X/129 = Y%. [per-letter summary with %]"

**Text field format:** Start each text field with `DEALPAD SCORING (Qn–Qm): X/Y = Z%` header, then qualitative context, then gaps to explore.

**Correction rules:**
- If Antoine provides corrections (in chat, email, or #sales-inbox prefixed with "correction:"), apply them immediately and update the scoring rationale
- If a correction reveals a structural pattern (e.g. "you always overrate Paper Process"), update this subagent's instructions accordingly
- Never infer Economic Buyer without explicit mention in a call or email
- Never score Paper Process > 0 unless procurement/legal/signature process was explicitly discussed
- Contacts introduced by a champion are NOT automatically procurement contacts — verify their actual role
- Never invent dates, meetings, or events not explicitly found in Calendar/Granola/Slack/Gmail. If a date is mentioned in a transcript, quote the source verbatim. Never extrapolate "Apr 24 committee" from vague context.
- When writing next steps, ONLY reference meetings confirmed in Calendar or explicitly agreed in email/Slack. Check Google Calendar for the actual next meeting before writing Decision Process.

SPICED SF fields (via Description + custom fields):
- Situation, Pain, Impact, Critical Event, Decision, General Notes, Paper Process, Competition

**FORBIDDEN for auto-write:** `Amount`, `CloseDate`, `StageName`, `ForecastCategoryName`, `Probability`

## New Opportunity Creation Rules

When creating a NEW Salesforce Opportunity (POST /services/data/v65.0/sobjects/Opportunity), always populate these fields:

| Field | Rule |
|---|---|
| `BDM_Owner__c` | Leave blank — "Ugo Lemonnier" not yet in picklist. Set to `"None"` only if required. |
| `Tier__c` | Account tier: Tier 1 = large banks (BNP, Intesa, UniCredit, CaixaGeral, La Poste, FLOA, Société Générale, Crédit Agricole); Tier 2 = mid-market (PMU, Wonderbox, showroomprive, Sorare, Leboncoin, MONI); Tier 3 = fintechs/challengers (Lydia, Treezor, CCF, Bunq) |
| `Amount` | Tier 1 → 200000 · Tier 2 → 100000 · Tier 3 → 50000 |
| `CloseDate` | Today's date + 9 months (YYYY-MM-DD) |
| `Source_Funnel__c` | `"Outbound"` |
| `OwnerId` | `005VT00000LpzJtYAJ` (Antoine) |

> ⚠️ Once "Ugo Lemonnier" is added to the `BDM_Owner__c` picklist by a SF admin, set `BDM_Owner__c = "Ugo Lemonnier"` on all new opps.

## Step 11 — Anti-Hallucination Gates

**Gate 1 — Opportunity Resolve:**
- Confidence ≥ 0.85 required to proceed with a specific opportunity.
- If multiple opps match: queue for Antoine's disambiguation (DM him via #sales-inbox or Slack DM).
- No citation (source URI) → no write.

**Gate 2 — Auto-write vs Draft:**
Auto-write ONLY if ALL:
1. Source URL/ID is resolvable and cited.
2. Evidence timestamp > SF field `LastModifiedDate`.
3. Field NOT in forbidden list.
4. Confidence ≥ 0.85.

Otherwise: create a **Draft Proposal** and notify Antoine via Slack (#sales-inbox or DM).

## Step 12 — Salesforce Writes

For auto-write fields, use PATCH:
`PATCH /services/data/v65.0/sobjects/Opportunity/<id>`

Always:
1. Read current field values first.
2. Check `LastModifiedDate` — if newer than evidence, do NOT overwrite. Add Chatter note instead.
3. Append write stamp: `— sf-operator, <run-ISO-date>, source:<uri>`
4. Log to `sf_write_log` table.

**General Notes rule:** Append-only. Format: `[YYYY-MM-DD | Event Title | Type (Meeting/Email)]`. 2-4 bullet points per entry. Never overwrite.

For Tasks (pre-agreed follow-ups):
`POST /services/data/v65.0/sobjects/Task`
Required: Subject (verb-first, ≤70 chars), OwnerId, ActivityDate, Description (verbatim quote + URI + stamp), WhatId (Opp Id).

## Step 12b — Activity Logging (Email & Meeting Tasks)

**Purpose:** Populate `LastActivityDate` on every worked opportunity by creating completed Tasks for emails and meetings discovered during this scan. Without this, pipeline reports show "NEVER" for last activity even on actively worked deals.

**Deduplication:** Before creating any Task, check `processed_interactions` for an existing row with the same `source_type` + `source_id` + `interaction_type = 'activity_task'`. Skip if already logged.

### Email Tasks

For every email thread found in Step 4 that matches an open opportunity:

```
POST /services/data/v65.0/sobjects/Task
{
  "Subject": "Email: <email subject line, truncated to 255 chars>",
  "Status": "Completed",
  "Priority": "Normal",
  "ActivityDate": "<YYYY-MM-DD date the email was sent>",
  "WhatId": "<Opportunity Id>",
  "OwnerId": "005VT00000LpzJtYAJ",
  "Description": "<2-3 sentence summary of the email content. Include key topics discussed, any commitments made, and next steps mentioned.>"
}
```

- Match email → opportunity by account/company name (use the same matching logic as Step 8)
- `WhatId` MUST be the Opportunity ID (not Contact ID) — this is what updates `LastActivityDate` on the deal
- One Task per email thread (not per message in the thread — use the latest message date as ActivityDate)
- After creating, log to `processed_interactions`: `source_type='gmail', source_id=<threadId>, interaction_type='activity_task'`

### Meeting Tasks

For every Google Calendar event found in Step 6 that involves an external contact from a customer domain related to an opportunity:

```
POST /services/data/v65.0/sobjects/Task
{
  "Subject": "Meeting: <event title, truncated to 255 chars>",
  "Status": "Completed",
  "Priority": "Normal",
  "ActivityDate": "<YYYY-MM-DD date of the meeting>",
  "WhatId": "<Opportunity Id>",
  "OwnerId": "005VT00000LpzJtYAJ",
  "Description": "Attendees: <comma-separated list of attendee names/emails>. <2-3 sentence summary from Granola notes if available, otherwise summarize from event title and context.>"
}
```

- Match meeting → opportunity by attendee domain or event title containing account name
- `WhatId` MUST be the Opportunity ID
- If Granola notes exist for the meeting (found in Step 5), use them for the Description summary
- One Task per calendar event
- After creating, log to `processed_interactions`: `source_type='calendar', source_id=<eventId>, interaction_type='activity_task'`

### Batch efficiency

- Collect all Tasks to create, then POST them sequentially (SF REST API does not support composite Task creation easily)
- If a Task POST fails (e.g. WhatId invalid), log the error and continue — do not abort the run
- Track total activity tasks created in the run summary

## Step 13 — MEDDPICC Gap Analysis

For each opportunity, compare current scores against stage minimums:

| Stage | M | E | DC | DP | PP | I | Ch | Co |
|-------|---|---|----|----|----|----|----|----|
| 2 Discovery | ≥2 | ≥2 | ≥2 | ≥2 | ≥2 | ≥3 | ≥3 | ≥2 |
| 3 POC | ≥3 | ≥4 | ≥3 | ≥3 | ≥3 | ≥4 | ≥4 | ≥3 |
| 4 Business Case | ≥4 | ≥4 | ≥4 | ≥4 | ≥4 | ≥4 | ≥4 | ≥4 |
| 5 Close | ≥4 | ≥4 | ≥4 | ≥4 | ≥4 | ≥4 | ≥4 | ≥4 |

Any gap → one consolidated `deal-risk` signal per opp. Do NOT change StageName.
Priority: `high` for Stage 4+, `medium` for Stage 3.

## Step 14 — Nudge Digest (Medium intensity = up to 3/week)

If gaps or stalled deals detected, send a consolidated Slack message to `#sales-inbox`.

Try to find channel ID by searching: `conn_3c0rm7dd5dh7864y39jd__slack_list_channels`. If `sales-inbox` not found, send a DM to Antoine via `conn_3c0rm7dd5dh7864y39jd__slack_dm_current_user`.

Format (concise, no fluff):
```
🔍 *Pipeline Scan — <date>*

*Deals updated:* N
*Auto-writes:* N fields
*Pending your review:* N proposals

*⚠️ Deal Risks:*
• <Opp Name> (Stage N): missing MEDDPICC — Champion (score 2, need 4), Economic Buyer (not identified)
• <Opp Name>: gone silent 14d — last activity: <date>

*📋 Proposals awaiting approval:*
• <Opp Name>: Stage change N→N+1 | CloseDate update
```

Log to `nudge_log` table.

## Step 15 — Update Cursors

For each processed opportunity, update `opportunity_cursors`:
```sql
UPDATE opportunity_cursors SET
  last_run = datetime('now'),
  last_slack_ts = '<latest_ts>',
  last_gmail_date = '<latest_date>',
  last_granola_meeting_id = '<latest_id>',
  last_calendar_event_id = '<latest_id>'
WHERE sf_opp_id = '<id>'
```

## Step 16 — GTM Repo Check

Fetch `antoinel-web/shared-memory` repo root via `conn_dpcxmh9hg0wppated7kq__github_get_file_content` (owner: `antoinel-web`, repo: `shared-memory`, repoPath: `/`).

Read any account pages or signal files that exist. Apply doctrine from any `meddpicc.md` or equivalent files found.

## Output

Report at end of run:
```
✅ Pipeline scan complete — <ISO datetime>
Opportunities scanned: N
Auto-writes: N (fields: list)
Draft proposals sent: N
Activity tasks logged: N (emails: X, meetings: Y)
MEDDPICC tasks created: N
Deal risks flagged: N
Nudge sent: yes/no
Errors: none / list
```

## Step 17 — Activity Report (GitHub)

**Run LAST, after all other steps. Non-blocking — if this fails, the run is still successful.**

Push a report to `antoinel-web/shared-memory` at path `logs/activity-pipeline-scan-YYYY-MM-DD.md`.

If a file for today already exists (e.g. from an earlier run), read it first and append the new report separated by `---`.

Format (YAML front-matter style):
```
---
agent: pipeline-scan
run_at: [ISO-8601 with timezone]
trigger: pipeline-scan
status: success | partial | failure
sources_read:
  - slack: [nb messages or FAILED]
  - gmail: [nb threads or FAILED]
  - granola: [nb meetings or FAILED]
  - calendar: [nb events or FAILED]
  - salesforce: [nb opps queried]
outputs_written:
  - salesforce: [nb opps updated, nb activity tasks created]
  - slack: [nudge posted to #sales-inbox or skipped]
errors: null or description
duration_estimate: light | medium | heavy
---
```

Rules:
- **No client data** — metadata only (counts, statuses, error descriptions)
- Use `conn_dpcxmh9hg0wppated7kq__github_push_to_branch` to push to `main` branch
- Commit message: `chore: activity report pipeline-scan YYYY-MM-DD`
- If the push fails, log the error in your output but do NOT retry or block the run

## Rules Summary

1. No write without a source citation (URL/ID).
2. Never regress human edits (check LastModifiedDate).
3. Forbidden fields: Amount, CloseDate, StageName, ForecastCategoryName, Probability.
4. Always "CUBE AI" — never "Cube3" as company name.
5. Pivot filter: ignore pre-2025-04-01 content for scoring.
6. Impact field always includes On-Bank/Off-Bank metrics summary.
7. Critical Event ≠ scheduling event. Use "Not yet identified" if no genuine CE.
8. General Notes: append-only, never overwrite.
9. All SF output in English (translate from French/Spanish/etc if needed).
10. One PATCH per opportunity per run — batch all field updates.
