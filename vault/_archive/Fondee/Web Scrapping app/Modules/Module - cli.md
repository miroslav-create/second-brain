---
type: module
tags:
  - module/core
aliases:
  - fund_snoop.cli
  - cli.py
  - python -m fund_snoop
---

# Module — `fund_snoop.cli`

> File: `fund_snoop/cli.py`. Argparse entry for `python -m fund_snoop`. Developer-facing trigger; non-tech users go through [[Module - web app]].

## Purpose

Tiny wrapper around [[Module - runner|run()]] + [[Module - render report|write_report()]]. Adds one flag, prints output paths.

## Public surface

- `main(argv: list[str] | None = None) -> int` — parses args, runs, prints, returns exit code
- Module entry: `python -m fund_snoop` → invokes `__main__.py` → `main()`

Flags:
- *(default)* — full scrape via [[Module - runner|run()]] then render via [[Module - render report|write_report()]]
- `--render-only` — skip scrape, load `latest_snapshot()`, render. Exits 1 with "no snapshot found" message if `data/snapshots/` is empty.

Always prints:
```
[render] přehled: <path-to-index.html>
[render] produkty: <path-to-products.html>
[render] archiv: <summary_archive>, <products_archive>
```

## Depends on

- [[Module - runner]] — `run()`, `latest_snapshot()`
- [[Module - render report]] — `write_report()`
- stdlib `argparse`, `sys`

## Used by

- Developers running scrapes locally
- CI / smoke tests (not currently wired)

## Domain concepts handled

[[Snapshot]] (via runner) · [[Report]] (via renderer)

## Relevant decisions

- [[ADR-0001 on-demand only]] — `--render-only` enables re-rendering without re-scraping

## Cross-links

- [[Tech stack]] § Run
- [[Pickup checklist]] § Run cheats
- [[About]] § How it works

## Live state

Source file is short. `HANDOVER.md` § "How to run → CLI" mirrors it.

## Source of truth

`fund_snoop/cli.py` (repo).
