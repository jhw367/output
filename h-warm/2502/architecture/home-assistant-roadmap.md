# Warm2502 Home Assistant Roadmap

## Status
Preparation only. No Home Assistant implementation is approved.

## Purpose
Prepare Warm2502 for a future local-first Home Assistant integration without introducing premature infrastructure or alternative source-of-truth storage.

## Preferred direction

- Raspberry Pi 4 or better;
- local-first architecture;
- cloud-independent where possible;
- future coupling with HomeWizard, SolarEdge, battery, EV and the Digital Twin.

## Preparation work allowed now

- Document candidate Home Assistant architecture.
- Map future entities/sensors needed by the Digital Twin.
- Define how Home Assistant data would be exported into the GitHub ingest structure.
- Identify local storage and backup requirements.
- Define security/privacy constraints.
- Keep dashboards/model outputs reproducible from GitHub.

## Not approved yet

- Installing Home Assistant.
- Connecting live devices.
- Creating Home Assistant automations.
- Creating time-scheduled Warm2502 data checks.
- Using Home Assistant as the source of truth instead of GitHub.

## Future integration principle
Home Assistant may become a local data-collection and control layer, but GitHub remains the source of truth for Warm2502 decisions, ledgers, datasets, model manifests, dashboards and reproducibility records.
