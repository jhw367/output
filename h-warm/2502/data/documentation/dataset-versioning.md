# Warm2502 Dataset Versioning

## Principle
Raw uploaded source files are immutable evidence. They must be stored unchanged and referenced by hash from ledgers, processed outputs, model runs and dashboards.

## Canonical raw path

```text
data/raw/<dataset-family>/YYYY/MM/<YYYYMMDD-HHMMSS>__<original-filename>
```

Example:

```text
data/raw/homewizard-p1-electricity/2026/06/20260606-010000__homewizard-electricity.csv
```

## Canonical processed path

```text
data/processed/<dataset-family>/<processing-version>/<derived-file>
```

Processed files must reference their raw input files and hashes.

## Required metadata per dataset version

Record in `_agent-state/data-ledger.md`:

- dataset family;
- upload timestamp;
- original filename;
- raw GitHub path;
- checksum/hash;
- validation result;
- processed path, if any;
- model-run ID, if used;
- dashboard version, if used;
- differences versus previous version.

## Version comparison rule
Every accepted upload must be compared against the previous accepted version for the same dataset family where possible. If no previous version exists, record it as the baseline.
