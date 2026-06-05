# Truviq public dashboard design

**Date:** 2026-06-05  
**Status:** implemented as static HTML; no cron changes made  
**Audience:** Soenil and other non-GitHub users who need one readable link.

## 1. Goal

Create one public HTML dashboard that is safe to share without requiring GitHub knowledge.

The dashboard must show:

- latest findings;
- active hypotheses;
- new sources;
- publication history;
- last update;
- what the findings mean for Truviq action.

The dashboard is generated from the existing GitHub state files, not from memory or chat history.

## 2. Public artifact

Recommended stable public URL after GitHub Pages update:

```text
https://jhw367.github.io/output/truviq/public-dashboard.html
```

Repository path:

```text
output/truviq/public-dashboard.html
```

This gives Soenil a single link and avoids asking him to browse GitHub folders, commits or raw files.

## 3. Source files

The dashboard uses only these existing state files:

```text
output/truviq/_agent-state/run-ledger.md
output/truviq/_agent-state/hypotheses.md
output/truviq/_agent-state/source-ledger.md
output/truviq/_agent-state/changed-since-last-publish.md
output/truviq/_agent-state/publish-gate.md
```

Optional later source when implemented:

```text
output/truviq/_meta-agent/evaluation-ledger.md
```

## 4. Information architecture

### 4.1 Header

- Title: `Truviq Health Market Intelligence`
- Subtitle: short explanation in plain English/Dutch-neutral business language.
- Last updated timestamp.
- Source statement: generated from state ledgers.

### 4.2 Status cards

Four compact cards:

1. Latest run
2. Active hypotheses
3. New/used sources
4. Publication gate status

### 4.3 Latest findings

Derived from `changed-since-last-publish.md` and latest `run-ledger.md` entries.

Purpose: answer “what is new?” without exposing state-file mechanics.

### 4.4 Active hypotheses

Derived from `hypotheses.md`.

For each hypothesis:

- ID/title
- status
- account type
- current evidence summary
- next best test

### 4.5 New sources

Derived from latest dated sections in `source-ledger.md`.

Show:

- date
- source domain/title by URL
- affected hypothesis
- short note

### 4.6 Publication history

Derived from `publish-gate.md` and `run-ledger.md`.

Show:

- whether public output changed;
- why the gate was met/not met;
- what decision changed.

### 4.7 Footer / usage notes

- No credentials or private notes.
- Hypotheses are not confirmed sales opportunities unless explicitly marked.
- Dashboard is automatically refreshable by the Truviq research agent after future runs.

## 5. Update model

The Truviq research agent should regenerate `output/truviq/public-dashboard.html` after each completed run from the state files.

No separate database is needed.

Recommended update rules for the research agent prompt:

```text
After updating _agent-state files, regenerate /home/hermes/work/output/truviq/public-dashboard.html from:
- _agent-state/run-ledger.md
- _agent-state/hypotheses.md
- _agent-state/source-ledger.md
- _agent-state/changed-since-last-publish.md
- _agent-state/publish-gate.md

Do this whenever any of these state files changed.
Do not change cronjobs.
Commit and push if the dashboard changed.
Verify the GitHub Pages or raw URL where practical.
```

This prompt change is **not applied yet**; it needs explicit approval if added to a cronjob.

## 6. Design direction

Use a dark, precise dashboard style inspired by Linear:

- near-black background;
- high-contrast typography;
- subtle cards and borders;
- minimal decoration;
- status chips;
- mobile-first responsive layout;
- visible source provenance.

Reason: Truviq content is technical/commercial intelligence. The design should feel like an executive research cockpit, not a marketing flyer.

## 7. Safety and privacy rules

- Do not include credentials, tokens, internal chat logs or private comments.
- Link only to public source URLs already present in `source-ledger.md`.
- Label hypotheses clearly as hypotheses.
- Avoid saying “buyer confirmed” unless the state file explicitly proves it.
- Keep public content meeting-first and action-oriented.

## 8. Acceptance criteria

- One HTML dashboard file exists at `output/truviq/public-dashboard.html`.
- Dashboard renders without external data files.
- Dashboard can be opened by Soenil without GitHub knowledge.
- It includes newest findings, active hypotheses, new sources, publication history and last update.
- It is generated from existing state files.
- No cronjobs are changed.
- Files are committed and pushed to GitHub.
