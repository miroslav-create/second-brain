---
type: module
tags:
  - module/core
aliases:
  - fund_snoop.runner
  - runner.py
---

# Module — `fund_snoop.runner`

> File: `fund_snoop/runner.py`. Orchestrator. Opens Chromium once, iterates all 22 scrapers, persists a [[Snapshot]].

## Purpose

Single entry point for "do the scrape". Wraps the Playwright lifecycle, gathers `ProviderEntry` results, writes `data/snapshots/snapshot-YYYY-MM-DD.json`, emits structured events to an optional callback (for the [[Module - web app|web launcher]] SSE stream).

## Public surface

- `run(emit: EmitCallback | None = None) -> Snapshot` — scrape all providers, persist, return the Snapshot
- `latest_snapshot() -> Snapshot | None` — read the most-recent JSON from `snapshot_dir()`
- `EmitCallback = Callable[[dict], None]` — event type alias
- Private: `_print_emit`, `_persist`

Event types emitted to the callback:

- `scrape_start` — `{provider, url, index, total}`
- `scrape_done` — `{provider, mgmt_pct, products}`
- `scrape_error` — `{provider, error}`
- `persist` — `{path}`
- `complete` — `{snapshot_date, ok, errors}`

## Depends on

- [[Module - schema]] — `Snapshot`, `ProviderEntry`
- [[Module - paths]] — `snapshot_dir()`
- [[Module - providers base]] — indirectly, via subclasses
- `fund_snoop.providers.ALL_SCRAPERS` — registered scraper classes
- Playwright sync API

## Used by

- [[Module - cli]] — `run()` for full scrape
- [[Module - web app]] — `run(emit=queue.put)` to stream events to SSE clients
- Tests indirectly via fixtures (not directly imported in tests)

## Domain concepts handled

[[Snapshot]] (produces + persists) · [[Provider]] (iterates all 22) · [[Product]] (counts via `synthesized_products()` for the `scrape_done` event)

## Relevant decisions

- [[ADR-0001 on-demand only]] — synchronous, no scheduler
- [[ADR-0003 product-centric taxonomy]] — `scrape_done.products` uses synthesized product count

## Cross-links

- [[Mind map]] § Data flow + Scraper anatomy
- [[Tech stack]] § Run
- [[Known issues]] § scraper accuracy

## Live state

Source file is the truth. `HANDOVER.md` § "Key files" + § "Web launcher updates" describe the latest event-payload shape.

## Source of truth

`fund_snoop/runner.py` (repo).
