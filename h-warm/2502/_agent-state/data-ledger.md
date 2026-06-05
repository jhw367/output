# Warm2502 Data Ledger

No datasets imported yet under the 2026-06-06 Digital Twin architecture.

## Expected dataset families
- HomeWizard P1 Gas
- HomeWizard P1 Electricity
- HomeWizard Water
- KNMI data
- EV charging data
- future energy datasets, only after explicit mapping into the GitHub ingest structure

## Canonical raw-data archive target
Uploaded files must be archived unchanged under:

```text
data/raw/<dataset-family>/YYYY/MM/<timestamp>__<original-filename>
```

Initial dataset-family keys:

- `homewizard-p1-gas`
- `homewizard-p1-electricity`
- `homewizard-water`
- `knmi`
- `ev-charging`

## Future receipt format
For every uploaded/imported dataset, record:

- receipt date/time;
- upload channel;
- dataset family;
- original filename;
- raw archive path;
- file hash/checksum;
- validation result;
- processed path, if any;
- model-run ID impacted, if any;
- dashboard version impacted, if any;
- differences versus previous dataset/model/dashboard version;
- issues or anomalies.
