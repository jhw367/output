# Truviq health-market research agent state

Purpose: compact durable state for the recurring Truviq NL/EU health-market research agent. Read these files before any run and avoid re-reading large public outputs unless needed for state repair or publication.

Operating rules:
- Select exactly one active hypothesis per run.
- Check `source-ledger.md` and `negative-searches.md` before external lookups.
- Use max 3 new public sources per run; prefer official, procurement, programme, insurer/provider, event/partner pages.
- Update ledgers after every source or negative query.
- Publish public outputs only when `publish-gate.md` criteria are met.
- Keep outputs meeting-first: qualify 20-minute workflow-fit meetings, not generic orchestration pitches.
- Treat `addie-proposition-system.md` as the proposition-optimization layer: each material run should connect the selected hypothesis to one narrow Addie micro-cycle (Observe → Simulate → Synthesize → Evaluate → Improve → Deploy/Test) without broadening source scope.

Public outputs reconstructed from local files on 2026-06-05:
- `../health-market-analysis.html`
- `../sales-people-health-opportunity.html`
- `../hlth-europe-2026-first-deal-sprint-plan.md`
- `../hlth-europe-2026-first-deal-sprint-plan.html`
- `../truviq-health-match-readout.md`
- `../truviq-health-match-readout.html`

Process layers:
- `addie-proposition-system.md` — Addie / Adaptive Dynamic Development & Intelligence Engine for autonomous proposition definition and optimization.
