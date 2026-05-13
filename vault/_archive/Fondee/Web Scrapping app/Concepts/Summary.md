---
type: concept
tags:
  - concept/domain
aliases:
  - summary
  - SummaryCard
  - landing
---

# Summary

> The per-[[Provider]] headline view on the [[Report]] landing page. One `SummaryCard` per Provider, grouped by [[ProviderType]] section. Target reading time: ≤1 min per card.

## Role in the domain

Phase F landing artifact (see [[ADR-0004 summary as landing]]). Composed from the latest [[Snapshot]] only — **not** a diff. Sections appear in fixed order:

1. Robo poradci → 3 cards
2. Banky → 11 cards (savings + mutual fund / DIP)
3. Brokeři → 5 cards
4. Investiční společnosti → 3 cards

Total: 22 cards across 4 sections.

## SummaryCard shape

Each card carries:

- Top ≤5 [[Product|Products]] ordered by [[Overlap]] rank (direct first, adjacent next, off last)
- Per-Product [[Overlap]] badge (green = direct, lime = adjacent, grey = off)
- Fee mini-row (Provider-wide [[FeeSchedule]] tx/fx/entry)
- [[Minimum]] line
- One-line note

## Relationships

- Belongs to one [[Report]]
- Built from one [[Snapshot]]
- Linked downward to faceted `/products` page at the table cross-link

## Used by

- [[Module - render report]] — `SummaryCard` / `SummaryProduct` dataclasses, `build_summary_cards`, `_top_products`, `_overlap`, `_format_headline`, `_format_fee_line`, `_format_minimum`. Template: `summary.html.j2`.

## See also

- [[About]] § Decisions
- [[Status]] § Phase F
- [[Mind map]] § Renderer split
- [[ADR-0004 summary as landing]]
- `CONTEXT.md` § Summary

## Source of truth

`CONTEXT.md` + `fund_snoop/render/report.py` (repo).
