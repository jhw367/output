# Warm2502

Warm2502 is the home-energy Digital Twin project.

## Source of truth
GitHub is the only durable source of truth for architecture, decisions, datasets, models, workflows, backlogs and agent behaviour.

Start here:

1. `project-register.md`
2. `architecture/ADR-2026-06-06-warm2502-digital-twin.md`
3. `_agent-state/README.md`

## Goal
Evolve Warm2502 from chat/files/analyses into a reproducible Digital Twin that can process uploaded current data, preserve historical energy data, run simulations, compare scenarios, evaluate investments and reproduce results.

## Current operating model
Warm2502 is **upload-driven**.

It must not run on a daily or other automatic time schedule for data checks or model updates.

On receipt of new HomeWizard, water, EV or KNMI files through Telegram, the target flow is:

1. validate uploaded files;
2. store raw files in the canonical GitHub ingest structure;
3. update dataset/version ledgers;
4. run a reproducible Digital Twin update;
5. recalculate Htr, energy balance and scenario outcomes where inputs allow;
6. update dashboard and ledgers;
7. report differences from the previous version.

Prepared but not yet automatically scheduled:

- Telegram upload pipeline;
- GitHub ingest structure;
- reproducible model runs;
- dataset versioning;
- Home Assistant roadmap;
- weekly improvement backlog for model, dashboard and user interface.
