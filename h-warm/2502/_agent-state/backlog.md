# Warm2502 Backlog

## Accepted operating constraints

- Warm2502 is upload-driven for now.
- No daily data checks.
- No scheduled Warm2502 cronjobs.
- No automatic optimisation based on missing data.
- GitHub remains the only durable source of truth.
- New components must attach to `h-warm/2502/` and must not introduce alternative storage locations.

## Proposed next steps — upload-driven foundation

- Define file-recognition rules for Telegram uploads:
  - HomeWizard P1 Gas;
  - HomeWizard P1 Electricity;
  - HomeWizard Water;
  - KNMI;
  - EV charging.
- Define validation rules per dataset family.
- Implement canonical raw-data archive layout under `data/raw/<dataset-family>/YYYY/MM/`.
- Define processed-data outputs under `data/processed/<dataset-family>/`.
- Create first Digital Twin data model schema.
- Create reproducible model-run manifest format.
- Define Htr recalculation inputs, assumptions and fallback rules.
- Define energy-balance calculation protocol.
- Define scenario-output comparison protocol.
- Decide dashboard format and publication rule.
- Prepare, but do not implement, Home Assistant integration plan.

## Weekly improvement backlog — preparation only

These are allowed as planning/documentation improvements, not automatic time-triggered runs:

- Improve model equations, assumptions and validation tests.
- Improve dashboard clarity, comparisons and version-difference reporting.
- Improve Telegram user interface for upload acknowledgement and error messages.
- Improve dataset versioning and reproducibility metadata.
- Improve Home Assistant roadmap and local-first integration design.

## Requires explicit approval before implementation

- Home Assistant deployment.
- Any cron/scheduled agents for Warm2502.
- Daily data checks.
- Automatic optimisation based on missing data.
- Fundamental architecture changes.
- Movement of existing historical Warm2502 artifacts into the new canonical structure.
- Any storage path outside `jhw367/output/h-warm/2502/` for Warm2502 source-of-truth data, models, ledgers or dashboards.
