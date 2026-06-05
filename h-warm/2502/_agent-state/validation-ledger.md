# Warm2502 Validation Ledger

No dataset or model validation has been run yet under the 2026-06-06 Digital Twin architecture.

## Validation stance
Every uploaded HomeWizard, water, EV or KNMI file must be validated before it is used for processed data, model runs, dashboards or scenario outputs.

Validation does not imply automatic optimisation when data is absent. Missing data must be reported as a limitation unless J H explicitly approves an optimisation/imputation rule.

## Future validation entry format
- date/time;
- upload channel;
- input file or model version;
- dataset family;
- checks performed;
- result;
- errors/warnings;
- decision: accepted / needs correction / rejected;
- downstream effect: archived only / processed / model update / dashboard update;
- difference versus previous version, if applicable.
