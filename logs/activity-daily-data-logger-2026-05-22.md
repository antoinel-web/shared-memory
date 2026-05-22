---
agent: daily-data-logger
run_at: 2026-05-22T23:01:00+02:00
status: success
sources_read:
  - gmail: 10 emails lus
  - bluedot: via webhook temps réel (hors scope logger nocturne)
outputs_written:
  - drive_files: 10 fichiers écrits
  - classification_emails: 0 emails envoyés
  - mapping_log_appends: 0 règles ajoutées
pending_classifications: 0 domaines en attente
errors: null
duration_estimate: medium
---

## Détail des fichiers écrits

### BGL BNP Luxembourg (bgl.lu)
- `Clients/BGL BNP Luxembourg/emails/2026-05-22_re-cube-ai-bgl-bnp-paribas-comptes-mules.txt` — outbound, email post-call Antoine → BGL team (envoi dataset 20 000 IBANs)

### Swan (swan.io)
- `Clients/Swan/emails/2026-05-22_re-swan-intro-next-steps_1007.txt` — inbound, Luc Drye → demande de report call au 1er juin
- `Clients/Swan/emails/2026-05-22_re-swan-intro-next-steps_1059.txt` — outbound, Antoine → réponse avec instructions KPIs pour analyse

### BNP Paribas Fortis (bnpparibasfortis.com)
- `Clients/BNP Paribas Fortis/emails/2026-05-22_re-bnp-paribas-fortis-intro-next-steps_0955.txt` — inbound, Roel Zoons → demande de remplacer Roel par Siert Cornille
- `Clients/BNP Paribas Fortis/emails/2026-05-22_re-bnp-paribas-fortis-intro-next-steps_1150.txt` — outbound, Antoine → récapitulatif post-réunion (résultats 10 comptes, envoi 20 nouveaux IBANs)
- `Clients/BNP Paribas Fortis/emails/2026-05-22_fwd-bnp-paribas-fortis-intro-next-steps_1327.txt` — outbound, Antoine → forward email à Siert Cornille (nouveau contact)

### BPER Banca (bper.it)
- `Clients/BPER Banca/emails/2026-05-22_re-bper-intro-next-steps_0901.txt` — outbound, Antoine → demande de report réunion du 3 juin au 5 juin
- `Clients/BPER Banca/emails/2026-05-22_r-bper-intro-next-steps_0904.txt` — inbound, Davide Belloni → accord pour le 5 juin 9-10h

### BNL BNP Paribas IT (bnpparibas.com)
- `Clients/BNL BNP Paribas IT/emails/2026-05-22_re-bnl-intro-next-steps.txt` — outbound, Antoine → update avant POC (résultats institution comparable BNP group, 97% détection, 40j avance)

### Banca Profilo (bancaprofilo.it)
- `Clients/Banca Profilo/emails/2026-05-22_re-cube-ai-banca-profilo-mules-testing.txt` — outbound, Antoine → récapitulatif post-call Manuel (résultats 10 comptes : 70% avance, 100% suspects, 5 comptes actifs)

## Nouveaux contacts détectés
- `siert.cornille@bnpparibasfortis.com` — Siert Cornille, nouveau référent BNP Paribas Fortis (remplace Roel Zoons) — domaine déjà mappé

## Appels du jour (Bluedot webhook)
- `(16) CUBE AI <> BGL BNP Paribas - Comptes Mules` (19 min) — géré via Bluedot
- `(16) CUBE AI <> Banca Profilo - conti mule` (21 min) — géré via Bluedot
- `Antoine / Jonas Eneba sync` (6 min) — géré via Bluedot
- `Antoine / Slay` (13 min, BNP Paribas Group) — géré via Bluedot
