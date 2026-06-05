# Warm2502 Reproducible Model Run Protocol

## Status
Prepared protocol. No model version has been formalised yet.

## Trigger
A model run is triggered by validated uploaded data, not by a time schedule.

## Required manifest
Each model run must create or update a manifest containing:

- model-run ID;
- run timestamp;
- trigger upload(s);
- input dataset paths and hashes;
- processing/model code path;
- assumptions and parameter versions;
- Htr method and result;
- energy-balance method and result;
- scenario set and outputs;
- dashboard version updated;
- comparison with previous model run;
- validation status;
- known limitations.

## Calculation stance

- Recalculate Htr only when required inputs are sufficient or an approved assumption exists.
- Recalculate energy balance from validated data only.
- Recalculate scenario outcomes from recorded assumptions and dataset versions.
- Do not optimise around missing data unless a later GitHub decision explicitly approves that rule.

## Reproducibility rule
A future agent must be able to reproduce a model output from GitHub alone: raw data hashes, processed data, model code/config, assumptions and ledgers must all live under `h-warm/2502/`.
