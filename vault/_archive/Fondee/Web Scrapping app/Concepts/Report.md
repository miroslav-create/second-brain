---
type: concept
tags:
  - concept/domain
aliases:
  - report
  - dashboard
---

# Report

> The rendered HTML view of one [[Snapshot]]. Two pages, both self-contained, both painted with Fondee tokens (`#004033` + `#C4DE00`, Inter typography).

## Role in the domain

A Report is one ephemeral artifact per Snapshot, plus a dated archive copy:

- `reports/index.html` — [[Summary]] landing page (per-Provider cards grouped by [[ProviderType]]). See [[ADR-0004 summary as landing]].
- `reports/products.html` — faceted product table. Sidebar filters: [[ProductType]] (checkboxes), [[Provider]] (checkboxes), minimum-deposit range, headline rate/fee range. Single result table; rows highlight green/red against [[Provider - Fondee]] baseline.
- `reports/archive/*-YYYY-MM-DD.html` — dated copies of both surfaces.

## Relationships

- Renders one [[Snapshot]] (latest)
- Hosts one [[Summary]] view + one faceted products view
- Distributed via OneDrive shared link (static) or local FastAPI launcher (live)

## Used by

- [[Module - render report]] — `render_summary_html`, `render_products_html`, `write_report`
- [[Module - paths]] — `reports_dir()`, `archive_dir()`
- [[Module - web app]] — serves both pages via `GET /report` + `GET /products`

## See also

- [[About]] § Use case
- [[Tech stack]] § Output
- [[Mind map]] § Renderer split
- [[ADR-0004 summary as landing]]
- `CONTEXT.md` § Report

## Source of truth

`CONTEXT.md` + `fund_snoop/render/report.py` (repo).
