# Warm2502 Dashboard Update Protocol

## Status
Prepared protocol. Dashboard format not yet finalised.

## Trigger
Dashboard updates follow successful upload-driven validation and model runs. They are not scheduled daily checks.

## Minimum dashboard content

- dataset coverage by family;
- latest accepted upload/version;
- Htr current value and previous-version difference;
- energy-balance current result and previous-version difference;
- scenario outcome comparison;
- data-quality warnings;
- missing-data limitations;
- links to ledgers/model-run manifests.

## Difference reporting
Every dashboard update must make changes visible against the previous dashboard/model version:

- added/rejected datasets;
- changed measurement coverage;
- changed Htr;
- changed energy-balance result;
- changed scenario outcomes;
- changed assumptions or validation status.

## Design rule
Use clear, evidence-first information design: comparisons, hierarchy, minimal chartjunk and explicit uncertainty.
