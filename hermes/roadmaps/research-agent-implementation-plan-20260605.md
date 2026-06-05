# Research-agent implementation plan — Truviq + Hamer-Kuling

**Date:** 2026-06-05  
**Source redesign:** `/home/hermes/work/output/hermes/roadmaps/research-agent-redesign-20260605.md`  
**Output file:** `/home/hermes/work/output/hermes/roadmaps/research-agent-implementation-plan-20260605.md`  
**Important:** this is a plan only. No cron jobs are changed by this document.

## 0. Plain-language goal

Move the Truviq and Hamer-Kuling research jobs from “frequent broad research + repeated publishing” to “one narrow research question per run + GitHub as the notebook”.

The migration must avoid:

- data loss;
- duplicate cron jobs;
- repeated Telegram error spam when model quota/API errors happen;
- expensive default-model surprises;
- public-page rewrites when nothing actually changed.

## 1. Current jobs to change, pause, replace or keep

### A. Truviq jobs

#### `d702de473292` — Truviq health marketing plan NL/EU — 15-minute lightweight collector

- **Current status:** paused.
- **Current schedule:** `7,22,37,52 * * * *` — 4× per hour.
- **Current model/provider:** inherited default.
- **Current skills:** `b2b-prospect-research`, `llm-wiki`, `user-facing-deliverables`, `output-master-review`.
- **Current workdir:** `/home/hermes/work/truviq/markt-health`.
- **Current issue:** historically highest token use and high repetition risk.
- **Migration action:** keep paused, do not resume. Replace conceptually with the new single Truviq hypothesis-driven job.
- **Later cleanup:** remove only after the new job has run successfully at least 3 times and the user approves deletion.

#### `461376c11da5` — Truviq health marketing plan NL/EU — hourly deep publish

- **Current status:** paused.
- **Current schedule:** `3 * * * *` — hourly.
- **Current model/provider:** inherited default.
- **Current skills:** `b2b-prospect-research`, `llm-wiki`, `user-facing-deliverables`, `output-master-review`.
- **Current workdir:** `/home/hermes/work/truviq/markt-health`.
- **Current issue:** too frequent for deep synthesis; can repeat public-page updates.
- **Migration action:** keep paused. Replace with the new single Truviq hypothesis-driven job.
- **Later cleanup:** remove only after the new job has run successfully at least 3 times and the user approves deletion.

### B. Hamer-Kuling job

#### `1edd5a1ca92a` — Stamboom Hamer-Kuling research update — update English HTML

- **Current status:** scheduled/active.
- **Current schedule:** `15 5 * * *` — daily 05:15 UTC.
- **Current model/provider:** inherited default.
- **Current skills:** `genealogical-research`, `user-facing-deliverables`, `output-master-review`.
- **Current issue:** good domain fit, but prompt should be narrowed to one proof goal per run and should use GitHub state files first.
- **Migration action:** update this existing job in place; do not create a duplicate Hamer-Kuling cronjob.
- **Important safety step:** pause the job before updating the prompt, update it while paused, then resume only after manual approval.

### C. Jobs outside scope but relevant to spam/overlap

#### `7e2f365b873a` — Daily Hermes usage watchdog

- **Current status:** scheduled/active.
- **Current schedule:** `0 8 * * *`.
- **Migration action:** keep as-is. It is script-only and zero-token.

#### `b4e59c4b9943` — Daily usage summary

- **Current status:** paused.
- **Migration action:** keep paused. Do not resume, because it overlaps with the script-only watchdog.

## 2. New job structure

### Structure after migration

There should be exactly two research cronjobs for these domains:

1. **Truviq health-market hypothesis research** — new job.
2. **Hamer-Kuling genealogy proof-goal research** — updated existing job `1edd5a1ca92a`.

Old Truviq jobs remain paused during canary period:

- `d702de473292` — paused, not deleted immediately.
- `461376c11da5` — paused, not deleted immediately.

This avoids duplicate jobs and gives rollback safety.

### GitHub persistent-memory folders

The jobs should use these folders as their notebook/state:

```text
/home/hermes/work/output/truviq/_agent-state/
/home/hermes/work/output/family/_agent-state/
```

Recommended files inside each folder:

```text
README.md
run-ledger.md
source-ledger.md
negative-searches.md
changed-since-last-publish.md
publish-gate.md
```

Extra domain-specific files:

```text
/home/hermes/work/output/truviq/_agent-state/hypotheses.md
/home/hermes/work/output/family/_agent-state/proof-goals.md
/home/hermes/work/output/family/_agent-state/privacy-rules.md
```

### State initialization job

Do not let the recurring jobs create state blindly on their first live run.

First, run a one-time manual initialization step after approval:

- create `_agent-state/` folders;
- seed compact state from existing public outputs;
- commit and push state files to GitHub;
- verify `git status` is clean.

This prevents the first scheduled run from spending many tokens reconstructing memory.

## 3. Exact new prompts

### 3.1 New Truviq prompt

Use this exact prompt for the new Truviq job.

```text
You are Hermes Agent running the redesigned Truviq health-market research agent.

Primary goal:
Maximize net-new, actionable information for Truviq’s NL/EU health-market sales motion while minimizing repetition and token use.

Persistent memory rule:
GitHub is the durable memory. Do not rely on chat memory. At the start of every run, read only these small state files first:
- /home/hermes/work/output/truviq/_agent-state/README.md
- /home/hermes/work/output/truviq/_agent-state/run-ledger.md
- /home/hermes/work/output/truviq/_agent-state/hypotheses.md
- /home/hermes/work/output/truviq/_agent-state/source-ledger.md
- /home/hermes/work/output/truviq/_agent-state/negative-searches.md
- /home/hermes/work/output/truviq/_agent-state/changed-since-last-publish.md
- /home/hermes/work/output/truviq/_agent-state/publish-gate.md

If any state file is missing:
- Do not start broad research.
- Create or repair only the missing compact state file from existing local GitHub output files.
- Commit and push the state repair.
- End the run with a short status note.

Existing public outputs, read only when needed:
- /home/hermes/work/output/truviq/health-market-analysis.html
- /home/hermes/work/output/truviq/sales-people-health-opportunity.html
- /home/hermes/work/output/truviq/hlth-europe-2026-first-deal-sprint-plan.md
- /home/hermes/work/output/truviq/truviq-health-match-readout.md

Run discipline:
1. Select exactly ONE active hypothesis from hypotheses.md, unless it is blocked within 10 minutes; then select one fallback hypothesis.
2. Before searching, check source-ledger.md and negative-searches.md. Do not repeat old broad searches unless the source changed or the query is materially narrower.
3. Form a testable research question in this structure:
   - Hypothesis ID:
   - What would confirm it:
   - What would weaken/reject it:
   - Source(s) to check now:
4. Use a source budget: maximum 3 new public sources per run. Prefer official sources, procurement/tender pages, programme documents, insurer/provider pages and event/partner pages. Avoid broad web crawling.
5. Stop external lookups after repeated HTTP 403, 429, timeout or quota errors. Log the blocker in source-ledger.md or run-ledger.md. Do not retry in a loop.
6. Extract only decision-useful facts: programme owner, procurement signal, funding route, workflow pain, buyer persona, timing, disqualifier, outreach next step.
7. Add every checked source to source-ledger.md with date, URL, result and which hypothesis it affects.
8. Add every no-result or rejected query to negative-searches.md so it is not repeated.
9. Update hypotheses.md: status must be one of active / strengthened / weakened / rejected / promoted-to-action.
10. Update changed-since-last-publish.md with only real deltas. Do not rewrite public HTML for cosmetic changes.
11. Run a demolish check: why this lead/hypothesis may be wrong, what evidence is weak, what would disconfirm it.

Publish gate:
Only update public GitHub output files when at least one is true:
- a direct-buyer account changes priority tier;
- a new official source confirms funding/procurement/timing;
- a meeting script or next action materially changes;
- a previous claim must be corrected or downgraded;
- the user explicitly asks for publication.

If publishing is triggered:
- update the relevant Markdown/HTML under /home/hermes/work/output/truviq/;
- keep the public output concise, sourced, and meeting-first;
- commit and push to origin main;
- verify the GitHub/raw URL where practical.

Telegram / delivery rule:
Use short Dutch bullets only when there is a material update, a publication, or an action needed from J H. If there is no material update, produce a compact final line beginning with [NO MATERIAL UPDATE]. If there is a model/provider/quota/API error, stop after one concise error explanation and do not retry repeatedly.

Output format:
## Truviq research run
- Hypothese getest: [ID + titel]
- Nieuwe informatie: [1–3 bullets]
- Beslisimpact: [prioriteit/actie gewijzigd of geen publicatie]
- Bestanden bijgewerkt: [state files; public files only if changed]
- Volgende beste test: [één concrete bron/query]
- Token-discipline: [aantal bronnen; publicatie ja/nee]
```

### 3.2 New Hamer-Kuling prompt

Use this exact prompt to update existing job `1edd5a1ca92a`.

```text
You are Hermes Agent running the redesigned Hamer-Kuling genealogy research agent.

Primary goal:
Maximize new, source-backed genealogical information per run while minimizing repeated searches and preserving privacy.

Persistent memory rule:
GitHub and the local family dossier are the durable memory. Do not rely on chat memory. At the start of every run, read only the compact state files first:
- /home/hermes/work/output/family/_agent-state/README.md
- /home/hermes/work/output/family/_agent-state/run-ledger.md
- /home/hermes/work/output/family/_agent-state/proof-goals.md
- /home/hermes/work/output/family/_agent-state/source-ledger.md
- /home/hermes/work/output/family/_agent-state/negative-searches.md
- /home/hermes/work/output/family/_agent-state/changed-since-last-publish.md
- /home/hermes/work/output/family/_agent-state/privacy-rules.md

If any state file is missing:
- Do not start broad research.
- Create or repair only the missing compact state file from existing local dossier/output files.
- Commit and push the state repair if it is under /home/hermes/work/output.
- End the run with a short status note.

Then read only the necessary private dossier files for the selected proof goal:
- /home/hermes/work/familie/bronnen/source_register.csv
- /home/hermes/work/familie/onderzoek/evidence_matrix.md
- /home/hermes/work/familie/onderzoek/onderzoekslog.md
- /home/hermes/work/familie/personen/stamboom_hamer_kuling.md

Existing public outputs, read only when needed:
- /home/hermes/work/output/family/hamer-kuling-family-research.html
- /home/hermes/work/output/family/hamer-kuling-family-overview-20260526.html

Privacy rule:
Recent people, children, addresses and possibly living persons must not be expanded in public output. Keep public wording restricted. Do not publish raw private dossier paths, raw private notes, or living-person details in public HTML.

Run discipline:
1. Select exactly ONE proof goal from proof-goals.md. If blocked quickly, select one fallback proof goal.
2. Before searching, check source-ledger.md and negative-searches.md. Do not repeat a broad query already logged as NO_RESULT/REJECTED unless the repository, index, spelling variant or date range changed materially.
3. Express the proof goal as:
   - Claim / hypothesis:
   - Current status: A / B / B+ / C / D / X
   - What would confirm it:
   - What would weaken/reject it:
   - Exact source/query to test now:
4. Use a source budget: maximum 3 source checks per run. Prefer official civil records, archive indexes/scans, population/family cards, Delpher family notices, OGS/National Archives and relationship-network evidence.
5. Stop external lookups after repeated HTTP 403, 429, timeout or quota errors. Log the blocker and do not retry in a loop.
6. Capture relationship-network clues when official records are missing: witnesses, in-laws, addresses, occupations, places, ship/event context, newspaper notices and family advertisements. Label these as B/C/D evidence, never as silent proof.
7. Log each query with a stable code: RUN -> QRY -> SRC -> IU -> CLM -> EVID. Negative searches must be logged as NO_RESULT or REJECTED.
8. Update proof-goals.md and source-ledger.md after every run. If a claim changes status, update changed-since-last-publish.md.
9. Update private dossier files only when evidence supports it. Do not promote hypotheses to fact without source-backed evidence.
10. Rebuild public HTML only if new evidence changes the public-safe story, source count, proof-goal status, or privacy-safe historical context.
11. If public output changes, copy the rebuilt HTML to /home/hermes/work/output/family/, commit and push to origin main, and verify the GitHub/raw URL where practical.

Priority proof goals:
- Confirm or explain why the 1958 Den Haag marriage HFM Hamer × YBM Kuling is not publicly visible.
- Confirm full names behind HFM/YBM through direct or strong relationship-network evidence.
- Strengthen or reject the HFM/Huib Hamer = Hubertus Ferdinandus Maria Hamer work identity.
- Strengthen or reject the YBM/Yvonne Kuling = Yvonne Beatrice Maria Kuling work identity.
- Expand Poelau Bras/B.P.M./Indies context only when it adds evidence to the family story, not as general history.

Telegram / delivery rule:
Use short Dutch bullets only when there is a material finding, a state/publication update, or an action needed from J H. If there is no material update, produce a compact final line beginning with [NO MATERIAL UPDATE]. If there is a model/provider/quota/API error, stop after one concise error explanation and do not retry repeatedly.

Output format:
## Hamer-Kuling research run
- Bewijsdoel: [ID + claim]
- Nieuwe informatie: [1–3 bullets]
- Statuswijziging: [A/B/B+/C/D/X of ongewijzigd]
- Bestanden bijgewerkt: [state/private/public]
- Herhaalde zoekacties vermeden: [aantal of voorbeelden]
- Volgende beste bewijstest: [één exacte bron/query]
- Privacy: public-safe / private-only
```

## 4. Exact schedules

### Truviq new job

Recommended normal schedule:

```cron
20 7 * * 1,3,5
```

Meaning in plain language: Monday, Wednesday and Friday at 07:20 UTC.

Optional event-sprint schedule, only after explicit user approval:

```cron
20 7 * * *
```

Meaning: daily at 07:20 UTC during a named sales/event sprint.

### Hamer-Kuling updated job

Keep current daily schedule:

```cron
15 5 * * *
```

Meaning: every day at 05:15 UTC.

Lower-cost alternative, only after explicit user approval:

```cron
15 5 * * 1,3,5
```

Meaning: Monday, Wednesday and Friday at 05:15 UTC.

## 5. Model/provider and no-fallback policy

### Recommended policy

- Routine recurring research should use a cheaper model, preferably **DeepSeek via OpenRouter** if available and configured.
- **GPT-5.5** should be used only for complex synthesis/escalation.
- Do **not** rely on inherited default if the default is expensive or points to OpenAI/Codex.
- Do **not** configure automatic fallback to OpenAI/Codex.

### Implementation note

Before changing jobs, verify the exact local model identifier for DeepSeek via OpenRouter. Do not guess if the local config uses a different model name.

Read-only checks before implementation:

```bash
hermes config
hermes model --help
```

If the local verified model is `deepseek/deepseek-chat-v3-0324` via provider `openrouter`, use that as the per-job model override. If not verified, keep the jobs paused and ask for confirmation.

## 6. Anti-spam and quota-error design

### Problem

The old jobs show quota/API errors such as HTTP 403 key-limit exceeded. If a recurring job keeps running, it can repeatedly send Telegram error messages.

### Prevention plan

1. **Keep old Truviq jobs paused.** Do not resume them.
2. **Pause Hamer-Kuling before editing.** Resume only after state files and model/provider are verified.
3. **Create the new Truviq job paused first**, not active immediately.
4. **Manual canary run before scheduling live delivery.** Run once only after approval and check output.
5. **Prompt-level stop rule:** both prompts tell the agent to stop after quota/API errors and avoid retry loops.
6. **Source budget:** max 3 sources per run, preventing bursty web/API behavior.
7. **No duplicate jobs:** do not create a second Hamer-Kuling job; update existing job `1edd5a1ca92a` in place.
8. **Only one Telegram final per run:** no mid-run messaging; final output only.

### Optional stronger anti-spam control

If Hermes supports it in this local installation, first create/canary new jobs with `deliver=local` and switch to `origin` only after successful run. If not supported or not desired, keep jobs paused until manual run is approved.

## 7. Rollback plan

### Before any cron change

1. Save the current cron config:

```bash
cp /home/hermes/.hermes/cron/jobs.json /home/hermes/.hermes/cron/jobs.backup-20260605-pre-research-agent-migration.json
```

2. Commit current output repository state if dirty:

```bash
cd /home/hermes/work/output
git status --short --branch
```

If dirty, stop and review. Do not start cron migration on a dirty repo unless changes are understood.

### Rollback if new Truviq job misbehaves

1. Pause the new Truviq job.
2. Keep old Truviq jobs paused by default.
3. If the user explicitly wants the old behavior back, resume only one old Truviq job at a time:
   - Prefer `461376c11da5` hourly deep publish over `d702de473292` 15-minute collector.
   - Do not resume the 15-minute collector unless token/quota budget is confirmed.
4. Restore previous cron config from backup only if the cron tool state becomes inconsistent.

### Rollback if Hamer-Kuling update misbehaves

1. Pause `1edd5a1ca92a`.
2. Restore prompt from `/home/hermes/.hermes/cron/jobs.backup-20260605-pre-research-agent-migration.json` or the audit report.
3. Resume only after a manual canary run succeeds.

### Rollback for GitHub state files

Because state files are committed to GitHub, rollback is normal git rollback:

```bash
cd /home/hermes/work/output
git log --oneline -- hermes/roadmaps truviq/_agent-state family/_agent-state
git revert <bad_commit_hash>
git push origin main
```

Use revert, not reset, so history is not destroyed.

## 8. Risks

### Risk 1 — duplicate jobs

- **Risk:** old Truviq jobs get resumed while the new Truviq job also runs.
- **Mitigation:** keep old jobs paused; create one new Truviq job; document old IDs; do not delete until canary period passes.

### Risk 2 — quota/API Telegram spam

- **Risk:** recurring jobs repeatedly fail and message Telegram.
- **Mitigation:** create new job paused first; canary manually; source budget; prompt stop rule; no automatic fallback.

### Risk 3 — data loss in cron config

- **Risk:** prompt updates overwrite old job config.
- **Mitigation:** backup `jobs.json` before any cron update; old prompts also preserved in `cronjob-audit-20260605-1027-utc.md`.

### Risk 4 — GitHub state becomes noisy

- **Risk:** state files grow into large logs and become token-heavy.
- **Mitigation:** keep run-ledger entries short; use source-ledger summaries; archive old entries quarterly if needed.

### Risk 5 — private genealogy data leaks into public output

- **Risk:** Hamer-Kuling agent reads private dossier and publishes too much.
- **Mitigation:** privacy-rules.md, prompt privacy rule, publish gate, public-safe wording only.

### Risk 6 — wrong model/provider identifier

- **Risk:** job is pinned to a non-working model name.
- **Mitigation:** verify local model/provider identifier before cron update; otherwise keep job paused.

### Risk 7 — first run spends too many tokens creating state

- **Risk:** recurring job uses a full context reconstruction on first run.
- **Mitigation:** initialize `_agent-state/` manually and commit before enabling recurring jobs.

## 9. Actions requiring manual approval first

No cron changes should be executed until J H explicitly approves each group below.

### Approval A — state initialization

Requires explicit approval before creating these files:

```text
/home/hermes/work/output/truviq/_agent-state/*
/home/hermes/work/output/family/_agent-state/*
```

This is a GitHub/output repo change, not a cron change.

### Approval B — cron backup

Requires explicit approval before copying:

```text
/home/hermes/.hermes/cron/jobs.json
```

to:

```text
/home/hermes/.hermes/cron/jobs.backup-20260605-pre-research-agent-migration.json
```

### Approval C — Hamer-Kuling job pause/update/resume

Requires explicit approval before any action on job:

```text
1edd5a1ca92a
```

Planned action sequence:

1. pause `1edd5a1ca92a`;
2. update prompt to the new Hamer-Kuling prompt;
3. keep schedule `15 5 * * *`;
4. pin cheaper DeepSeek/OpenRouter model only after exact local model ID is verified;
5. manual canary run;
6. resume after user approval.

### Approval D — new Truviq job creation

Requires explicit approval before creating the new job.

Planned new job:

- **Name:** `Truviq health-market hypothesis research — GitHub-state`
- **Schedule:** `20 7 * * 1,3,5`
- **Initial status:** paused if supported by workflow; otherwise create but do not resume until canary approval.
- **Skills:** `b2b-prospect-research`, `llm-wiki`, `user-facing-deliverables`, `output-master-review`
- **Workdir:** `/home/hermes/work/truviq/markt-health`
- **Toolsets:** `file`, `terminal`, `browser`
- **Model/provider:** DeepSeek via OpenRouter only after exact local model ID is verified.
- **Delivery:** origin/Telegram only after canary succeeds; local-only canary preferred if supported.

### Approval E — old Truviq job deletion

Requires separate explicit approval after canary period.

Do not delete these immediately:

```text
d702de473292
461376c11da5
```

Recommended deletion condition:

- new Truviq job has completed at least 3 successful runs;
- no duplicate output issue;
- GitHub state files are working;
- user confirms old jobs are no longer needed.

## 10. Concrete implementation sequence after approval

### Phase 1 — prepare state, no cron changes

1. Check git status in `/home/hermes/work/output`.
2. Create `_agent-state/` folders and seed files.
3. Commit and push state initialization.
4. Verify raw GitHub links return HTTP 200.

### Phase 2 — make cron safe

1. Backup `jobs.json`.
2. Verify DeepSeek/OpenRouter exact model identifier locally.
3. Confirm old Truviq jobs are still paused.

### Phase 3 — migrate Hamer-Kuling

1. Pause `1edd5a1ca92a`.
2. Update prompt only; keep schedule daily.
3. If exact cheaper model ID is verified, pin model/provider; otherwise leave paused and ask.
4. Run one manual canary.
5. Resume only after user approval.

### Phase 4 — create new Truviq job

1. Create new Truviq job with schedule `20 7 * * 1,3,5`.
2. Keep it paused until canary succeeds.
3. Run one manual canary.
4. Resume after user approval.

### Phase 5 — observe and clean up

1. After 3 successful Truviq runs, review outputs and token use.
2. If stable, ask user whether old Truviq jobs can be removed.
3. Do not remove old jobs without explicit approval.

## 11. Verification checklist

Before saying migration is complete:

- [ ] `jobs.json` backup exists.
- [ ] GitHub `_agent-state/` files exist and are committed.
- [ ] Old Truviq jobs remain paused.
- [ ] Only one Truviq replacement job exists.
- [ ] Hamer-Kuling uses the updated proof-goal prompt.
- [ ] Hamer-Kuling has no duplicate cronjob.
- [ ] New schedules are correct.
- [ ] No job uses inherited expensive default unintentionally.
- [ ] Canary run succeeded without quota-error spam.
- [ ] Git status is clean after push.

## 12. What this plan does not do

This plan does not execute cron changes. It does not create `_agent-state/` files. It does not pause, resume, create, update or delete any cronjob. Those actions require later explicit approval.
