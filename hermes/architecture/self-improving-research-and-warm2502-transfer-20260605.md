# Hermes self-improvement layer for research jobs + Warm 2502 transfer design

**Date:** 2026-06-05  
**Status:** design only — no cronjobs changed  
**Primary domains:** Truviq health-market research, later Hamer-Kuling genealogy, and preparation for Warm 2502 transfer into GitHub/Hermes.

## 0. Executive summary

This document designs a **Hermes self-improvement layer**: a meta-agent that runs after each research-agent run and evaluates whether the run was useful.

The meta-agent is not a replacement for the research agent. It is a lightweight reviewer that reads the research run output, GitHub state files, token data and changed files, then writes a structured evaluation into GitHub.

Core rule:

> The meta-agent may write ledgers, summaries and recommendations, but it may not change cronjobs, schedules, prompts, providers or delivery settings without explicit human approval.

Practical purpose:

- Make every Truviq run accountable.
- Reduce repetition.
- Detect token waste early.
- Prevent Telegram spam during quota/provider failures.
- Build a reliable GitHub-backed memory trail.
- Prepare the same pattern for Warm 2502 once the user exports reliable ChatGPT context and source inputs.

---

## 1. Architecture of the meta-agent

### 1.1 Components

```text
Hermes cron scheduler
│
├── Research agent: Truviq / Hamer-Kuling / future Warm 2502
│   ├── Reads domain state from GitHub
│   ├── Performs one hypothesis/proof-goal run
│   ├── Updates domain state
│   ├── Publishes only if publish gate is met
│   └── Writes short final run summary
│
└── Self-improvement meta-agent
    ├── Reads latest research-agent run output
    ├── Reads GitHub state ledgers
    ├── Reads Git diff / changed files
    ├── Reads session token counters where available
    ├── Scores usefulness, novelty, repetition and publish quality
    ├── Writes meta-ledgers in GitHub
    ├── Proposes schedule / prompt / model changes
    └── Stops at recommendation; no cron mutation without approval
```

### 1.2 Invocation pattern

Recommended safe pattern:

- Keep the research job as the primary cronjob.
- Add a **separate meta-agent cronjob** that runs after the research window.
- The meta-agent reads state and recent Git commits instead of being chained directly inside the same research-agent prompt.

For Truviq, if the research job runs Monday/Wednesday/Friday at `07:20 UTC`, the meta-agent can run at:

```cron
50 7 * * 1,3,5
```

Reason: give the research job time to finish, commit/push, and record token usage.

Alternative later:

- Use `context_from` if Hermes cron chaining is stable for this profile.
- Still keep the meta-agent as a separate job so quota/provider failure in the research agent does not automatically trigger a second expensive retry path.

### 1.3 Meta-agent scope

The meta-agent evaluates:

- Was a hypothesis actually tested?
- Was the evidence new compared with `source-ledger.md`?
- Did the run update useful GitHub state?
- Did the run publish only when the publish gate was met?
- Did the run repeat old searches or sources?
- Did the token use match the value created?
- Was Telegram output necessary and compact?
- Should the next run frequency stay the same, go up, go down, or pause for approval?

The meta-agent does **not**:

- perform broad new research;
- rewrite public dashboards except for its own report/index files;
- alter cron schedules;
- alter prompts;
- resume/pause/delete jobs;
- retry after quota/provider errors;
- publish large Telegram reports.

---

## 2. Required GitHub files

### 2.1 Shared directory pattern

Each domain gets two state layers:

```text
output/<domain>/_agent-state/          # research-agent memory
output/<domain>/_meta-agent/           # self-improvement memory
```

For Truviq:

```text
output/truviq/_agent-state/
output/truviq/_meta-agent/
```

For Hamer-Kuling later:

```text
output/family/_agent-state/
output/family/_meta-agent/
```

For Warm 2502 later:

```text
output/h-warm/2502/_agent-state/
output/h-warm/2502/_meta-agent/
```

### 2.2 Research-agent files already used

The research agent should continue using:

```text
_agent-state/README.md
_agent-state/run-ledger.md
_agent-state/hypotheses.md
_agent-state/source-ledger.md
_agent-state/negative-searches.md
_agent-state/changed-since-last-publish.md
_agent-state/publish-gate.md
```

### 2.3 New meta-agent files

```text
_meta-agent/README.md
_meta-agent/evaluation-ledger.md
_meta-agent/decision-ledger.md
_meta-agent/frequency-ledger.md
_meta-agent/token-ledger.md
_meta-agent/error-ledger.md
_meta-agent/prompt-improvement-backlog.md
_meta-agent/publication-ledger.md
_meta-agent/approval-queue.md
```

Purpose per file:

- `README.md`
  - Explains what the meta-agent may and may not do.
  - Contains safety rules and approval boundaries.

- `evaluation-ledger.md`
  - One compact evaluation per research run.
  - Captures novelty, usefulness, source quality, repetition and next question.

- `decision-ledger.md`
  - Records recommendations and whether they were later approved/rejected by the user.
  - Example: “Recommend lowering frequency from 3×/week to 2×/week after 3 low-value runs.”

- `frequency-ledger.md`
  - Tracks run quality over time and frequency recommendations.
  - Should never directly change cron.

- `token-ledger.md`
  - Tracks tokens excluding cache and including cache.
  - Separates successful research, public rebuild, meta-review and error runs.

- `error-ledger.md`
  - Records quota/provider failures and whether output was suppressed.
  - Used to avoid retry loops and Telegram spam.

- `prompt-improvement-backlog.md`
  - Draft prompt changes with rationale.
  - Changes stay proposed until approved.

- `publication-ledger.md`
  - Records whether public HTML/MD was updated and why.
  - Helps audit if dashboards are being overpublished.

- `approval-queue.md`
  - Human-readable list of actions that need J H approval.
  - Example: schedule change, model change, prompt rewrite, delete old job.

---

## 3. Meta-agent evaluation schema

Each evaluation entry should use this compact format:

```markdown
## YYYY-MM-DD HH:MM UTC | <domain> | <research-job-id>

- Research hypothesis/proof goal: <ID + title>
- Run status: ok | no-change | provider-error | quota-error | partial | failed
- Novelty score: 0–5
- Usefulness score: 0–5
- Source quality score: 0–5
- Repetition score: 0–5 where 5 = too repetitive
- Publication decision: yes | no | correction-only | not-applicable
- Changed state files: <list>
- Changed public files: <list>
- New sources: <count + list or short summary>
- Tokens excluding cache: <number or unknown>
- Tokens including cache: <number or unknown>
- Verdict: useful | marginal | wasteful | blocked
- Next best critical question: <one question>
- Recommendation: keep schedule | lower frequency | raise frequency temporarily | prompt change | model change | pause and ask user
- Requires approval: yes/no; if yes, exact proposed action
```

Plain-language scoring:

- Novelty: did we learn something genuinely new?
- Usefulness: does it change action, priority, outreach, proof status or public output?
- Source quality: official/procurement/primary sources score high; general news/blog summaries score lower.
- Repetition: high score is bad; it means the run repeated old work.

---

## 4. Frequency decision rules

### 4.1 Baseline for Truviq

Current redesigned Truviq schedule:

```cron
20 7 * * 1,3,5
```

This remains the default.

### 4.2 Frequency up rules

The meta-agent may **recommend** increasing frequency, but may not apply it.

Recommend temporary higher frequency when at least one condition is true:

- There is a near-term commercial event, e.g. HLTH Europe campaign window.
- Two consecutive runs scored:
  - Novelty ≥ 4
  - Usefulness ≥ 4
  - Repetition ≤ 2
- A source indicates a time-sensitive procurement, tender, programme deadline or event deadline.
- The user explicitly asks for more frequent monitoring.

Recommended temporary schedules:

- Event sprint mode:

```cron
20 7 * * 1-5
```

- Short daily monitor for 7–14 days:

```cron
20 7 * * *
```

Hard rule: frequency-up requires explicit user approval.

### 4.3 Frequency down rules

Recommend lowering frequency when:

- Three consecutive runs have Novelty ≤ 2 and Usefulness ≤ 2.
- Two consecutive runs publish nothing and add no meaningful source-ledger or hypothesis change.
- Token cost exceeds budget for two consecutive runs without public or decision impact.
- Repetition score ≥ 4 twice in a row.

Recommended lower schedules:

- From 3×/week to 2×/week:

```cron
20 7 * * 1,4
```

- From 3×/week to weekly:

```cron
20 7 * * 1
```

Hard rule: frequency-down also requires explicit user approval, because reducing cadence is still a cron change.

### 4.4 Pause recommendation rules

Recommend pausing the research job when:

- Five consecutive runs are `marginal` or `wasteful`.
- Quota/provider errors occur repeatedly and cannot be avoided by model/provider selection.
- The domain has no active hypotheses left.
- The next action is human-only, e.g. “need private input from ChatGPT/export/user documents before Hermes can proceed.”

Pause requires explicit approval.

---

## 5. Token-budget rules

### 5.1 Per-run budget classes

For Truviq:

- State-only/meta-review run:
  - Target: 5k–20k tokens excluding cache.
- Normal hypothesis test:
  - Target: 25k–80k tokens excluding cache.
- Publish run with dashboard update:
  - Target: 80k–160k tokens excluding cache.
- Above 160k excluding cache:
  - Must be justified in `token-ledger.md`.

For Hamer-Kuling later:

- One proof-goal source run:
  - Target: 15k–60k tokens excluding cache.
- Complex identity reconciliation:
  - Target: 60k–120k tokens excluding cache.

For Warm 2502 later:

- Calculation-only or state update:
  - Target: 5k–25k tokens excluding cache.
- Scenario recalculation with dashboard rebuild:
  - Target: 25k–90k tokens excluding cache.
- Document ingestion or contradiction cleanup:
  - Target: 50k–120k tokens excluding cache.

### 5.2 Token decision rules

The meta-agent should flag:

- **Green:** tokens under budget and useful output.
- **Amber:** tokens over target but clear public/decision value.
- **Red:** tokens over target with no new source, no state delta and no decision value.

Red outcome actions:

- Write a recommendation to `decision-ledger.md`.
- Add a prompt improvement to `prompt-improvement-backlog.md`.
- Do not change cron automatically.
- Do not rerun automatically.

### 5.3 Model recommendations

The meta-agent may recommend model changes:

Use a stronger model such as GPT-5.5 only for:

- contradictory evidence;
- high-value commercial prioritization;
- public dashboard rewrite;
- important demolish check;
- genealogy identity reconciliation.

Use cheaper models for:

- state-file scoring;
- ledger updates;
- no-change detection;
- extracting simple source facts;
- deterministic report generation.

Changing model/provider always requires explicit approval.

---

## 6. Anti-spam and quota/provider-error rules

### 6.1 Error recognition

The meta-agent and research agents must treat these as hard stops:

- `rate limit`
- `quota`
- `insufficient_quota`
- `429`
- `provider overloaded`
- `exhausted credentials`
- `model unavailable`
- `unauthorized` / `401`
- `forbidden` / `403`
- repeated transport errors from the same provider

### 6.2 No retry-loop rule

If a quota/provider error is detected:

1. Stop the run.
2. Do not call the same provider again in a loop.
3. Do not trigger the research job again.
4. Write one compact entry to `error-ledger.md` if file tools are still available.
5. Return `[SILENT]` if the cron context supports silence and there is no user-actionable update.
6. If a message must be sent, send one short message only:
   - what failed;
   - whether files changed;
   - what approval/action is needed.

### 6.3 Telegram anti-spam rules

Telegram delivery should happen only when:

- there is new decision-useful information;
- public output changed;
- approval is required;
- a persistent error needs human action;
- the user manually triggered the run and asked for a report.

Do not send Telegram messages for:

- no-change meta-evaluations;
- state-only housekeeping;
- quota/provider errors that are already logged and not actionable;
- repeated warnings about the same unresolved issue.

Recommended pattern:

- Daily/weekly digest only if multiple non-urgent meta findings accumulate.
- Immediate message only for approval queue or high-value findings.

---

## 7. Critical questions the meta-agent asks the research agent

The meta-agent should interrogate each research run as if reviewing a junior analyst.

### 7.1 Novelty questions

- What exactly was new compared with `source-ledger.md`?
- Was this a new source, a changed source, or merely a reread?
- Did the run update a hypothesis status?
- Did the run add a negative-search entry to prevent future repetition?

### 7.2 Evidence questions

- Is the strongest source official, primary, procurement-related or only sector commentary?
- Which claim would fail if the source disappeared?
- Did the source prove a buyer, or only a context signal?
- Is the evidence enough to change commercial priority?

### 7.3 Commercial/proof value questions

For Truviq:

- Did this change an outreach angle?
- Did this identify a programme owner, buyer persona, funding route, tender, partner or event route?
- Does this help book a 20-minute workflow-fit meeting?
- What would disconfirm this as a real opportunity?

For Hamer-Kuling:

- Did this prove, weaken or reject a specific relationship/identity hypothesis?
- Is this source-backed enough for public narrative, or only private hypothesis state?
- Which exact source should be checked next?

For Warm 2502:

- Did this improve the financial model input quality?
- Which numbers are user-provided, invoice-backed, subsidy-backed, estimated or uncertain?
- Did this reduce the risk of presenting wrong savings/monthly-cost conclusions?
- What assumption most changes the outcome?

### 7.4 Publication questions

- Was the publish gate actually met?
- Which public claim changed?
- Would a user reading the dashboard make a different decision?
- If not, why publish?

### 7.5 Token-efficiency questions

- Did the token cost match the value of the result?
- Could the same evaluation have used only state files?
- Did the agent read large HTML files unnecessarily?
- Did the run use more than 3 new sources without justification?

---

## 8. Actions the meta-agent may perform automatically

Allowed automatic actions:

- Create missing `_meta-agent/` files.
- Append evaluation entries to `evaluation-ledger.md`.
- Append recommendations to `decision-ledger.md`.
- Append token records to `token-ledger.md`.
- Append errors to `error-ledger.md`.
- Append prompt-change proposals to `prompt-improvement-backlog.md`.
- Append approval items to `approval-queue.md`.
- Commit and push these meta-ledger updates to GitHub.
- Produce a compact Telegram summary when user-actionable.
- Return `[SILENT]` for no-change/no-action outcomes if running as cron and supported.

Automatic commit rule:

- Commit only if a ledger changed.
- Commit message format:

```text
Log <domain> research meta-evaluation
```

---

## 9. Actions that always require explicit approval

Always require user approval before:

- Creating a new cronjob.
- Pausing/resuming/deleting a cronjob.
- Changing a cron schedule.
- Changing a cron prompt.
- Changing model/provider.
- Changing delivery target.
- Enabling Telegram delivery for a new recurring job.
- Switching from no-agent/script-only to LLM-driven job, or the reverse.
- Deleting or archiving old state ledgers.
- Rewriting public dashboard structure substantially.
- Moving Warm 2502 private/source files into a public GitHub path.
- Publishing Warm 2502 financial assumptions publicly if source confidence is unclear.

Approval request format:

```markdown
## Approval needed

Proposed action: <exact action>
Why: <reason>
Risk if done: <risk>
Risk if not done: <risk>
Exact command/change planned: <cronjob update / file paths / model>
Rollback: <how to undo>
```

---

## 10. Proposed meta-agent prompt

```text
You are Hermes Agent running the self-improvement meta-agent for a research job.

Primary goal:
Evaluate the latest research-agent run for usefulness, novelty, repetition, token efficiency, publication quality and safety. Write durable evaluation state to GitHub. Do not modify cronjobs, schedules, providers, prompts or delivery settings without explicit human approval.

Input state:
Read the domain's research state files first:
- _agent-state/run-ledger.md
- _agent-state/hypotheses.md or proof-goals.md
- _agent-state/source-ledger.md
- _agent-state/negative-searches.md
- _agent-state/changed-since-last-publish.md
- _agent-state/publish-gate.md

Read or create the meta state files:
- _meta-agent/README.md
- _meta-agent/evaluation-ledger.md
- _meta-agent/decision-ledger.md
- _meta-agent/frequency-ledger.md
- _meta-agent/token-ledger.md
- _meta-agent/error-ledger.md
- _meta-agent/prompt-improvement-backlog.md
- _meta-agent/publication-ledger.md
- _meta-agent/approval-queue.md

Evaluate exactly one latest completed research run.

Hard safety rules:
- If quota/provider/rate-limit error is detected, log it once and stop. No retry-loop.
- If there is no new information and no approval needed, return [SILENT] when running as cron.
- Do not change cronjobs.
- Do not change schedules.
- Do not change prompts.
- Do not change model/provider.
- Do not delete state.

Scoring:
- Novelty score 0–5.
- Usefulness score 0–5.
- Source quality score 0–5.
- Repetition score 0–5 where 5 is bad.
- Token status: green / amber / red.
- Verdict: useful / marginal / wasteful / blocked.

Required output files:
Append a compact entry to evaluation-ledger.md.
Append to token-ledger.md if token counters are available.
Append to publication-ledger.md if public files changed.
Append to decision-ledger.md and approval-queue.md if a human decision is needed.
Append to prompt-improvement-backlog.md if the research prompt should improve.
Commit and push only if files changed.

Telegram output:
Use short Dutch bullets only when user-actionable.
```

---

## 11. Warm 2502 transfer preparation

### 11.1 Current known context

Warm 2502 already has public output in GitHub:

```text
output/h-warm/2026/h-warm-2502-investeringsroute-20260525-v2.html
output/h-warm/2026/h-warm-2502-preview-20260525-v2.png
output/h-warm/2026/h-warm-2502-investeringsroute-20260524.html
output/h-warm/2026/h-warm-2502-preview-20260524.jpg
output/h-warm/2026/h-warm-2502-investeringsroute-20260524.md
output/h-warm/2026/h-warm-2502-cashflow-model-20260524.json
```

Local source/project files also exist:

```text
/home/hermes/energie/H - Warm 2502 - archiefsamenvatting.md
/home/hermes/energie/H - Warm 2502 - investeringsroute en cashflow.md
/home/hermes/energie/H - Warm 2502 - investeringsroute.html
/home/hermes/energie/H - Warm 2502 - projectdashboard.html
```

Important prior correction:

- Earlier Warm 2502 financial output had wrong/weak assumptions.
- Current monthly costs, battery/inverter/charging-unit investments and subsidy treatment must be corrected from reliable input.
- Later-year support/subsidy assumptions were unclear and should not be presented as certain.

### 11.2 Goal of Warm 2502 transfer

Move Warm 2502 from mixed local/chat context into a GitHub-backed Hermes project where:

- assumptions are explicit;
- source documents are separated from compiled outputs;
- every number has confidence/provenance;
- model versions are reproducible;
- dashboards are regenerated from data rather than hand-edited claims;
- future Hermes runs can calculate and document without re-reading old chats blindly.

### 11.3 Proposed GitHub map structure for Warm 2502

Recommended public/non-secret structure:

```text
output/h-warm/2502/
├── README.md
├── _agent-state/
│   ├── README.md
│   ├── run-ledger.md
│   ├── assumptions-ledger.md
│   ├── source-ledger.md
│   ├── contradiction-ledger.md
│   ├── decision-ledger.md
│   ├── model-ledger.md
│   ├── changed-since-last-publish.md
│   └── publish-gate.md
├── _meta-agent/
│   ├── README.md
│   ├── evaluation-ledger.md
│   ├── decision-ledger.md
│   ├── frequency-ledger.md
│   ├── token-ledger.md
│   ├── error-ledger.md
│   ├── prompt-improvement-backlog.md
│   ├── publication-ledger.md
│   └── approval-queue.md
├── sources/
│   ├── README.md
│   ├── user-provided/
│   ├── quotes-invoices/
│   ├── subsidy-rules/
│   ├── tariffs-contracts/
│   └── product-specs/
├── data/
│   ├── assumptions.yaml
│   ├── scenarios.yaml
│   ├── cashflow-model.json
│   ├── source-confidence.csv
│   └── calculation-log.md
├── models/
│   ├── warm2502_cashflow.py
│   ├── validate_inputs.py
│   └── README.md
├── reports/
│   ├── warm2502-investment-route-latest.md
│   └── warm2502-risk-and-assumptions.md
└── public/
    ├── index.html
    ├── warm2502-dashboard-latest.html
    └── preview.png
```

If privacy is a concern, split into:

- public GitHub output: sanitized dashboards and non-sensitive assumptions;
- local private source store: invoices, personal addresses, contract details, meter data.

Do not publish private documents unless the user explicitly approves.

### 11.4 Input needed from ChatGPT before Hermes transfer

The user should export or paste a compact ChatGPT handoff with:

1. **Project objective**
   - What is Warm 2502 optimizing for: lowest monthly cost, fastest payback, resilience, comfort, CO2 reduction, grid independence, or another goal?

2. **Current baseline**
   - Current monthly energy cost.
   - Current gas/electricity usage.
   - Current contract type and tariff assumptions.
   - Current solar generation and self-consumption if known.

3. **Known investments**
   - Battery cost.
   - Inverter/hub cost.
   - Charging-unit cost.
   - Installation cost.
   - VAT treatment.
   - Any already-committed vs optional investments.

4. **Subsidies / reimbursements**
   - Which subsidy applies.
   - Amount.
   - Status: confirmed / likely / estimated / not applied.
   - Timing of payment.

5. **EV / ERE assumptions**
   - Expected kWh/year.
   - Reimbursement rate.
   - Whether reimbursement is certain, employer-related, or not applicable.
   - Important: do not assume employer/lease reimbursement unless explicitly confirmed.

6. **Future measures**
   - Heat pump route.
   - EMS/dynamic contract route.
   - Additional batteries.
   - Insulation or other home changes.
   - Which are planned, optional, or speculative.

7. **Correction list**
   - What was wrong in previous outputs.
   - Which numbers must be withdrawn.
   - Which assumptions must be downgraded.

8. **Source bundle**
   - Quotes/invoices.
   - Screenshots or PDFs of tariffs.
   - Subsidy pages.
   - Product specs.
   - Meter data or annual statements.

Preferred ChatGPT handoff format:

```markdown
# Warm 2502 handoff to Hermes

## Goal
...

## Corrected baseline
- monthly cost:
- electricity use:
- gas use:
- solar generation:
- contract/tariffs:

## Confirmed investments
| item | amount | source | confidence |

## Subsidies/reimbursements
| item | amount | timing | source | confidence |

## Future scenarios
...

## Known wrong old assumptions
...

## Open questions
...

## Attached/source files
...
```

Telegram note: tables may render poorly in Telegram, but they are fine inside Markdown files in GitHub.

### 11.5 How Hermes can continue after receiving ChatGPT input

After the user supplies the handoff, Hermes should:

1. Create the Warm 2502 GitHub structure.
2. Copy public-safe existing output into the new structure.
3. Capture source files or source summaries under `sources/`.
4. Create `data/assumptions.yaml` with confidence levels:
   - confirmed
   - user-provided
   - invoice-backed
   - source-backed
   - estimated
   - speculative
   - withdrawn
5. Create `contradiction-ledger.md` for old incorrect assumptions.
6. Create a reproducible cashflow model script.
7. Validate model inputs before producing any dashboard.
8. Generate a new dashboard only when numbers pass validation.
9. Mark old dashboard conclusions as superseded where needed.
10. Commit and push every state/documentation change.

### 11.6 Warm 2502 publish gate

Do not publish a new public dashboard unless:

- current monthly baseline is known;
- investment amounts are source-backed or clearly labeled as estimates;
- subsidy treatment is explicit;
- later-year assumptions are labeled as scenario assumptions, not facts;
- old wrong conclusions are explicitly withdrawn or superseded;
- dashboard includes confidence/uncertainty section.

### 11.7 Warm 2502 meta-agent questions

The Warm 2502 meta-agent should ask after each calculation/documentation run:

- Which inputs changed?
- Which source proves the change?
- Did the outcome change materially?
- Did any old conclusion need withdrawal?
- Are subsidies and later-year support assumptions still uncertain?
- Did the model separate confirmed cashflow from speculative scenarios?
- Is the public dashboard safe to share?

---

## 12. Implementation phases — design only

### Phase A — Prepare meta state for Truviq

Actions after approval:

- Create `output/truviq/_meta-agent/` files.
- Do one manual meta-evaluation of the latest Truviq run.
- Commit/push ledgers.
- No cron changes yet.

### Phase B — Create Truviq meta-agent cronjob

Requires explicit approval.

Proposed schedule:

```cron
50 7 * * 1,3,5
```

Proposed model:

- cheaper model by default;
- GPT-5.5 only when evaluating high-stakes publish decision or prompt rewrite.

### Phase C — Extend pattern to Hamer-Kuling

Requires separate approval.

- Create `output/family/_meta-agent/`.
- Add proof-goal evaluation rules.
- Evaluate one manual run before scheduling.

### Phase D — Warm 2502 transfer

Requires user input first.

- Ask user for ChatGPT handoff/source bundle.
- Create structure.
- Rebuild assumptions and model.
- Only publish after validation.

---

## 13. Risk register

### Risk 1 — Meta-agent becomes another noisy cronjob

Mitigation:

- Default to GitHub ledger only.
- Telegram only for approval-needed or useful findings.
- Use `[SILENT]` for no-change outcomes where supported.

### Risk 2 — Meta-agent silently changes production jobs

Mitigation:

- Prompt hard rule: no cron mutation.
- Approval queue required for every cron/model/provider/prompt change.
- GitHub decision-ledger records all proposed changes.

### Risk 3 — Token overhead cancels savings

Mitigation:

- Meta-agent reads small state files only.
- No broad web research.
- Token budget target 5k–20k excluding cache.
- Weekly aggregation if per-run review proves too expensive.

### Risk 4 — False sense of quality

Mitigation:

- Require evidence, novelty and repetition scores separately.
- Require critical questions and demolish check.
- Record weak-source and no-result findings.

### Risk 5 — Warm 2502 repeats earlier financial mistakes

Mitigation:

- No dashboard publication until assumptions are source-classified.
- Contradiction ledger for old wrong figures.
- Validation script before dashboard generation.
- Explicit uncertainty labels for subsidy/later-year support.

### Risk 6 — Public/private leakage

Mitigation:

- Keep invoices, addresses, contracts and personal meter data local/private unless approved.
- Public GitHub receives sanitized assumptions and outputs only.
- Secret/PII scan before commit if source files are included.

---

## 14. First approvals needed before any execution

No approval is needed for this design document; it is documentation only.

Before implementation, ask approval for each separate action:

1. Create Truviq `_meta-agent/` files.
2. Run one manual meta-evaluation on latest Truviq run.
3. Create a recurring Truviq meta-agent cronjob.
4. Choose model/provider for meta-agent.
5. Extend same pattern to Hamer-Kuling.
6. Create Warm 2502 GitHub folder structure.
7. Move/copy any Warm 2502 local files into GitHub.
8. Publish any new Warm 2502 public dashboard.

---

## 15. Recommended immediate next step

Recommended next manual action after approval:

> Create the Truviq `_meta-agent/` directory and run exactly one manual meta-evaluation against the completed `ad941c94e9ed` run from 2026-06-05.

This is low-risk because it only writes evaluation ledgers and does not change cronjobs.
