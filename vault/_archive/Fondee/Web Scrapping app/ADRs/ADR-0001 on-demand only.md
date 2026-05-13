---
type: adr
tags:
  - adr/accepted
aliases:
  - ADR-0001
  - on-demand only
---

# ADR-0001 — On-demand only, not scheduled

> Status: **Accepted**. Repo: `docs/adr/0001-on-demand-only.md`.

## Context

Fee + product changes at competitors happen on a slow cadence (weeks to months). A daily/weekly scheduled job would generate noise and require a hosted runtime. Primary use case is "give me a fresh comparison" on demand from the operator, not continuous trend tracking.

## Decision

Fund Snoop runs only when the operator triggers it. No cron, no GitHub Action, no scheduler. Trigger surfaces: CLI (`python -m fund_snoop`) and the local FastAPI launcher (`start_app.bat`).

## Consequences

- **Positive:** zero hosting cost. No long-lived process. Snapshots emitted at known operator-meaningful moments. Static HTML output ships via OneDrive — no auth, no backend.
- **Trade-off:** lose the implicit historical changelog a daily job would build. Mitigation: every run archives a timestamped snapshot under `data/snapshots/`, so a diff view can be reconstructed later from accumulated history.

## Impacts

- Modules: [[Module - cli]], [[Module - web app]], [[Module - runner]].
- Concepts: [[Snapshot]] (one per operator-triggered run).
- Providers: all 22 — they run together per trigger.

## Cross-links

- [[About]] § Decisions
- [[Mind map]] § Triggers
- [[Tech stack]] § FastAPI rationale
- Related: [[ADR-0002 broker scope]], [[ADR-0003 product-centric taxonomy]], [[ADR-0004 summary as landing]]

## Source of truth

`docs/adr/0001-on-demand-only.md` (repo).
