# Truviq HTML dashboard implementation report

**Date:** 2026-06-05  
**Status:** implemented and ready for public GitHub Pages sharing  
**Cron changes:** none

## Implemented artifact

- HTML file: `/home/hermes/work/output/truviq/public/truviq-health-market-dashboard.html`
- Intended public URL after push: `https://jhw367.github.io/output/truviq/public/truviq-health-market-dashboard.html`
- Format: one standalone static HTML file.
- External login: none.
- External JavaScript dependencies: none.
- Sensitive material: no API keys, tokens, internal auth files, private file paths, or provider credentials included.

## Source files used

The dashboard is generated from the existing Truviq GitHub-state files:

- `truviq/_agent-state/run-ledger.md`
- `truviq/_agent-state/hypotheses.md`
- `truviq/_agent-state/source-ledger.md`
- `truviq/_agent-state/changed-since-last-publish.md`
- `truviq/_agent-state/publish-gate.md`

## Dashboard sections

The static dashboard contains:

- title and latest update
- executive summary
- newest findings
- active hypotheses
- new/recent sources
- changes since previous publication
- recommended actions
- publication history
- clear disclaimer that the page is a work document / intelligence-in-progress

## Design choices

- Mobile-first, dark-theme dashboard suitable for sharing with Soenil.
- One-file static HTML so non-GitHub users only need one public link.
- No hidden backend or login dependency.
- Source provenance shown in footer.
- Public intelligence language is deliberately cautious: hypotheses are not presented as confirmed sales opportunities.

## Provider status at implementation time

Observed via `hermes status --all` and logs at `2026-06-05T14:42:55Z`:

- Active model: `gpt-5.5`
- Active provider: `openai-codex` / OpenAI Codex
- Auth status: OpenAI Codex logged in
- Gateway: running
- Telegram: configured
- OpenRouter API key present, but current active provider is OpenAI Codex

## Latest rate-limit / quota-related errors

Most relevant provider error found in Hermes logs:

- Time: `2026-06-05 11:27:44 UTC`
- Provider/model: `openai-codex` / `gpt-5.5`
- Error: `HTTP 429: The usage limit has been reached`
- Log summary: API call failed after 3 retries, approx. `115,027` context tokens
- Earlier similar errors occurred around `2026-06-05 11:23 UTC`

Other quota-related warning:

- Time: `2026-06-05 09:01 UTC`
- Component: skills hub / GitHub unauthenticated API
- Error: unauthenticated GitHub API rate limit exhausted, 60 requests/hour
- Impact on this task: none; git SSH push remains available.

Provider status after those errors:

- Later OpenAI Codex calls in this session succeeded around `2026-06-05 14:42 UTC`.
- Interpretation: provider is currently usable, but the morning quota/rate-limit event means cron jobs should avoid dense retries and large overlapping runs.

## Safe execution-frequency recommendation

### Truviq

Recommended safe frequency:

- **2–3 runs per week maximum** while using `gpt-5.5` / OpenAI Codex.
- Keep the current hypothesis-driven pattern: exactly one hypothesis or one narrow follow-up per run.
- Avoid same-day manual reruns unless there is a concrete new source or user request.
- If a 429 / usage-limit error occurs: do not retry-loop; record the error, stop, and wait until the provider reset window or next scheduled day.

Reasoning:

- Recent clean Truviq runs can exceed 100k context tokens.
- A dense collector/deep-run pattern already proved unsafe and was paused.
- Current state-ledger pattern reduces repetition, but `gpt-5.5` quota remains the limiting factor.

### Hamer-Kuling

Recommended safe frequency:

- **1–2 runs per week maximum** for deep genealogical research.
- Use short targeted runs only: one hypothesis branch, one archive/person/place cluster, or one source family per run.
- Avoid daily deep runs unless using a cheaper/provider-safe model and strict token caps.
- If no new official or network evidence is found, skip publication and only append a compact ledger note.

Reasoning:

- Genealogical research tends to expand through networks and source chains, which can become token-heavy.
- The project benefits more from careful auditability than from high-frequency repetition.
- Current active daily schedule is probably too aggressive under the observed provider quota behavior.

## No cron changes made

This implementation did not:

- create a cronjob
- edit any cronjob schedule
- pause or resume any cronjob
- change model/provider settings
- alter prompts

Any automation of this dashboard should be approved separately before modifying the Truviq cron prompt or schedule.
