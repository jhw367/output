# Research-agent redesign — Truviq + Hamer-Kuling

**Date:** 2026-06-05  
**Output path:** `output/hermes/roadmaps/research-agent-redesign-20260605.md`  
**Purpose:** redesign the recurring Truviq and Hamer-Kuling research agents for more new information per run, less repetition, hypothesis-driven work, GitHub-backed persistent memory, and lower token use.

## 1. Context used

This redesign used the existing GitHub output repository as the persistent context layer.

### Truviq outputs reviewed

- `output/truviq/health-market-analysis.html`
- `output/truviq/sales-people-health-opportunity.html`
- `output/truviq/hlth-europe-2026-first-deal-sprint-plan.html`
- `output/truviq/hlth-europe-2026-first-deal-sprint-plan.md`
- `output/truviq/truviq-health-match-readout.html`
- `output/truviq/truviq-health-match-readout.md`
- `output/truviq/hlth-europe-2026-first-deal-sprint-plan-20260526.html`

Main existing Truviq knowledge:

- Strongest current angles: Dutch/EU health transformation workflows, AZWA/IZA funding routes, insurer-led transformation programmes, hybrid care, acute-care handoffs, identity/consent/eIDAS/EHDS, EHR-adjacent orchestration, NEN 7510, Camunda/FHIR/SMART-on-FHIR positioning.
- Commercial objective has shifted from generic “market research” to **meeting-first account qualification**: book 20-minute workflow-fit meetings with direct buyers, programme owners, insurer teams, implementation partners and ecosystem validators.
- Current public outputs already contain many mature lead hypotheses: Zorg bij jou/Santeon, Zilveren Kruis digital triage, VZVZ/Mitz/LSP/ZORG-ID, AZWA chain approaches, acute-care regions, Nictiz/eIDAS, PZP, Dutch hospitals with ED/NEED context, Sales-People as outreach partner.
- Problem: the old cron design kept revisiting and extending the same public pages frequently, creating repetition and high token use.

### Hamer-Kuling outputs reviewed

- `output/family/hamer-kuling-family-research.html`
- `output/family/hamer-kuling-family-overview-20260526.html`

Main existing Hamer-Kuling knowledge:

- Public-safe family research dashboard exists and is already audit-oriented.
- Source-backed facts include 1926 Hamer × Van den Oever marriage, 1942 Poelau Bras death context, 1895 Amsterdam birth/militia/B.P.M. leads, Kuling / De Groot context, Medan/Batavia/undertrouw leads, coded searches and negative-search logging.
- HFM/Huib Hamer and YBM/Yvonne Kuling are strong hypotheses/work identities, but the 1958 marriage confirmation remains a key proof goal.
- Existing public output is good, but the job should avoid repeatedly broad-searching the same names and should behave more like a proof-goal engine.

### Cron audit used

- `output/hermes/audits/cronjob-audit-20260605-1027-utc.md`

Historical token use from audit:

- Truviq 15-minute collector: **31,883,325 tokens incl. cache** / **3,128,935 without cache** across 358 runs.
- Truviq hourly deep publish: **20,610,957 tokens incl. cache** / **887,763 without cache** across 86 runs.
- Hamer-Kuling research: **21,575,830 tokens incl. cache** / **1,259,030 without cache** across 36 runs.

## 2. Diagnosis of current agents

### Truviq current design issue

The Truviq work was split into a very frequent 15-minute collector and an hourly deep-publish agent. That made sense while building momentum, but it now causes:

- repeated context loading of the same large HTML/MD files;
- many runs with tiny or no net-new information;
- public pages being touched too often;
- excessive cache-token totals;
- unclear handoff between collector facts, account hypotheses and final published output.

The current knowledge base is already rich enough that the agent should no longer “keep researching the market” broadly. It should now operate as a **hypothesis queue**:

1. pick one unresolved commercial hypothesis;
2. test it against fresh evidence;
3. update GitHub state files;
4. only publish when the evidence changes an account priority, outreach angle, or meeting ask.

### Hamer-Kuling current design issue

The Hamer-Kuling job is closer to the right shape because it already uses proof goals and coded audit trails. The main improvement is to make the run selection stricter:

- never repeat a query already logged as `NO_RESULT` unless the source or query conditions changed;
- separate “public HTML rebuild” from “new research found”; rebuild only when content changed;
- use GitHub-persisted proof-goal queue and negative-search register as the primary memory;
- choose exactly one proof goal per run unless the first goal is blocked quickly.

## 3. Target architecture: GitHub as persistent memory

Both agents should use the GitHub repository as their durable memory, not the model context.

### Shared memory pattern

Each research domain should maintain small, structured state files in GitHub:

```text
output/<domain>/_agent-state/
├── README.md                     # operating rules and public/private boundaries
├── run-ledger.md                 # append-only run summaries, one short entry per run
├── hypotheses.md                 # active hypotheses with status, evidence, next test
├── source-ledger.md              # source URLs/signatures already checked
├── negative-searches.md          # queries that should not be repeated blindly
├── changed-since-last-publish.md # small staging notes for next public update
└── publish-gate.md               # rules for when public HTML should be rebuilt
```

For Truviq, use:

```text
output/truviq/_agent-state/
```

For Hamer-Kuling, use:

```text
output/family/_agent-state/
```

Plain-language implication: the agent should not rely on remembering what happened in previous chats. It should read these small files from GitHub at the start of every run, update them, and only then decide whether a public page needs changing.

## 4. Redesigned Truviq agent

### Recommended frequency

Replace both old Truviq jobs with one lower-frequency research agent:

- **Standard:** 3× per week, e.g. Monday/Wednesday/Friday at 07:20 UTC.
- **Event sprint mode:** daily during the 4–6 weeks before HLTH Europe or another named sales event.
- **Do not run every 15 minutes.** That cadence is too expensive and mostly creates repeated output.

Recommended cron expression for standard mode:

```cron
20 7 * * 1,3,5
```

### Expected token use

Expected per run after redesign:

- **Cheap model / DeepSeek-class:** 15k–35k tokens incl. cache if using only state files and 1–3 source pages.
- **GPT-5.5-class:** 35k–80k tokens incl. cache for deeper synthesis or major publish decisions.

Expected monthly use:

- 12–14 standard runs/month × 15k–35k = **180k–490k tokens/month** on cheap model.
- Occasional GPT-5.5 escalation, 2–4 runs/month × 35k–80k = **70k–320k tokens/month**.

Compared with historical Truviq usage, this should cut token use by roughly **85–95%** while increasing useful new information per run.

### When to use GPT-5.5

Use GPT-5.5 only when at least one of these is true:

- ranking direct-buyer accounts for a real outreach decision;
- reconciling contradictory evidence;
- designing/refining meeting scripts for high-value accounts;
- deciding whether a public page should change materially;
- doing a demolish check before action.

Use a cheaper model for:

- state-file reading;
- source-ledger checks;
- minor account enrichment;
- negative-search logging;
- simple publish/no-publish decisions.

### New Truviq prompt

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

If these files do not exist yet, create them from the current public outputs in /home/hermes/work/output/truviq/ and keep them compact.

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
5. Extract only decision-useful facts: programme owner, procurement signal, funding route, workflow pain, buyer persona, timing, disqualifier, outreach next step.
6. Add every checked source to source-ledger.md with date, URL, result and which hypothesis it affects.
7. Add every no-result or rejected query to negative-searches.md so it is not repeated.
8. Update hypotheses.md: status must be one of active / strengthened / weakened / rejected / promoted-to-action.
9. Update changed-since-last-publish.md with only real deltas. Do not rewrite public HTML for cosmetic changes.
10. Run a demolish check: why this lead/hypothesis may be wrong, what evidence is weak, what would disconfirm it.

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

Output to Telegram:
Use short Dutch bullets:
## Truviq research run
- Hypothesis tested: [ID + title]
- New evidence: [1–3 bullets]
- Decision impact: [priority/action changed or no publish]
- Files updated: [state files; public files only if changed]
- Next best test: [one concrete next source/query]
- Token discipline: [sources checked count; public publish yes/no]
```

### Expected quality improvement

Expected improvement:

- fewer repeated “market overview” updates;
- more evidence per run because every run tests one hypothesis;
- cleaner distinction between direct buyers, ecosystem influencers and implementation partners;
- better sales usefulness because every run must end in a meeting/action implication;
- lower risk of hallucinated buyer claims because confirmation/disconfirmation criteria are explicit;
- GitHub becomes an inspectable research memory rather than a pile of final HTML pages.

## 5. Redesigned Hamer-Kuling genealogy agent

### Recommended frequency

Keep Hamer-Kuling active, but make it a disciplined proof-goal agent.

Recommended frequency:

- **Normal mode:** daily at 05:15 UTC is acceptable if each run is narrow and source-budgeted.
- **Lower-cost mode:** 3× per week if token budget becomes important.
- **Manual escalation:** GPT-5.5 only for difficult identity reconciliation, not for routine source lookup.

Recommended cron expression for daily mode:

```cron
15 5 * * *
```

Recommended cron expression for lower-cost mode:

```cron
15 5 * * 1,3,5
```

### Expected token use

Expected per run after redesign:

- **Cheap model / DeepSeek-class:** 12k–30k tokens incl. cache for one proof goal and 1–3 source checks.
- **GPT-5.5-class:** 30k–70k tokens incl. cache for complex identity merge/split or evidence-matrix reasoning.

Expected monthly use:

- Daily cheap-mode: 30 × 12k–30k = **360k–900k tokens/month**.
- 3× weekly cheap-mode: 12–14 × 12k–30k = **144k–420k tokens/month**.
- Occasional GPT-5.5 escalation: 1–3 × 30k–70k = **30k–210k tokens/month**.

Compared with historical Hamer-Kuling usage, expected reduction is roughly **50–80%**, mainly by avoiding repeated broad searches and only rebuilding public HTML when state changed.

### When to use GPT-5.5

Use GPT-5.5 only for:

- resolving HFM/Huib or YBM/Yvonne identity links from conflicting/indirect evidence;
- deciding whether a B/B+ hypothesis can be promoted or must remain restricted;
- interpreting multi-source relationship networks with witnesses, in-laws, places, occupations and newspapers;
- writing or revising the public narrative when privacy and uncertainty need careful framing.

Use a cheaper model for:

- reading registers;
- logging queries;
- extracting source facts;
- updating source ledgers;
- rebuilding HTML when content is deterministic.

### New genealogy prompt

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

Then read only the necessary private dossier files for the selected proof goal:
- /home/hermes/work/familie/bronnen/source_register.csv
- /home/hermes/work/familie/onderzoek/evidence_matrix.md
- /home/hermes/work/familie/onderzoek/onderzoekslog.md
- /home/hermes/work/familie/personen/stamboom_hamer_kuling.md

Existing public outputs, read only when needed:
- /home/hermes/work/output/family/hamer-kuling-family-research.html
- /home/hermes/work/output/family/hamer-kuling-family-overview-20260526.html

Privacy rule:
Recent people, children, addresses and possibly living persons must not be expanded in public output. Keep public wording restricted. Do not publish raw private dossier paths or private notes in public HTML.

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
5. Capture relationship-network clues when official records are missing: witnesses, in-laws, addresses, occupations, places, ship/event context, newspaper notices and family advertisements. Label these as B/C/D evidence, never as silent proof.
6. Log each query with a stable code: RUN -> QRY -> SRC -> IU -> CLM -> EVID. Negative searches must be logged as NO_RESULT or REJECTED.
7. Update proof-goals.md and source-ledger.md after every run. If a claim changes status, update changed-since-last-publish.md.
8. Update private dossier files only when evidence supports it. Do not promote hypotheses to fact without source-backed evidence.
9. Rebuild public HTML only if new evidence changes the public-safe story, source count, proof-goal status, or privacy-safe historical context.
10. If public output changes, copy the rebuilt HTML to /home/hermes/work/output/family/, commit and push to origin main, and verify the GitHub/raw URL where practical.

Priority proof goals:
- Confirm or explain why the 1958 Den Haag marriage HFM Hamer × YBM Kuling is not publicly visible.
- Confirm full names behind HFM/YBM through direct or strong relationship-network evidence.
- Strengthen or reject the HFM/Huib Hamer = Hubertus Ferdinandus Maria Hamer work identity.
- Strengthen or reject the YBM/Yvonne Kuling = Yvonne Beatrice Maria Kuling work identity.
- Expand Poelau Bras/B.P.M./Indies context only when it adds evidence to the family story, not as general history.

Output to Telegram:
Use short Dutch bullets:
## Hamer-Kuling research run
- Proof goal: [ID + claim]
- New evidence: [1–3 bullets]
- Status change: [A/B/B+/C/D/X or unchanged]
- Files updated: [state/private/public]
- Repeated searches avoided: [count or examples]
- Next best proof test: [one exact source/query]
- Privacy: public-safe / private-only
```

### Expected quality improvement

Expected improvement:

- each run has a clear proof purpose;
- fewer repeated Delpher/OpenArchieven/Wiewaswie-style broad searches;
- negative results become useful memory instead of wasted repetition;
- stronger distinction between fact, strong indication, hypothesis and rejected lead;
- public page changes only when the story actually advances;
- better privacy control because publication is a gate, not the default action.

## 6. Implementation roadmap

### Phase 1 — create compact state files

Create state folders:

```text
output/truviq/_agent-state/
output/family/_agent-state/
```

Seed them from current public outputs:

- current hypotheses;
- source list;
- negative searches if already present in dashboards;
- current publish gate;
- one run-ledger entry saying “state initialized from existing GitHub outputs”.

### Phase 2 — replace cron prompts

- Replace Truviq collector + hourly deep prompt with the redesigned Truviq prompt.
- Keep both old Truviq jobs paused until user explicitly approves replacement.
- Update Hamer-Kuling prompt to the redesigned proof-goal prompt.
- Preserve current Hamer-Kuling daily schedule unless token budget requires 3× weekly.

### Phase 3 — add model discipline

Recommended model policy:

- Default recurring runs: cheap model, preferably DeepSeek via OpenRouter if available and configured.
- GPT-5.5 only for explicit escalation conditions.
- No automatic fallback to expensive OpenAI/Codex for routine runs.

Important practical note: current cron jobs with `model/provider = inherited default` follow the live Hermes default. If the default is expensive, recurring jobs become expensive unless each job is pinned to a cheaper model/provider.

### Phase 4 — publish only on meaningful deltas

Add a `publish-gate.md` file per domain. Public HTML/MD should only change when a material finding appears.

This is the most important anti-repetition measure.

## 7. Summary recommendation

### Truviq

- Replace frequent collector/deep-publish structure with one hypothesis-driven agent.
- Run 3× per week in normal mode; daily only around named event sprints.
- Use GitHub state files as the operational memory.
- Publish only when account priority, meeting ask, source evidence or correction materially changes.
- Expected result: **85–95% lower token use** and more actionable sales intelligence per run.

### Hamer-Kuling

- Keep as a proof-goal agent, not a broad search agent.
- Daily is acceptable if source-budgeted; 3× weekly if cost control is priority.
- Use GitHub state files plus private dossier as durable memory.
- Rebuild public HTML only on meaningful proof/story/status changes.
- Expected result: **50–80% lower token use**, better auditability and clearer hypothesis progression.

## 8. Plain-language bottom line

The old agents behaved too much like “keep looking and keep publishing”.  
The redesigned agents should behave like researchers with a lab notebook:

1. choose one question;
2. check whether it was already tried;
3. test it against a small number of strong sources;
4. write down what changed;
5. only update the public page when the answer actually moved forward.
