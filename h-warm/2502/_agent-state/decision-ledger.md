# Warm2502 Decision Ledger

## Accepted

### 2026-06-06 — WARM2502 Digital Twin architecture decision
- GitHub is the primary source of truth.
- Trust order: GitHub > Project register > Agent memory > Chat history.
- Warm2502 evolves into a reproducible Digital Twin.
- Telegram is the primary operational interface.
- Home Assistant is prepared for but not implemented yet.
- Agents must improve reproducibility, data quality, dashboards, storage and documentation, but must not change fundamental architecture without explicit approval.

Source: `../architecture/ADR-2026-06-06-warm2502-digital-twin.md`

### 2026-06-06 — Upload-driven operating model
- Warm2502 must not automatically run on a time schedule for now.
- New HomeWizard, water, EV or KNMI files trigger the intended workflow.
- On upload, agents must validate files, place them in the correct GitHub map, update the Digital Twin, recalculate Htr, energy balance and scenarios, update dashboard/ledgers and report differences against the previous version.
- No daily data checks.
- No automatic optimisation based on missing data.
- Preparation is approved for the Telegram upload pipeline, GitHub ingest structure, reproducible model runs, dataset versioning, Home Assistant roadmap and weekly improvement of model/dashboard/UI.
- GitHub remains the only source of truth; new components must attach to the existing repository structure and must not introduce alternative storage locations.

Source: J H instruction recorded in `../architecture/ADR-2026-06-06-warm2502-digital-twin.md`
