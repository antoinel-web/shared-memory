# Deal Pulse — Integration Guide

## Data Source

Deal signals are published by the **pipeline-scan** agent (runs Tue & Fri 07:00 CET) as a single JSON file:

```
https://github.com/antoinel-web/shared-memory/blob/main/data/deal-signals.json
```

Raw URL:
```
https://raw.githubusercontent.com/antoinel-web/shared-memory/main/data/deal-signals.json
```

## Update Frequency

- **Overwritten** every pipeline-scan run (Tue & Fri morning)
- Always reflects the **latest snapshot** of all open opportunities
- `generated_at` field = ISO-8601 timestamp of last update

## Schema (v1.0)

```json
{
  "generated_at": "2026-05-11T07:00:00+02:00",
  "agent": "pipeline-scan",
  "schema_version": "1.0",
  "deals": {
    "<salesforce_opportunity_id>": {
      "account_name": "string",
      "opportunity_name": "string",
      "stage": "string (SF StageName)",
      "ai_health_score": "number (0.0–10.0, Dealpad pWin proxy)",
      "meddpicc_scores": {
        "metrics": "number (0.0–9.0)",
        "economic_buyer": "number (0.0–9.0)",
        "decision_criteria": "number (0.0–9.0)",
        "decision_process": "number (0.0–9.0)",
        "paper_process": "number (0.0–9.0)",
        "pain": "number (0.0–9.0)",
        "champion": "number (0.0–9.0)",
        "competition": "number (0.0–9.0)"
      },
      "signals": {
        "deal_velocity": "enum — see below",
        "champion_engagement": "enum — see below",
        "economic_buyer_status": "enum — see below",
        "metrics_validation": "enum — see below",
        "procurement_status": "enum — see below",
        "client_timeline": "string | null",
        "engagement_7d": "integer",
        "engagement_30d": "integer",
        "score_wow_delta": "number (signed decimal)"
      },
      "next_step": "string (255 chars max)",
      "next_step_date": "string (YYYY-MM-DD) | null",
      "red_flags": "string (255 chars max) | null"
    }
  }
}
```

## Signal Enums

### `deal_velocity`
| Value | Meaning |
|---|---|
| `Accelerating` | Strong forward momentum: timeline stated, procurement launched, multiple meetings/week, new stakeholders |
| `Steady` | Regular cadence (≥1 interaction/week), confirmed next step, no red flags |
| `Slowing` | Declining interactions, vague/pushed next steps, >7d since last substantive exchange |
| `Stalled` | >14d no meaningful contact, no confirmed next step, client ghosting |
| `Dark` | >21d radio silence, no response to last 2+ outreaches |

### `champion_engagement`
| Value | Meaning |
|---|---|
| `Active_Internal_Selling` | Champion actively advocating: presenting to board, making intros to EB/stakeholders |
| `Active` | Champion engaged and responsive, no evidence of internal advocacy yet |
| `Passive` | Champion responds when prompted, not driving momentum |
| `Identified_Only` | Name known, minimal engagement |
| `None` | No champion identified |

### `economic_buyer_status`
| Value | Meaning |
|---|---|
| `Met` | Direct meeting/call/email exchange with EB (verified in calendar/notes) |
| `Scheduled` | Meeting with EB confirmed in calendar (future) |
| `Identified` | Name/title known, no direct contact yet |
| `Unknown` | No EB identified |

### `metrics_validation`
| Value | Meaning |
|---|---|
| `Full_Analysis` | On-Bank + Off-Bank analysis completed AND results shared to client |
| `Partial` | One phase done, or results generated but not shared/discussed |
| `Pitch_Only` | Only generic metrics/case studies shared |
| `None` | No quantitative discussion |

### `procurement_status`
| Value | Meaning |
|---|---|
| `Active` | Procurement/legal engaged: RFP/RFI, MSA/SOW under review |
| `Discussed` | Client mentioned procurement process, no formal engagement |
| `Not_Started` | No procurement discussion |

### `client_timeline`
Verbatim client-stated timeline with source and date.
- Example: `"1-2 months if interest confirmed" (Bunq, Slack, 2026-05-05)`
- `null` if no client-stated timeline exists — never fabricated.

## Recommended HOT / MED / COLD Classification

Deal Pulse owns the classification logic. Suggested approach:

### 🔥 HOT
All of:
- `deal_velocity` = `Accelerating` OR (`Steady` AND `engagement_7d` ≥ 3)
- `champion_engagement` ∈ {`Active_Internal_Selling`, `Active`}
- `ai_health_score` ≥ 3.0
- `score_wow_delta` ≥ 0

### 🟡 MEDIUM
Any of:
- `deal_velocity` = `Steady`
- `champion_engagement` ≥ `Passive` AND `engagement_30d` ≥ 4
- `ai_health_score` ≥ 2.0 AND `score_wow_delta` ≥ -0.5

### 🧊 COLD
Any of:
- `deal_velocity` ∈ {`Stalled`, `Dark`}
- `engagement_30d` ≤ 1
- `champion_engagement` = `None` AND `economic_buyer_status` = `Unknown`

## Additional Salesforce fields (read directly from SF if needed)

The pipeline-scan agent also writes these directly to Salesforce on every run:
- `AI_Health_Score__c` — Dealpad pWin (0–10)
- `AI_Health_Score_Reason__c` — plain-English justification
- `NextStep` — concrete next action
- `Next_Step_Date__c` — date
- `Red_Flags__c` — key blockers
- `MEDDPICC_*_Score__c` (8 fields) — per-dimension scores (capped at 9)

These are mirrored in the JSON for convenience, but SF is the source of truth for MEDDPICC data.

## Versioning

If the schema changes, `schema_version` will be bumped. Deal Pulse should check `schema_version` and handle gracefully.
