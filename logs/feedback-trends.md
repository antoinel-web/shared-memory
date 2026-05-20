---
period: 2026-04-29 to 2026-04-30
total_deltas: 13
breakdown:
  Utilisé: 6
  Modifié: 3
  Remplacé: 1
  Ignoré: 3
  Pending: 0
top_categories:
  - Contexte client ajouté (6 occurrences)
  - Contenu supprimé (4 occurrences)
  - Ton ajusté (3 occurrences)
  - Structure modifiée (3 occurrences)
  - Autre (3 occurrences)
  - CTA changé (2 occurrences)
trends:
  - "Forge v2: 'Contexte client ajouté' is the dominant delta category across
    this period — Antoine consistently enriches follow-ups with real-time
    client context (meeting postponements, live inbound signals, updated data
    points) not available at draft generation time. Agents should be designed
    to signal when recent client activity may change draft assumptions."
  - "Forge v2: Ignoré rate for Apr 29 batch is 30% (3/10 drafts never sent:
    KBC, Swissquote, Deblock). Pattern suggests Forge v2 generates drafts for
    deals Antoine has already deprioritized or placed on hold. Consider adding
    a deal-activity recency check before generating new drafts."
  - "Meeting Echo (Apr 30): All three drafts resulted in sent emails, but all
    required significant addition — primarily 'What we covered' recap sections
    and competitive references not captured in the draft's meeting summary.
    Meeting Echo systematically under-captures the qualitative context Antoine
    adds when writing the actual email (metrics reframing, competitive anchoring)."
  - "Qonto (Apr 29 Forge v2): Email dispatched with unfilled IBAN placeholder
    — a notable execution gap. Agent flagged it as a pre-send action item but
    the email was sent anyway. Consider surfacing incomplete placeholders as
    a blocking warning rather than advisory note."
---

---
period: 2026-05-04 to 2026-05-04
total_deltas: 2
breakdown:
  Utilisé: 0
  Modifié: 2
  Remplacé: 0
  Ignoré: 0
top_categories:
  - Contexte client ajouté (2 occurrences)
  - Structure modifiée (2 occurrences)
trends:
  - "Meeting Echo (May 4): Both UniCredit and Swan drafts required Modifié
    classification — Antoine added real-time context not available at draft
    generation time (DPO scheduling options requested via inbound email,
    intermediate email exchange context around omnibus accounts). Meeting Echo
    captures meeting outcomes well but systematically misses inbound activity
    occurring between draft generation and final send."
  - "Meeting Echo (May 4): Both emails were dispatched 1–2 days after draft
    generation (UniCredit: +1 day, Swan: +2 days), with intervening events
    materially changing scope. Consider implementing a stale-draft flag when
    an inbound email from the same thread arrives after draft creation."
---

---
period: 2026-05-05 to 2026-05-05
total_deltas: 10
breakdown:
  Utilisé: 2
  Modifié: 3
  Remplacé: 1
  Ignoré: 4
top_categories:
  - CTA changé (4 occurrences)
  - Autre (4 occurrences)
  - Ton ajusté (3 occurrences)
  - Contexte client ajouté (3 occurrences)
  - Structure modifiée (2 occurrences)
  - Contenu supprimé (1 occurrence)
trends:
  - "Forge v2 (May 5): High Ignoré rate — 4 out of 10 drafts never sent (Swan,
    Nickel, OuiTrust, UniCredit). Across the batch, Antoine appears to have
    deprioritized several accounts, suggesting Forge v2 generates follow-up
    drafts without adequate weighting of deal-activity recency or Antoine's
    current sequencing priorities."
  - "Forge v2 (May 5): CTA changé is the single most frequent delta category
    (4 occurrences). Antoine consistently softens or replaces direct scheduling
    and escalation CTAs with open-ended closings. Agents should default to softer
    CTA formulations unless explicit meeting-urgency context is present."
  - "Forge v2 (May 5): Dukascopy Modifié — the draft proposed a bypass strategy
    (routing around the rejection contact). Antoine instead chose direct
    confrontation with a live ROI argument (~€700k annual exposure). Indicates
    the agent lacks visibility into Antoine's read on counterpart credibility
    and internal decision-making power."
  - "Forge v2 (May 5): OuiTrust email written but not dispatched (Gmail draft
    flag confirmed) — though subsequently sent 10 days later (May 15). Suggests
    a delayed-send pattern where Antoine prepares re-engagement emails well in
    advance of execution. Consider tracking Gmail draft state as a signal."
---
