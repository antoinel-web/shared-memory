---
agent: daily-data-logger
run_at: 2026-04-30T23:05:00+02:00
status: partial
sources_read:
  - gmail: 6 emails lus
  - granola: FAILED (timeout x2)
outputs_written:
  - drive_files: 8 fichiers écrits
  - classification_emails: 0 emails envoyés
  - mapping_log_appends: 1 règle ajoutée (bancaprofilo.it → Banca Profilo)
pending_classifications: 0
errors: Granola inaccessible (timeout x2) — calls non collectés. bnpparibasfortis.com détecté dans events calendrier (pas de mail client à classer aujourd'hui). Granola call 2026-04-29 "Stratégie prospection Stéphanie Rios/Bunq" : réponse Antoine reçue (Classer sous Bunq) mais pas de fichier pending disponible.
duration_estimate: medium
---

## Détail des emails traités

### Bunq (bunq.com)
- `2026-04-30_re-cube-bunq-friday-3pm-inbound-1102.txt` — inbound, Kaue Alves (conflicting meeting)
- `2026-04-30_re-cube-bunq-friday-3pm-outbound-1252.txt` — outbound Antoine (let's speak at 3:00)
- `2026-04-30_re-cube-bunq-friday-3pm-inbound-1524.txt` — inbound, Kaue + Stephanie Rios introduite, NDA envoyé

### Swan (swan.io)
- `2026-04-30_re-swan-intro-next-steps.txt` — inbound, Martin Miguel (question comptes omnibus / Wise)

### Caixa Geral de Depositos (cgd.pt)
- `2026-04-30_re-cube-ai-prevencao-fraude-precoce-ia-2045.txt` — outbound Antoine (sample 500 comptes PT + testing guide + revised proposal)
- `2026-04-30_re-cube-ai-prevencao-fraude-precoce-ia-2050.txt` — outbound Antoine (addendum: nouvelle propsal)

### Banca Profilo (bancaprofilo.it) — classement rétroactif 2026-04-29
- `2026-04-29_r-cube-ai-banca-profilo-mules-testing.txt` — inbound, Antonio Gravetti (annulation + report réunion)
- `2026-04-29_re-cube-ai-banca-profilo-mules-testing.txt` — outbound Antoine (report réunion + company deck)

## Emails exclus
- Calendly newsletter (productupdates@send.calendly.com)
- Gemini meeting notes x3 (gemini-notes@google.com)
- ACFE marketing email (MemberServices@acfe.com)
- Calendar acceptances BNP Paribas Fortis x2 (Accepted: CUBE AI <>...)
- Calendar acceptance CGD (Accepted: CUBE.AI <>...)
- Granola team notification x2
- Fireflies.ai daily digest
- Tasklet agent notifications
- Draft emails (Antoine → bnpparibasfortis.com, Antoine → bunq.com Stephanie)

## Règle apprise
`bancaprofilo.it` → `Banca Profilo` (confirmé par Antoine le 2026-04-30)

## Note
`bnpparibasfortis.com` détecté dans acceptances calendrier. Domaine absent du domain-mapping. Aucun email client non-calendrier à classer aujourd'hui, mais à surveiller pour la prochaine run.
