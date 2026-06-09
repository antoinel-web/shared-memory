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

---
period: 2026-05-07 to 2026-05-07
total_deltas: 4
breakdown:
  Utilisé: 2
  Modifié: 2
  Remplacé: 0
  Ignoré: 0
top_categories:
  - Contexte client ajouté (4 occurrences)
  - CTA changé (2 occurrences)
  - Structure modifiée (2 occurrences)
  - Contenu supprimé (1 occurrence)
  - Ton ajusté (1 occurrence)
trends:
  - "Meeting Echo (May 7): 50% Utilisé rate (Credem, BPER) — intro/next-steps
    follow-up emails where core content (benchmark metrics, demo recap, agreed
    deliverables, next meeting) was preserved faithfully. Targeted enrichments
    only: Italian closing added, recipient list widened, dataset scope updated.
    Meeting Echo performs reliably for standard intro-to-POC templates when
    the meeting structure is clear and no inbound activity intervened."
  - "Meeting Echo (May 7): Both Modifié entries (Bunq, BPCE) involved structural
    rewrites. BPCE's factual results summary was reorganised into three strategic
    business-value axes (internal mule reduction, client protection, cash-out
    reduction) absent from the draft. Bunq's 4-step next-step list was condensed
    to 2 (critical commercial steps only). Meeting Echo captures meeting data
    accurately but misses Antoine's post-reflection strategic framing — a
    consistent gap for results-review and competitive-context sessions."
  - "Meeting Echo (May 7): 'Contexte client ajouté' appears in all 4 entries —
    confirming a systematic blind spot. Real-time enrichments (updated detection
    figures, expanded recipient scope, live data points from post-meeting
    follow-up) consistently added by Antoine at send time. Consider a lightweight
    signal to Antoine for post-meeting context capture before draft is finalized."
---

---
period: 2026-05-15 to 2026-05-15
total_deltas: 9
breakdown:
  Utilisé: 0
  Modifié: 0
  Remplacé: 2
  Ignoré: 7
top_categories:
  - Autre (7 occurrences)
  - CTA changé (2 occurrences)
  - Contenu supprimé (2 occurrences)
  - Structure modifiée (2 occurrences)
  - Contexte client ajouté (1 occurrence)
trends:
  - "Forge v2 (May 15): Ignoré rate of 78% (7/9 drafts never sent) — the highest
    in any observed batch. All 7 unmatched drafts were Pending at the May 15 run
    and confirmed unsent after 72h. Antoine was OOO until May 17; drafts generated
    at or just before the OOO window went entirely unprocessed. Consider suppressing
    draft generation when an OOO signal is detected on the account owner."
  - "Forge v2 (May 15): Both Remplacé entries (Bunq, Deblock) share a common
    pattern — inbound client activity (Bunq NDA counter-signature, Deblock live IBAN
    signal) made the outbound draft obsolete before it could be sent. Antoine responded
    reactively to the inbound rather than adapting the draft. Forge v2 lacks an
    inbound-activity gate; generating outbound drafts without checking for recent
    inbound from the same thread consistently produces obsolete content."
  - "Forge v2 (May 15): The CTA changé and Contenu supprimé categories in the two
    Remplacé entries confirm a recurrent pattern across all observed batches — Antoine
    removes or replaces structured multi-step CTAs with simpler reactive responses
    whenever an inbound signal is present. Forge v2 should weight down direct CTAs
    when recent inbound activity is detected in the same thread."
---

---
period: 2026-05-20 to 2026-05-21
total_deltas: 4
breakdown:
  Utilisé: 1
  Modifié: 2
  Remplacé: 0
  Ignoré: 1
top_categories:
  - Contexte client ajouté (3 occurrences)
  - CTA changé (2 occurrences)
  - Structure modifiée (2 occurrences)
  - Contenu supprimé (1 occurrence)
  - Autre (1 occurrence)
trends:
  - "Meeting Echo (May 20-21): Contexte client ajouté is dominant (3/4 entries)
    — Antoine consistently enriched follow-ups with client-specific data
    unavailable at draft generation time (coaching-brief transformation for BNP
    Paribas BCEF, IBAN evidence detail walkthrough for BNP Paribas Fortis,
    activity timeline enrichment for Intesa Sanpaolo). Meeting Echo performs
    structurally well but lacks real-time post-meeting context at generation time."
  - "Meeting Echo (May 21): Intesa Sanpaolo produced the fastest dispatch of any
    observed entry in the sliding window — sent 6 minutes post-draft, resulting
    in Utilisé classification (~80% content similarity). Rapid dispatch strongly
    correlates with high content fidelity: Antoine adopts drafts more faithfully
    when acting within minutes of meeting end, before reflective post-processing
    introduces additional context or restructuring."
  - "Meeting Echo (May 20): CA/LCL Ignoré — the only undelivered draft in this
    window. Post-call recap generated at 13:10 CEST; Gmail draft confirmed
    never dispatched through May 22. The CA/LCL account appears to require a
    longer deliberation window than Meeting Echo's same-day draft cadence
    accommodates; the June 18 follow-up meeting proceeded without the post-session
    recap having reached the counterpart."
---

---
period: 2026-05-22 to 2026-05-22
total_deltas: 10
breakdown:
  Utilisé: 0
  Modifié: 0
  Remplacé: 1
  Ignoré: 9
top_categories:
  - Autre (10 occurrences)
  - CTA changé (1 occurrence)
trends:
  - "Forge v2 (May 22): Ignoré rate of 90% (9/10 drafts never sent) — tied
    for the highest observed rate alongside the May 15 OOO batch. Unlike May 15,
    no OOO signal was present on May 22; Antoine was actively sending (one email
    dispatched at 09:12 CEST). The 9 unsent drafts span Italian and French
    mid-market banking prospects, suggesting Antoine deliberately deprioritised
    a broad cohort in favour of a single high-intensity engagement, rather than
    a workflow disruption."
  - "Forge v2 (May 22): The sole Remplacé entry illustrates a recurring
    constraint-override pattern — the agent had internally marked a specific
    outreach angle as unsuitable after prior rejections, yet Antoine deployed
    exactly that angle and received engagement. This is the second observed
    instance of Antoine overriding an agent-derived constraint rule. The agent's
    self-limiting logic does not track Antoine's evolving read on counterpart
    receptivity; constraint rules should decay or be flagged for review after
    30+ days of no new signal."
  - "Forge v2 (May 22): The 9 bulk-Ignoré drafts all carry category 'Autre'
    with no actionable delta — they were simply not deployed. This pattern
    recurs across May 15 and May 22. A per-account silence-tolerance model
    (e.g., suppress draft generation if the last outbound was < N days ago and
    no inbound was received) would reduce waste in batches where Antoine is
    clearly holding fire on multiple accounts simultaneously."
---

---
period: 2026-05-26 to 2026-05-26
total_deltas: 2
breakdown:
  Utilisé: 0
  Modifié: 0
  Remplacé: 0
  Ignoré: 2
top_categories:
  - Autre (2 occurrences)
trends:
  - "Forge v2 (May 26): Both drafts expired as Ignoré — neither was dispatched
    despite time-sensitive angles (ACPR regulatory pressure for one account,
    post-sample silence follow-up for another). Both accounts appear to have been
    deprioritised on this date with no inbound signals explaining the halt. The
    regulatory-pressure angle in particular had a concrete compliance hook that
    went undeployed."
  - "Forge v2 (May 26): This is the third batch (after May 15 and May 22) where
    multiple drafts in a single day expired as Ignoré with no intervening
    client-side signal. The pattern suggests Forge v2 continues to generate
    drafts for accounts Antoine has placed on informal hold. A deal-stage or
    last-outbound recency gate before draft generation would reduce noise across
    all three of these observed batches."
---
