---
type: adr
tags:
  - adr/accepted
aliases:
  - ADR-0004
  - summary as landing
---

# ADR-0004 — Summary as landing, faceted table moved to /products

> Status: **Accepted** (shipped Phase F, 2026-05-10). Repo: `docs/adr/0004-summary-as-landing.md`.

## Context

The faceted product table at `reports/index.html` (Phase E) answered "fee vs fee deep dive" well, but it forced users to apply filters before each [[Provider]]'s shape was visible. The most-asked question from Fondee staff is "show me what each competitor offers, headline-only" — a question the table answered poorly.

## Decision

`reports/index.html` becomes the **Summary** landing page — one [[Summary|SummaryCard]] per [[Provider]], grouped by [[ProviderType]], targeting ≤1 min reading time per competitor. The faceted product table moves to `reports/products.html`, linked from header + footer of the summary. The launcher exposes both via `/report` and `/products`.

Per-Product [[Overlap]] badges (`direct` / `adjacent` / `off`) encode the Fondee-relevance derived from [[ProductType]] (per [[ADR-0003 product-centric taxonomy]]).

## Consequences

- **Positive:** landing serves the high-frequency question. Cards group naturally by [[ProviderType]] section (Robo poradci → Banky → Brokeři → Investiční společnosti). Faceted table remains for deep dives at `/products`.
- **Trade-off:** any OneDrive bookmark pointing at the old `index.html` for the table now lands on the summary. Users must re-bookmark `/products`.

## Impacts

- Concepts: [[Summary]], [[Report]], [[Overlap]].
- Modules: [[Module - render report]] (`render_summary_html`, `build_summary_cards`, `_overlap`), [[Module - web app]] (`GET /products`).
- Templates: `summary.html.j2` (new), `products.html.j2` (renamed from `report.html.j2`), `_fondee_tokens.css.j2` shared.

## Cross-links

- [[Status]] § Phase F
- [[About]] § Decisions
- [[Mind map]] § Renderer split
- Related: [[ADR-0003 product-centric taxonomy]] (defines the ProductType taxonomy [[Overlap]] derives from).

## Source of truth

`docs/adr/0004-summary-as-landing.md` (repo).
