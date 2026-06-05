# Warm2502 Project Register

## Status
Active project register. GitHub is the primary source of truth.

## Architecture rule
Trust order for Warm2502:

1. GitHub
2. Project register
3. Agent memory
4. Chat history

Agents must consult this GitHub project register and linked decision records before asking for clarification about project structure, storage locations, repository choices, workflows, agent behaviour, backlogs, datasets, models or earlier decisions.

## Core decision records
- `architecture/ADR-2026-06-06-warm2502-digital-twin.md` — accepted architecture decision: GitHub source of truth, Digital Twin target, Telegram interface, upload-driven operating model, Home Assistant preparation and agent behaviour.
- `architecture/home-assistant-roadmap.md` — preparation-only roadmap for future Home Assistant coupling; not an implementation approval.

## Project objective
Build a reproducible Digital Twin for the home energy system, preserving all relevant knowledge, data, decisions and models outside chat.

## Primary interface
Telegram is the primary operational user interface.

## Operating model
Warm2502 must run through an **upload-driven model**, not through automatic time-based ingestion.

When the user uploads new HomeWizard, water, EV or KNMI files, the agent must:

1. recognise the dataset family;
2. validate the file;
3. archive the raw file unchanged in the correct GitHub path;
4. update dataset/version ledgers;
5. run a reproducible Digital Twin model update;
6. recalculate Htr, energy balance and scenario outputs where inputs are sufficient;
7. update dashboard and ledgers;
8. report differences against the previous version.

Explicitly excluded for now:

- no daily data checks;
- no time-scheduled Warm2502 cron runs;
- no automatic optimisation based on missing data;
- no alternative storage locations outside the existing GitHub repository structure.

## Primary repository/location
Current GitHub repository: `jhw367/output`.

Current project root in this repository:

```text
h-warm/2502/
```

## Planned project structure

```text
h-warm/2502/
├── README.md
├── project-register.md
├── architecture/
│   ├── ADR-2026-06-06-warm2502-digital-twin.md
│   └── home-assistant-roadmap.md
├── _agent-state/
│   ├── README.md
│   ├── run-ledger.md
│   ├── decision-ledger.md
│   ├── data-ledger.md
│   ├── model-ledger.md
│   ├── validation-ledger.md
│   └── backlog.md
├── data/
│   ├── raw/
│   │   ├── homewizard-p1-gas/
│   │   ├── homewizard-p1-electricity/
│   │   ├── homewizard-water/
│   │   ├── knmi/
│   │   └── ev-charging/
│   ├── processed/
│   └── documentation/
│       ├── upload-driven-ingest.md
│       └── dataset-versioning.md
├── models/
│   └── model-run-protocol.md
├── simulations/
├── dashboards/
│   └── dashboard-update-protocol.md
└── reports/
```

## Supported dataset families
- HomeWizard P1 Gas
- HomeWizard P1 Electricity
- HomeWizard Water
- KNMI data
- EV charging data
- future energy datasets, only after explicit mapping into the GitHub ingest structure

## Required prepared components
Prepare, but do not yet schedule or deploy:

- Telegram upload pipeline;
- GitHub ingest structure;
- reproducible model runs;
- dataset versioning;
- dashboard update flow;
- Home Assistant roadmap;
- weekly improvement backlog for model, dashboard and user interface.

## Home Assistant stance
Prepare only. Do not implement until explicitly approved.

Preferred direction:
- Raspberry Pi 4 or better
- local-first
- cloud-independent where possible
- prepared for HomeWizard, SolarEdge, battery, EV and Digital Twin coupling

## Agent rule
Agents may improve documentation, ledgers, reproducibility and data-quality structure. Agents must not change fundamental architecture without explicit approval.

Agents must not create Warm2502 cronjobs or time-triggered automation unless J H explicitly approves that change in a later decision.
