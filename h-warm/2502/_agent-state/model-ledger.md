# Warm2502 Model Ledger

No Digital Twin model version has been formalised yet under the 2026-06-06 architecture decision.

## Required future model outputs
Upload-triggered model runs must be reproducible and must record whether they recalculated:

- Htr;
- energy balance;
- scenario outcomes;
- dashboard metrics;
- differences versus the previous model version.

## Future model entry format
- model ID/version;
- run timestamp;
- trigger file(s);
- input datasets and dataset versions;
- code/model artifact path;
- assumptions;
- parameters;
- Htr method and result;
- energy-balance method and result;
- scenario set and outputs;
- comparison with previous model version;
- validation status;
- reproducibility notes.
