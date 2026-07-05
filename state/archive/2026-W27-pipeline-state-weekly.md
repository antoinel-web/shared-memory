---
published_by: state-janitor
published_at: 2026-07-05T23:00:00+02:00
period: 2026-06-29 to 2026-07-05
sources_consolidated: 2
schema_version: 1.0
---

## Deals actifs

bunq : Technical Validation / POC · POV → Technical Validation / POC · POV | Montant : €100k | Momentum : HOT→HOT
Deblock : Account Validation Test (AVT) → Technical Validation / POC · POV | Montant : €100k→€50k | Momentum : HOT→MED
Swan : Navigating / MAP → Navigating / MAP | Montant : €30k→€50k | Momentum : HOT→MED
BPCE (Groupe BPCE) : Technical Validation / POC · POV → Technical Validation / POC · POV | Montant : €200k | Momentum : MED→MED
La Banque Postale : Account Validation Test (AVT) → Account Validation Test (AVT) | Montant : €200k | Momentum : MED→MED
CaixaGeral de Depositos : Navigating / MAP → Navigating / MAP | Montant : €150k | Momentum : MED→MED
BNP Paribas : Navigating / MAP → Navigating / MAP | Montant : €200k | Momentum : MED→MED
ssp (Score & Secure Payment) : Technical Validation / POC · POV → Technical Validation / POC · POV | Montant : €18k | Momentum : MED→MED
Xpollens : Account Validation Test (AVT) → Account Validation Test (AVT) | Montant : €50k | Momentum : MED→COLD
BCC Banca Iccrea : Account Validation Test (AVT) → Account Validation Test (AVT) | Montant : €25k | Momentum : MED→COLD
BIL (Banque Internationale à Luxembourg) : [nouveau] → Account Validation Test (AVT) | Montant : €200k | Momentum : COLD (1er snapshot)

## Changements notables

- 2026-06-30 : 24 deals actifs — 3 HOT (Deblock, Swan, bunq), 7 MED, 14 COLD
- 2026-07-03 : 25 deals actifs — 1 HOT (bunq), 7 MED, 17 COLD — nouvelle deal BIL apparue
- 2026-07-03 : Deblock avancé en Technical Validation / POC · POV (depuis AVT) — montant ajusté de €100k à €50k
- 2026-07-03 : Swan montant augmenté de €30k à €50k — momentum passé de HOT à MED
- 2026-07-03 : BNP Paribas en progression : AI Health +0.8, MEDDPICC +5.1 (tendance positive)
- 2026-07-03 : Xpollens et BCC Banca Iccrea passées de MED à COLD

## Alertes émises

- 2026-06-30 : Swan — EB non rencontré (cible : CRO), PP=0, dépendance PSD3, ROI > 10x requis
- 2026-06-30 : Deblock — pricing €100k refusé par Aaron, objection cyclicité, gap attribution crypto
- 2026-06-30 : bunq — aucune activité détectée en 7j, relance nécessaire
- 2026-06-30 : ssp — EB absent 2 semaines, deadline budget Jul 31
- 2026-07-03 : BPCE — réunion Jul 6 à préparer (résultats analyse IBAN)
- 2026-07-03 : BNP Paribas — risque RFP commoditisation, verrouiller pass/fail avant RFP

## Gaps

- 2026-06-29 (lundi) : pas de fichier pipeline-state
- 2026-07-01 (mercredi) : pas de fichier pipeline-state
- 2026-07-02 (jeudi) : pas de fichier pipeline-state
- 2026-07-04 (samedi) : pas de fichier pipeline-state
- 2026-07-05 (dimanche) : pas de fichier pipeline-state
