---
published_by: state-janitor
published_at: 2026-05-10T23:06:00+02:00
period: 2026-05-04 to 2026-05-10
sources_consolidated: 3
schema_version: 1.0
---

## Deals actifs

Banca Profilo - Apex - NB 27 : Qualifying → Qualifying | Montant : €100k
Poste Italiane - Apex - NB27 : Qualifying → Qualifying | Montant : €200k
KBC - Apex - 2026 : Qualifying → Qualifying | Montant : €200k
Incore - Apex - 2026 : Account Validation Test (AVT) → Account Validation Test (AVT) | Montant : €30k
Belfius - Apex - 2026 : Account Validation Test (AVT) → Account Validation Test (AVT) | Montant : €200k
Intesa Sanpaolo - Apex - NB 2026 : Qualifying → Qualifying | Montant : €200k
Groupe CCF - Apex - 2026 : Pre Opp → Pre Opp | Montant : €100k
BGL (BNP Luxembourg) - Apex - NB27 : Account Validation Test (AVT) → Account Validation Test (AVT) | Montant : €100k
Credit Agricole - Apex - 2026 : Qualifying → Qualifying | Montant : €200k
Deblock - Apex - NB 26 : Account Validation Test (AVT) → Account Validation Test (AVT) | Montant : €100k
OuiTrust - Apex - 2026 : Pre Opp → Pre Opp | Montant : €50k
Julius Baer - Apex - NB 26 : Account Validation Test (AVT) → Account Validation Test (AVT) | Montant : €100k
Swissquote - Apex - 2026 : Account Validation Test (AVT) → Account Validation Test (AVT) | Montant : €100k
Nickel - Apex - 2026 : Account Validation Test (AVT) → Account Validation Test (AVT) | Montant : €100k
La Banque Postale - Apex - 2026 : Account Validation Test (AVT) → Account Validation Test (AVT) | Montant : €200k
Qonto - Apex - 2026 : Pre-Opportunity → Pre-Opportunity | Montant : €50k
Bunq - Apex - 2026 : Account Validation Test (AVT) → Account Validation Test (AVT) | Montant : €140k
Betclic - Apex - 2026 : Account Validation Test (AVT) → Account Validation Test (AVT) | Montant : €30k
Treezor - Apex - 2026 : Account Validation Test (AVT) → Account Validation Test (AVT) | Montant : €30k
Banca Sella - Apex - NB 2027 : Qualifying → Qualifying | Montant : €100k
Dukascopy - Apex - 2026 : Account Validation Test (AVT) → Account Validation Test (AVT) | Montant : €100k
Lydia - Apex - 2026 : Technical Validation / POC · POV → Technical Validation / POC · POV | Montant : €57k
BPCE - Apex - 2026 : Account Validation Test (AVT) → Account Validation Test (AVT) | Montant : €200k
BNL (BNP italia) - Apex - NB 2026 : Qualifying → Qualifying | Montant : €100k
Score & Secure Payment - Apex - 2026 : Account Validation Test (AVT) → Account Validation Test (AVT) | Montant : €50k
Swan - Apex - 2026 : Account Validation Test (AVT) → Account Validation Test (AVT) | Montant : €30k
UniCredit - Apex - NB 2026 : Account Validation Test (AVT) → Account Validation Test (AVT) | Montant : €200k
BNP Paribas FRANCE - Apex - 2026 : Navigating / MAP → Navigating / MAP | Montant : €200k
CGD - Apex - 2026 : Negotiating → Negotiating | Montant : €150k
BNP Paribas Fortis - Apex - NB 26 : (Nouveau deal, apparu le 2026-05-05) Qualifying | Montant : €100k

## Changements notables

- 2026-05-05 : Nouveau deal apparu dans le pipeline — BNP Paribas Fortis - Apex - NB 26 | Qualifying | €100k | Premier meeting tenu le 2026-04-30
- 2026-05-04 : UniCredit — 1er review résultats mules (8 participants) ; 50% des 20 IBANs suspects/frauduleux ; health 6.0→6.2 ; MEDDPICC 51→51.4
- 2026-05-04 : Swan — 1ère restitution comptes mules ; 56% de détection sur 20k EU IBANs ; 9 pré-détections avant signal interne ; health 4.0→4.2 ; MEDDPICC 30→31.5
- 2026-05-05 : Julius Baer — Meeting tenu avec Olufemi Olowolafe & Siddharth Mirashi ; prochain meeting reschedulé au 2026-05-11
- 2026-05-05 : BNP Paribas France — Mule accounts review avec Jérôme Palin & Sabine Thévelin
- 2026-05-06 : BPCE — Demo + data test avec Sarah Guigny, Sacha Bergeon, Florent Grail, Olivier Irisson (COO) ; prochain meeting 2026-05-20 (analyse IBANs externes compromis)
- 2026-05-06 : CGD — Board meeting tenu le 2026-05-05 ; post-board debrief avec Manuel, Carlos, Neide ; MEDDPICC 38.4→39.5 ; paper process 0%→11% (procurement team will handle if board approves)
- 2026-05-06 : Bunq — Meeting avec Stephanie Rios (Fraud Prevention Product Owner, procurement) + Kaue ; health 3.5→3.9 ; MEDDPICC 35→35.2
- 2026-05-06 : Banca Profilo — Mules Testing meeting tenu avec Antonio Gravetti
- 2026-05-07 : Incore — Catch-up tenu avec Thomas Glaus (nouveau Head of Compliance)
- 2026-05-07 : Credit Agricole — Meeting avec Gonidec (CA-IBS) + De Beaumont (LCL) ; prochain rendez-vous reschedulé au 2026-05-18
- 2026-05-08 : BNP Paribas Fortis — Next meeting 2026-05-13 confirmé (livraison 10 comptes sample + doc GDPR + API docs)

## Alertes émises

- Semaine entière : OuiTrust — contact Jaime dark 29+ jours, deal probablement mort (MEDDPICC 0/80), recommandation : un dernier re-engage puis archiver
- Semaine entière : Groupe CCF — zéro signal dans toutes les sources de données, pas de contact identifié, deal potentiellement créé par erreur
- Semaine entière : Betclic — deal pausé jusqu'en juillet (World Cup), pas d'action possible avant reconnexion en juillet
- Semaine entière : Lydia — MISP (concurrent gratuit) bloque la progression ; William Brulin veut benchmark 3-4 mois ; champion Alexandre Meyran dark 5+ semaines
- 2026-05-08 : AE Salesforce Operator — aucun fichier d'activité détecté depuis le 2026-04-28 (>7 jours d'inactivité)

## Gaps

- Fichiers pipeline-state manquants pour : 2026-05-06 (mercredi), 2026-05-07 (jeudi), 2026-05-09 (samedi), 2026-05-10 (dimanche)
