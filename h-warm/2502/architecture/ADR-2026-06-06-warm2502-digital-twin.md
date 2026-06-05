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

## Decision 2 — Warm2502 Digital Twin
Warm2502 develops into a reproducible Digital Twin of the home.

The Digital Twin must eventually be able to:

- process current energy data;
- preserve historical energy data;
- run simulations;
- compare scenarios;
- evaluate investments;
- reproduce results.

## Decision 3 — Telegram as user interface
Telegram is the primary operational interface for the project.

Target workflow:

1. User uploads energy data.
2. Agent recognises file type.
3. Agent validates content.
4. Agent archives source data.
5. Agent places data automatically in the correct GitHub location.
6. Agent updates the Digital Twin.

Supported datasets:

- HomeWizard P1 Gas
- HomeWizard P1 Electricity
- HomeWizard Water
- KNMI data
- EV charging data
- future energy datasets

## Decision 4 — Home Assistant preparation
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

## Decision 5 — Agent behaviour
Dora, Hermes, Codex and future agents must actively contribute to improving:

- the Digital Twin;
- data quality;
- reproducibility;
- dashboards;
- storage structure;
- documentation.

Agents may propose improvements but must not change fundamental architecture without explicit approval.

## Resulting architecture direction
Warm2502 develops into an independently reproducible energy platform in which Telegram, GitHub, Home Assistant and the Digital Twin work together, with GitHub as the permanent source of truth.

## Operational instruction for agents
Before acting on Warm2502 tasks, agents must first read:

1. `h-warm/2502/project-register.md`
2. `h-warm/2502/architecture/ADR-2026-06-06-warm2502-digital-twin.md`
3. relevant files under `h-warm/2502/_agent-state/`
4. relevant raw or processed dataset documentation under `h-warm/2502/data/`

If the user asks a question already answerable from GitHub, answer from GitHub instead of asking for clarification.
