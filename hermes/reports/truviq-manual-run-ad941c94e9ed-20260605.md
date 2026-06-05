# Truviq manual cron run report — ad941c94e9ed

**Date:** 2026-06-05  
**Job:** `ad941c94e9ed` — Truviq health-market hypothesis research — GitHub-state

## 1. Hypotheses investigated

- **H-003 — AZWA basisfunctionaliteiten D5/D6**
  - Question: do official AZWA sources confirm a regional owner, timing or funding route that makes basisfunctionaliteiten actionable for Truviq workflow-fit meetings?
  - Outcome: strengthened. Official sources now confirm handreikingen, regional werkagenda route, VWS/SPUK 2027–2029 preparation and national 2030 coverage target.

- **H-004 — HLTH Europe 2026 sponsor/speaker routing**
  - Question: does official HLTH data reveal named NL provider/payer/programme participants or a changed event role for Truviq?
  - Outcome: strengthened. HLTH official sponsor page lists Truviq Systems BV as a 2026 sponsor; official speaker/attendee pages provide named NL routing targets and audience categories.

## 2. New sources found

### H-003 AZWA

- `https://www.zorgakkoorden.nl/actueel/nieuws/Eerste-handreikingen-voor-AZWA-basisfunctionaliteiten/`
- `https://www.zorgakkoorden.nl/programmas/aanvullend-zorg-en-welzijnsakkoord/Versterking-samenwerking-zorg--en-sociaal-domein/`
- `https://www.zn.nl/actueel/zorgverzekeraars-sluiten-aanvullend-zorg-en-welzijnsakkoord-azwa/`

### H-004 HLTH

- `https://hlth.com/events/europe/sponsors`
- `https://hlth.com/events/europe/speakers`
- `https://hlth.com/events/europe/who-attends`

## 3. `_agent-state` files changed

- `truviq/_agent-state/changed-since-last-publish.md`
- `truviq/_agent-state/hypotheses.md`
- `truviq/_agent-state/negative-searches.md`
- `truviq/_agent-state/publish-gate.md`
- `truviq/_agent-state/run-ledger.md`
- `truviq/_agent-state/source-ledger.md`

Public outputs also changed:

- `truviq/health-market-analysis.html`
- `truviq/hlth-europe-2026-first-deal-sprint-plan.md`
- `truviq/hlth-europe-2026-first-deal-sprint-plan.html`
- `truviq/truviq-health-match-readout.md`
- `truviq/truviq-health-match-readout.html`

## 4. Token use, excluding cache

- Completed cron run recorded in `state.db`: **137,337 tokens excluding cache**
  - Input: 127,855
  - Output: 9,482
  - Cache read: 1,089,024, excluded from this figure

Operational note: a later session record for the same job exists with no `ended_at` and no saved messages after the manual tick command timed out at shell level. It recorded **273,885 tokens excluding cache**. Because it has no completed cron final message, the completed-run figure above is the clean run metric; the conservative total observed after the manual trigger is **411,222 tokens excluding cache**.

## 5. HTML-dashboard publication decision

**Yes — publication was justified and was already performed by the job.**

Reason:

- H-003 produced official funding/timing/owner-route evidence for AZWA basisfunctionaliteiten.
- H-004 materially changed the HLTH campaign route because Truviq is now confirmed as a 2026 sponsor and official HLTH data gives named NL routing targets.
- These are not cosmetic changes; they change priority, meeting script and next-action logic.

## Commits produced by the job

- `bfde8b4` — Update Truviq AZWA D5D6 research signal
- `227f7a2` — Refresh AZWA dashboard dates and wording
- `f747fd7` — Update Truviq HLTH sponsor routing intelligence

## Practical next test

Download and inspect the official AZWA werkagenda/format document to identify the first specific regional owner, reporting duty and likely buyer route.
