---
type: concept
tags:
  - concept/domain
aliases:
  - overlap
  - badge
---

# Overlap

> A per-[[Product]] classification of how the Product competes with [[Provider - Fondee|Fondee]]'s offering. Drives the colored badge on [[Summary|SummaryCards]] and the ordering of products within each card.

## Values

- **direct** — [[ProductType - Robo portfolio]]. Fondee's core market. Green badge.
- **adjacent** — [[ProductType - Savings account]], [[ProductType - Term deposit]], [[ProductType - Mutual fund]], [[ProductType - DIP]]. Substitute or complementary products. Lime badge.
- **off** — [[ProductType - ETF headline]]. Brokers' headline ETFs; Fondee uses ETFs as instruments *inside* portfolios, not as user-pickable products (see [[ADR-0002 broker scope]]). Grey badge.

## Role in the domain

Overlap is **derived** from [[ProductType]] — not stored. The mapping is a one-way function in [[Module - render report]] (`_overlap(product_type) → "direct" | "adjacent" | "off"`).

Ordering: within a SummaryCard, top-5 Products are sorted by overlap rank (`direct=0 < adjacent=1 < off=2`) so the most-relevant offer appears first.

## Relationships

- Each [[Product]] maps to exactly one Overlap value via its [[ProductType]]
- Visualised on [[Summary]] cards
- Defined operationally in [[Module - render report]] (no enum in [[Module - schema]])

## Used by

- [[Module - render report]] — `_overlap()` helper, `_OVERLAP_RANK` dict for product ordering, `SummaryProduct.overlap` field

## See also

- [[About]] § Product types
- [[ADR-0003 product-centric taxonomy]] (defines ProductType)
- [[ADR-0004 summary as landing]] (introduces overlap badges)
- `CONTEXT.md` § Overlap

## Source of truth

`CONTEXT.md` + `fund_snoop/render/report.py` (`_overlap`, `_OVERLAP_RANK`).
