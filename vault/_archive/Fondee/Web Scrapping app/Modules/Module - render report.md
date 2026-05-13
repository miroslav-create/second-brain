---
type: module
tags:
  - module/core
aliases:
  - fund_snoop.render.report
  - render/report.py
---

# Module — `fund_snoop.render.report`

> File: `fund_snoop/render/report.py`. Renders a [[Snapshot]] into both surfaces: [[Summary]] landing + faceted products table.

## Purpose

The only place [[Snapshot]] → HTML happens. Builds `SummaryCard` list for the landing + `ProductRow` list for the faceted table. Renders both via Jinja templates. Writes `reports/index.html`, `reports/products.html`, plus dated archive copies.

## Public surface

Renderers:
- `render_summary_html(snapshot) -> str` (template: `summary.html.j2`)
- `render_products_html(snapshot) -> str` (template: `products.html.j2`)
- `render_html(snapshot) -> str` — back-compat alias → `render_products_html`
- `write_report(snapshot) -> ReportPaths` — writes all four files (summary + products + archive copies)

Builders:
- `build_summary_cards(snapshot) -> list[SummaryCard]`
- (faceted) builds `ProductRow` flat list with facet options

Dataclasses:
- `ProductRow` — flat dashboard row
- `SummaryCard` — one per Provider, on the landing
- `SummaryProduct` — top-N Products inside a card
- `ReportPaths` — return value of `write_report`

Helpers / mappings:
- `_overlap(product_type) -> "direct" | "adjacent" | "off"` — see [[Overlap]]
- `_OVERLAP_RANK` — `{"direct": 0, "adjacent": 1, "off": 2}` for top-product ordering
- `_top_products`, `_format_headline`, `_format_fee_line`, `_format_minimum`
- `PROVIDER_TYPE_SECTION` — Czech section labels for the [[Summary]] landing
- `PRODUCT_TYPE_LABELS` — re-exported from [[Module - schema]]

## Depends on

- [[Module - schema]] — `Snapshot`, `ProviderEntry`, `Product`, `ProductType`, `ProviderType`, `PRODUCT_TYPE_LABELS`
- [[Module - paths]] — `reports_dir()`, `archive_dir()`
- Jinja2 (`Environment`, `FileSystemLoader`, `select_autoescape`)

Templates live under `fund_snoop/render/templates/`:
- `summary.html.j2`, `products.html.j2`, `_fondee_tokens.css.j2` (shared partial with palette + typography tokens)

## Used by

- [[Module - cli]] — `write_report(snapshot)` after scrape or `--render-only`
- [[Module - web app]] — `write_report` on `complete`; `/report` + `/products` serve the files

## Domain concepts handled

[[Report]] (produces both surfaces) · [[Summary]] (cards) · [[Overlap]] (derives, owns the mapping)

## Relevant decisions

- [[ADR-0004 summary as landing]] — drove the split into two renderers
- [[ADR-0003 product-centric taxonomy]] — `ProductRow` flattens any ProductType uniformly
- [[ADR-0002 broker scope]] — `etf_headline` Products get `Overlap = off`

## Cross-links

- [[Mind map]] § Renderer split
- [[Tech stack]] § Output
- [[Status]] § Phase F
- [[Known issues]] § Cosmetic shared % filter range

## Live state

Source file is authoritative. `HANDOVER.md` § "Renderer split" describes the split.

## Source of truth

`fund_snoop/render/report.py` + `fund_snoop/render/templates/` (repo).
