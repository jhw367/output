# Warm2502 Upload-Driven Ingest Protocol

## Status
Prepared protocol. No automatic time schedule is approved.

## Scope
This protocol applies when J H uploads one or more files through Telegram that appear to belong to one of these dataset families:

- HomeWizard P1 Gas;
- HomeWizard P1 Electricity;
- HomeWizard Water;
- KNMI;
- EV charging.

## Non-goals

- No daily data checks.
- No scheduled Warm2502 cron runs.
- No automatic optimisation based on missing data.
- No storage outside `jhw367/output/h-warm/2502/` as source of truth.

## Ingest flow

1. Receive uploaded file(s) from Telegram.
2. Preserve original filename and upload metadata.
3. Detect dataset family.
4. Validate file structure, time range, units and required fields.
5. Compute checksum/hash for version tracking.
6. Archive raw file unchanged under `data/raw/<dataset-family>/YYYY/MM/`.
7. Write or update processed outputs under `data/processed/<dataset-family>/` only after validation succeeds.
8. Update `_agent-state/data-ledger.md` and `_agent-state/validation-ledger.md`.
9. Trigger a reproducible model run if validation passes and required inputs are sufficient.
10. Recalculate Htr, energy balance and scenario outputs where inputs allow.
11. Update `_agent-state/model-ledger.md`, dashboard files and dashboard protocol metadata.
12. Report differences versus the previous dataset/model/dashboard version.

## Dataset family keys

- HomeWizard P1 Gas → `homewizard-p1-gas`
- HomeWizard P1 Electricity → `homewizard-p1-electricity`
- HomeWizard Water → `homewizard-water`
- KNMI → `knmi`
- EV charging → `ev-charging`

## Validation minimums

Every ingest must check at least:

- readable file format;
- dataset family confidence;
- timestamp/date columns;
- units and expected numeric ranges;
- duplicate records;
- time coverage and gaps;
- source filename/path;
- checksum/hash;
- whether the file is accepted, rejected or archived-only.

## Difference report minimums

After a successful upload-driven model/dashboard update, report:

- new dataset files accepted/rejected;
- changed time coverage;
- Htr change versus previous model version;
- energy-balance change versus previous model version;
- scenario-output changes;
- dashboard changes;
- remaining limitations/missing data.
