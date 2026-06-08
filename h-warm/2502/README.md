# Warm2502

Warm2502 is the home-energy Digital Twin project.

## Source of truth

GitHub is the durable source of truth for architecture, decisions, models, workflows, backlogs and agent behaviour.

Start here:

1. `project-register.md`
2. `architecture/ADR-2026-06-06-warm2502-digital-twin.md`
3. `_agent-state/README.md`

## Goal

Evolve Warm2502 from chat/files/analyses into a reproducible Digital Twin that can process selected uploaded data, preserve historical energy context, run simulations, compare scenarios, evaluate investments and reproduce results.

## Current operating model

Warm2502 is **upload-driven**.

It must not run on a daily or other automatic time schedule for data checks or model updates.

On receipt of selected new HomeWizard, water, EV, SolarEdge, financial or KNMI files, the target flow is:

1. validate selected files;
2. store source material in the canonical ingest structure;
3. update dataset and version ledgers;
4. run a reproducible Digital Twin update;
5. recalculate Htr, energy balance and scenario outcomes where inputs allow;
6. update dashboard and ledgers;
7. report differences from the previous version.

Prepared but not yet automatically scheduled:

- Telegram upload pipeline;
- GitHub ingest structure;
- reproducible model runs;
- dataset versioning;
- dashboard update protocol;
- Home Assistant roadmap;
- weekly improvement backlog for model, dashboard and user interface.

## Current decision rule

Keep A_plus as the default route unless B becomes at least 10 percent better on 15-year TCO or 15-year NPV. When results are close, prefer the simpler system.

| Decision item | Current position | Confidence |
|---|---|---|
| Main heating route | A_plus remains default | medium |
| Alternative route | B remains benchmark challenger | medium |
| First priority | reduce demand before adding complexity | high |
| Reporting standard | separate facts, derived values, assumptions and scenarios | high |

## Improved output format

Future Warm2502 output should use this structure:

1. decision banner;
2. model status;
3. evidence table;
4. dashboard cards;
5. scenario comparison;
6. delta versus previous run;
7. next useful action.

## Standard dashboard blocks

| Block | Question answered |
|---|---|
| Model status | How current and reliable is the model? |
| Energy balance | What happens to electricity, PV, battery and EV? |
| Heat model | What does the building need? |
| Battery value | What is the 2026 and 2027 value? |
| Scenario comparison | Which gas-free route is better? |
| Action view | What input or decision is needed next? |

## Current model rules

| Rule | Meaning |
|---|---|
| 2026 and 2027 separated | avoids mixing different settlement contexts |
| EV home charging derived | avoids fixed annual home-charging assumptions |
| Battery value split | shows cash and wear-adjusted value separately |
| A_plus versus B only | keeps the decision model focused |
| Confidence explicit | avoids false precision |

## Provisional project-register figures

These values are current project-register figures and were not recalculated by this documentation update.

| Item | Current provisional value |
|---|---:|
| 2027 cash benefit | about 80 EUR per month |
| 2027 value including battery wear | about 60 EUR per month |
| Payback with VAT refund | about 5.5 years |
| Payback without VAT refund | about 7.0 years |

## Next useful run

The next useful run should be triggered by selected uploaded files. It should update the heat-loss estimate, daily ledgers, financial view, dashboard cards and scenario comparison.
