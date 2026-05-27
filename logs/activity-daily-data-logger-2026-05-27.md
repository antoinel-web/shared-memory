---
agent: daily-data-logger
run_at: 2026-05-27T23:01:00+02:00
status: partial
sources_read:
  - gmail: 20 emails lus
  - bluedot: via webhook temps réel (hors scope logger nocturne)
outputs_written:
  - drive_files: 16 fichiers écrits
  - classification_emails: 1 email envoyé
  - mapping_log_appends: 0 règles ajoutées
pending_classifications: 1 domaine en attente (iccrea.bcc.it)
errors: Domaine inconnu iccrea.bcc.it — email de classification envoyé à Antoine
duration_estimate: medium
---

## Détail des fichiers écrits

### Société Générale
- `2026-05-27_re-societe-generale-intro-prochaines-etapes.txt` (inbound, socgen.com — Eric Leboeuf demande volumétrie mensuelle SOGEFROO)

### Isabel Group
- `2026-05-27_re-cube-3-isabel_1108.txt` (outbound — Antoine envoie agenda réunion du lendemain)
- `2026-05-27_re-cube-3-isabel_1327.txt` (inbound — Tim Van der Wee demande point data privacy)
- `2026-05-27_re-cube-3-isabel_1602.txt` (outbound — Antoine répond sur OSINT et legal note)

### Treezor
- `2026-05-27_re-cube3-treezor-1ere-revue-danalyse_1109.txt` (outbound — Antoine propose contact équipe IT/Produit)
- `2026-05-27_re-cube3-treezor-1ere-revue-danalyse_1113.txt` (inbound — Cédric: sujet non prioritaire, reprendre au Q4)

### UniCredit
- `2026-05-27_re-unicredit-intro-next-steps_1017.txt` (outbound — Antoine envoie points compliance GDPR avant call DPO vendredi 29/05)
- `2026-05-27_re-unicredit-intro-next-steps_1034.txt` (inbound — Sollazzo: demande d'inclure les autres participants call)

### Caixa Geral de Depositos
- `2026-05-27_re-cube-ai-prevencao-de-fraude-precoce-com-ia.txt` (outbound — Antoine partage accès plateforme Panorama pour 20 comptes)

### BPCE - Banque Populaire Caisse d'Epargne
- `2026-05-27_re-analyse-comptes-mules-bpce.txt` (outbound — Antoine envoie doc complémentarité CUBE AI / FNC-RF, propose date avant 19 juin)

### LBP - La Banque Postale
- `2026-05-27_re-la-banque-postale-bilan-10-comptes-prochaines-etapes.txt` (outbound — Antoine relance sur échanges légal + propose call semaine prochaine)

### Dukascopy
- `2026-05-27_re-cube3-dukascopy-compte-mule.txt` (outbound — Antoine envoie two-pager, propose réunion en juin)

### Intesa Sanpaolo
- `2026-05-27_re-intesa-sanpaolo-intro-next-steps.txt` (outbound — Antoine repousse réunion du 3 juin au 4 juin)

### Bunq
- `2026-05-27_re-first-mule-test-50-accounts.txt` (outbound — Antoine propose date review résultats lundi/mardi prochain)

### Incore Bank
- `2026-05-27_re-cube-incore-results-review.txt` (outbound — Antoine confirme agenda call vendredi: timing détection + complétude)

### Banca Sella
- `2026-05-27_re-cube-ai-banca-sella.txt` (outbound — Antoine relance sur sample 20 comptes envoyé le 15 mai)

## Classifications en attente

- **iccrea.bcc.it** : Email d'intro Paolo Battiston + thread scheduling pour présentation ICCREA Banca. Email de classification envoyé à Antoine. Fichier pending: `/agent/home/pending/2026-05-27_iccrea.bcc.it_presentazione-cube-ai-a-iccrea.txt`
