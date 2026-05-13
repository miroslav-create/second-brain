---
type: concept
tags:
  - concept/domain
aliases:
  - snapshot
---

# Snapshot

> The output of one scrape run — all [[Provider|Providers]] and their [[Product|Products]] captured at a point in time. Persisted as JSON in `data/snapshots/`.

## Role in the domain

A Snapshot is the durable record of one operator-triggered run (see [[ADR-0001 on-demand only]]). Two consumers:

1. [[Module - render report]] reads it to produce the [[Report]] (Summary + Products HTML).
2. The archive at `data/snapshots/snapshot-YYYY-MM-DD.json` accumulates, enabling future diff-vs-previous views.

## Fields

- `captured_at` — UTC datetime when the run started
- `entries` — list of `ProviderEntry`, one per Provider scraped

## Relationships

- A Snapshot contains one Provider bundle per Provider scraped
- A [[Report]] renders the latest Snapshot
- Round-trips via `Snapshot.to_dict()` / `Snapshot.from_dict()` (legacy keys migrated)

## Used by

- [[Module - schema]] — `Snapshot` dataclass + `from_dict` migration path
- [[Module - runner]] — produces, persists
- [[Module - render report]] — consumes
- [[Module - paths]] — `snapshot_dir()` resolves location

## See also

- [[About]] § How it works
- [[Tech stack]] § Output
- [[Mind map]] § Data flow
- `CONTEXT.md` § Snapshot

## Source of truth

`CONTEXT.md` + `fund_snoop/schema.py` (repo).
