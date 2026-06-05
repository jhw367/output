# Warm2502 Agent State

## Purpose
This directory stores operational state for Warm2502 agents. Chat is temporary; durable project state belongs in GitHub.

## Required preflight for agents
Before asking the user to clarify Warm2502 project structure, storage locations or earlier decisions, read:

1. `../project-register.md`
2. `../architecture/ADR-2026-06-06-warm2502-digital-twin.md`
3. relevant ledgers in this directory

## Ledgers
- `run-ledger.md` — agent runs and outcomes
- `decision-ledger.md` — decisions and changes awaiting/after approval
- `data-ledger.md` — dataset receipts, source files, validation status and archive locations
- `model-ledger.md` — model versions and assumptions
- `validation-ledger.md` — data/model validation results
- `backlog.md` — proposed improvements and pending work

## Boundaries
Agents may improve data quality, documentation, ledgers, dashboards and reproducibility. Agents must not change fundamental architecture without explicit approval.
