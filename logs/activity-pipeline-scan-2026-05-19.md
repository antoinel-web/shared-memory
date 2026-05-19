---
agent: ae-salesforce-operator
run_at: 2026-05-19T09:01:00+02:00
trigger: pipeline-scan
status: success
sources_read:
  - slack: 12 messages
  - gmail: 8 threads
  - granola: 2 meetings
  - calendar: 22 events
  - salesforce: 29 opps queried
outputs_written:
  - salesforce: 9 opps updated, 3 activity tasks created
  - slack: nudge posted to #sales-inbox
errors: null
duration_estimate: heavy
---

## Key Actions This Run

1. **BPCE**: Meeting rescheduled May 20 → Jun 19 (Sarah Guigny). Updated NextStep, Red Flags.
2. **BNP Paribas France**: Group CyberMonitoring meeting completed May 18 (Escoffre, Tedeschi, Fangouse). Weekly sync May 19 today. Internal prep with Einaras on cyber positioning.
3. **Credit Agricole**: May 18 results review with Sebastien Gonidec completed (1h, Bluedot recap). Re-engagement after weeks.
4. **BNP Fortis**: May 20 follow-up confirmed. Fraud typology doc still pending (13 days).
5. **Intesa Sanpaolo**: May 20 intro with Gianluca Chiusano confirmed.
6. **Banca Profilo**: May 20 1st analysis readout with Antonio Gravetti confirmed.
7. **BGL Luxembourg**: May 22 POC kickoff confirmed. Marlot & Xavier unavailable, Benoit Audeyer attending.
8. **Swan**: May 22 2nd restitution confirmed. Martin raised omnibus/Wise IBAN challenge.
9. **Julius Baer**: REJECTED by Femi. Private wealth = poor ICP. Close-loss recommended.

## Deal Pulse Signals
- Updated `data/deal-signals.json` with 29 deal signals
- HOT deals: BNP Paribas France (Accelerating, engagement_7d=5)
- COLD deals: Julius Baer (Stalled/rejected), Lydia (Dark), Treezor (Stalled), Dukascopy (Stalled)
