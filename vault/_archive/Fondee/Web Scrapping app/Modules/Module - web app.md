---
type: module
tags:
  - module/core
aliases:
  - fund_snoop.web.app
  - web/app.py
  - FastAPI launcher
---

# Module — `fund_snoop.web.app`

> File: `fund_snoop/web/app.py`. Local FastAPI launcher. Button → live SSE log → report link, for non-tech Fondee staff via `start_app.bat`.

## Purpose

Wraps [[Module - runner|run()]] in a one-job-at-a-time FastAPI app that streams events to a browser frontend. Boots and dies on demand (no hosting). Serves both [[Report]] surfaces via static endpoints.

## Public surface

FastAPI app `app: FastAPI` with endpoints:

- `GET  /` — control page (button + status pill + progress bar + live log + report links)
- `POST /scrape` — kick off background scrape (rejects with 409 if already running)
- `GET  /events` — SSE stream of current/last job events
- `GET  /report` — serve `reports/index.html` ([[Summary]] landing)
- `GET  /products` — serve `reports/products.html` (faceted table)
- `GET  /status` — JSON job snapshot (for refresh-safe frontend reconnect)

Class `JobState` — holds `lock`, `job_id`, `status` (`idle | running | done | error`), `events: deque[200]`, `subscribers: list[asyncio.Queue]`, `loop`. Worker thread pushes events via `push_event()`; FastAPI loop fans them out to SSE subscribers.

`render` event payload (after Phase F): `summary_path` + `products_path` (was `report_path` + `archive_path`).

## Depends on

- [[Module - runner]] — `run()` invoked in worker thread with `emit=queue.put`
- [[Module - render report]] — `write_report` on `complete`
- [[Module - paths]] — `reports_dir()` mount
- FastAPI + Starlette + uvicorn + asyncio
- `fund_snoop/web/templates/index.html` (Czech frontend, Fondee palette)
- `fund_snoop/web/__main__.py` — uvicorn launcher with auto-open browser

## Used by

- `start_app.bat` at repo root — clickable launcher
- `python -m fund_snoop.web` — manual boot

## Domain concepts handled

[[Report]] (serves) · [[Snapshot]] (via runner) · job state (in-memory, not persisted)

## Relevant decisions

- [[ADR-0001 on-demand only]] — boots on click, dies on close
- [[ADR-0004 summary as landing]] — `/report` defaults to summary; `/products` exposed separately

## Cross-links

- [[Mind map]] § Data flow → Web branch
- [[Tech stack]] § Core
- [[About]] § How it works
- [[Pickup checklist]] § PyInstaller bundle (zero-install distribution)

## Live state

Source file + frontend template are authoritative. `HANDOVER.md` § "Web launcher updates" describes recent payload changes.

## Source of truth

`fund_snoop/web/app.py` + `fund_snoop/web/__main__.py` + `fund_snoop/web/templates/index.html` (repo).
