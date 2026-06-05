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
- `architecture/ADR-2026-06-06-warm2502-digital-twin.md` — accepted architecture decision: GitHub source of truth, Digital Twin target, Telegram interface, Home Assistant preparation and agent behaviour.

## Project objective
Build a reproducible Digital Twin for the home energy system, preserving all relevant knowledge, data, decisions and models outside chat.

## Primary interface
Telegram is the primary operational user interface.

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
│   └── ADR-2026-06-06-warm2502-digital-twin.md
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
│   ├── processed/
│   └── documentation/
├── models/
├── simulations/
├── dashboards/
└── reports/
```

## Supported dataset families
- HomeWizard P1 Gas
- HomeWizard P1 Electricity
- HomeWizard Water
- KNMI data
- EV charging data
- future energy datasets

## Home Assistant stance
Prepare only. Do not implement until explicitly approved.

Preferred direction:
- Raspberry Pi 4 or better
- local-first
- cloud-independent where possible
- prepared for HomeWizard, SolarEdge, battery, EV and Digital Twin coupling

## Agent rule
Agents may improve documentation, ledgers, reproducibility and data-quality structure. Agents must not change fundamental architecture without explicit approval.
