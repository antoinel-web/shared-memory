---
agent: daily-data-logger
run_at: 2026-06-29T23:00:00+02:00
status: success
sources_read:
  - gmail: 2 emails lus (Swan, BNL BNP Paribas IT)
  - bluedot: via webhook temps réel (hors scope logger nocturne)
outputs_written:
  - drive_files: 17 fichiers écrits
  - classification_emails: 1 email envoyé (gmail.com / Deblock)
  - mapping_log_appends: 2 règles ajoutées (bil.com, natixis.com)
pending_classifications: 1 domaine en attente (gmail.com)
errors: null
duration_estimate: heavy
---

## Détail des fichiers écrits

### Emails du jour (2026-06-29)
- `Swan/emails/2026-06-29_re-swan-intro-next-steps.txt` — Luc Drye (swan.io), réponses Q&A POC
- `BNL BNP Paribas IT/emails/2026-06-29_re-presentation-solution-cube-ai.txt` — Jerome Palin (bnpparibas.com), liste accès portail

### Backlog classifié ce soir (15 fichiers)
**BPCE - Banque Populaire Caisse d'Epargne:**
- `emails/2026-06-11_tfactory.fr_re-cube-ai-bpce-strategie.txt`
- `emails/2026-06-15_tfactory.fr_re-cube-ai-bpce-strategie.txt`
- `emails/2026-06-24_natixis.com_fwd-analyse-comptes-mules-bpce.txt`
- `calls/2026-06-12_t-factory-cube-ai.txt` (Bluedot: T FACTORY <> CUBE AI)

**BNP Paribas - France:**
- `calls/2026-06-08_cube-ai-bnp-group-17.txt` (Bluedot: CUBE AI <> BNP Group, Anne-Lise Escoffre)

**SSP - Score & Secure Payments:**
- `calls/2026-06-08_cube-ai-demo-plateforme.txt` (Bluedot: CUBE AI - Demo plateforme)

**BNL BNP Paribas IT:**
- `emails/2026-06-18_bnpparibas.com_en-amont-de-notre-meeting-du-23-juin.txt`
- `emails/2026-06-18_bnpparibas.com_reponse-automatique-en-amont-meeting-23-juin.txt`
- `emails/2026-06-22_bnpparibas.com_presentation-de-la-solution-cube-ai_0819.txt`
- `emails/2026-06-22_bnpparibas.com_presentation-de-la-solution-cube-ai_1027.txt`
- `emails/2026-06-22_bnpparibas.com_presentation-de-la-solution-cube-ai_1029.txt`

**BIL** (nouveau dossier créé):
- `emails/2026-06-26_bil.com_bil-intro-next-steps.txt`
- `calls/2026-06-25_cube-ai-x-bil-comptes-mules.txt` (Bluedot: CUBE AI x BIL - Comptes Mules)

**Isabel Group:**
- `calls/2026-06-12_cube-ai-isabel-belgium-anti-fraud.txt` (Bluedot: CUBE AI <> ISABEL: Belgium anti fraud initiative)

**Xpollens:**
- `calls/2026-06-18_cube-ai-x-xpollens-comptes-mules.txt` (Bluedot: CUBE AI x Xpollens - Comptes Mules)

### Ignorés (réponse Antoine "3")
- `2026-06-16_iwforguk_iwf-and-cube-ai-partnership.txt`
- `2026-06-23_hotmail.com_opportunity-speech-italian-banking-association.txt`

### En attente de classification
- `2026-06-29_gmail.com_deblock.txt` — Jonathan Anastasia (gmail.com), sujet "Deblock", "Can't make this"
  - Classification envoyée à Antoine (antoinel@cube3.ai)

## Règles apprises
- `bil.com` → BIL (Banque Internationale à Luxembourg)
- `natixis.com` → BPCE - Banque Populaire Caisse d'Epargne (filiale groupe BPCE)
