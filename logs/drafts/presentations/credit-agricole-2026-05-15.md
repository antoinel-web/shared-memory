---
client: Crédit Agricole Personal Finance & Mobility (CA PFM / CA IBS)
contact: Sébastien Gonidec — Leader Expert Innovation Paiement & Fraude
date_created: 2026-05-15
status: draft_v4_pending_validation
format: presentation (content-only, design handed off to Claude Design)
slides: 4
---

# Draft v1 → v3 — see prior commits

# Draft v4 — 2026-05-15

## Antoine's feedback on v3
- Slide 2 figures wrong. Expected ~33K April monde. Methodology must include ALL asset types (zelle, paypal, crypto, all bank country sub-types), not just "%bank account%". "33k IBANs" was shorthand for 33k accounts.
- Pilot duration changes from 2 months to 1 month, with explicit milestones: J0 share 20 accounts → J+7 review + scope extension → J+25 final review on extended scope.
- Order: "refais tout" → rewrite full deck.

## Methodology correction (slide 2)
Re-ran BigQuery with corrected scope: all asset_sub_types (no filter on '%bank account%'), first-detection bucket on group_id.
- World monthly: Jan 22.9K / Feb 21.8K / Mar 25.9K / Apr 32.9K — cumul ~104K
- New accounts/week April: ~7,700
- Europe IBAN monthly (asset_sub_type='eu bank account'): Jan 5.3K / Feb 5.8K / Mar 6.3K / Apr 6.3K — cumul ~23.7K
- Europe April: ~6,350 IBANs / 574 institutions

## Changes applied
- Slide 2 fully rewritten with corrected figures and LBP POV template structure.
- Slide 4 rewritten with new timeline J0 / J+7 / J+25 instead of 2-month POC. Reframed as "1-month pilot, results in 4 weeks".
- Slides 1 and 3 unchanged from v3.

## Final structure (4 slides)
1. Cover (unchanged)
2. World view Jan-Apr 2026 (corrected figures) + Europe focus
3. Bank A pilot results — 88% / 100% / ~30 days / €4.7M / ROI 30×+ (unchanged)
4. Proposition + timeline pilote 1 mois (J0/J+7/J+25)

## Pending
- Awaiting Antoine validation or "send".
