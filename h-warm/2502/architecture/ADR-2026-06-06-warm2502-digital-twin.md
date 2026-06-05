# WARM2502 – Architecture Decision 2026-06-06

## Status
Accepted by J H on 2026-06-06.

## Purpose
Warm2502 must evolve from a collection of chats, files and analyses into a reproducible Digital Twin whose knowledge, data, decisions and models live outside chat.

## Decision 1 — GitHub is the source of truth
GitHub is the primary source of truth for Warm2502.

Chat conversations are temporary working memory. Architecture, decisions, backlog, datasets, models and documentation must be structurally recorded in GitHub.

Agents must consult GitHub before asking the user for clarification about project structure, storage locations or earlier decisions.

Trust order:

1. GitHub
2. Project register
3. Agent memory
4. Chat history

New components must directly connect to the existing repository structure under `h-warm/2502/` and must not introduce alternative storage locations for source-of-truth data, models, ledgers, dashboards or decisions.

## Decision 2 — Warm2502 Digital Twin
Warm2502 develops into a reproducible Digital Twin of the home.

The Digital Twin must eventually be able to:

- process uploaded current energy data;
- preserve historical energy data;
- run simulations;
- compare scenarios;
- evaluate investments;
- reproduce results;
- report differences against earlier dataset, model and dashboard versions.

## Decision 3 — Telegram as upload-driven user interface
Telegram is the primary operational interface for the project.

Warm2502 is upload-driven for now. It must not automatically run on a daily or other time-based schedule.

Target workflow when J H uploads HomeWizard, water, EV or KNMI files:

1. Agent receives uploaded file(s) through Telegram.
2. Agent recognises dataset family and file type.
3. Agent validates content before downstream use.
4. Agent archives the raw source file unchanged.
5. Agent places the file automatically in the correct GitHub location.
6. Agent records dataset version/hash and validation result in ledgers.
7. Agent updates the Digital Twin with a reproducible model run.
8. Agent recalculates Htr, energy balance and scenario outcomes where inputs are sufficient.
9. Agent updates dashboard and ledgers.
10. Agent reports differences compared with the previous version.

Supported datasets:

- HomeWizard P1 Gas
- HomeWizard P1 Electricity
- HomeWizard Water
- KNMI data
- EV charging data
- future energy datasets, only after explicit mapping into the GitHub ingest structure

## Decision 4 — No scheduled data checks or automatic missing-data optimisation
Warm2502 must not use time-triggered automation for now.

Explicit exclusions:

- no daily data checks;
- no scheduled Warm2502 cronjobs;
- no automatic optimisation based on missing data;
- no implicit imputation/fallbacks that alter calculations without a recorded rule and user approval.

Missing data must be reported as a limitation unless an approved model rule exists in GitHub.

## Decision 5 — Home Assistant preparation
Warm2502 must be prepared for a future Home Assistant implementation.

Do not implement Home Assistant yet.

Prepare for:

- local data collection;
- HomeWizard integration;
- SolarEdge integration;
- battery integration;
- EV integration;
- Digital Twin coupling.

Preferred direction:

- Raspberry Pi 4 or better;
- local-first architecture;
- cloud-independent where possible.

The preparation roadmap is recorded in `home-assistant-roadmap.md` and is not an implementation approval.

## Decision 6 — Agent behaviour
Dora, Hermes, Codex and future agents must actively contribute to improving:

- the Telegram upload pipeline;
- the GitHub ingest structure;
- the Digital Twin;
- data quality;
- reproducibility;
- dataset versioning;
- dashboards;
- storage structure;
- documentation;
- Home Assistant roadmap;
- model/dashboard/user-interface improvements.

Agents may propose and document improvements but must not change fundamental architecture or create time-scheduled Warm2502 automation without explicit approval.

## Resulting architecture direction
Warm2502 develops into an independently reproducible, upload-driven energy platform in which Telegram, GitHub, future Home Assistant and the Digital Twin work together, with GitHub as the permanent source of truth.

## Operational instruction for agents
Before acting on Warm2502 tasks, agents must first read:

1. `h-warm/2502/project-register.md`
2. `h-warm/2502/architecture/ADR-2026-06-06-warm2502-digital-twin.md`
3. relevant files under `h-warm/2502/_agent-state/`
4. relevant raw or processed dataset documentation under `h-warm/2502/data/`

If the user asks a question already answerable from GitHub, answer from GitHub instead of asking for clarification.

When a user uploads a relevant file, the agent must use the upload-driven protocol in `data/documentation/upload-driven-ingest.md` and update the ledgers before reporting completion.
