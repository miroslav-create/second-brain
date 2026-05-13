---
type: module
tags:
  - module/core
aliases:
  - fund_snoop.paths
  - paths.py
---

# Module — `fund_snoop.paths`

> File: `fund_snoop/paths.py`. Filesystem base-dir resolver. PyInstaller-aware.

## Purpose

Resolves writable output directories ([[Snapshot]] JSON archive, [[Report]] HTML output). Handles the difference between dev mode (paths anchored to repo root) and frozen / PyInstaller mode (paths next to the `.exe`).

## Public surface

- `is_frozen() -> bool` — detects PyInstaller bundle
- `base_dir() -> Path` — repo root in dev, `Path(sys.executable).parent` when frozen
- `data_dir() -> Path` — `<base>/data`
- `snapshot_dir() -> Path` — `<base>/data/snapshots`
- `reports_dir() -> Path` — `<base>/reports`
- `archive_dir() -> Path` — `<base>/reports/archive`

## Depends on

- stdlib only (`sys`, `pathlib`)

## Used by

- [[Module - runner]] — `_persist()` writes to `snapshot_dir()`; `latest_snapshot()` reads from it
- [[Module - render report]] — `write_report` writes to `reports_dir()` + `archive_dir()`
- [[Module - web app]] — `REPORTS_DIR = reports_dir()` mount + file serving

## Domain concepts handled

[[Snapshot]] (where it lives) · [[Report]] (where it lives)

## Relevant decisions

- [[ADR-0001 on-demand only]] — outputs are durable artifacts, not transient state
- [[Pickup checklist]] § PyInstaller bundle — load-bearing for the frozen-mode branch

## Cross-links

- [[Tech stack]] § Output
- [[Mind map]] § Data flow

## Live state

Source file is short and stable. See `HANDOVER.md` § Key files.

## Source of truth

`fund_snoop/paths.py` (repo).
