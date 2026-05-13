---
type: concept
tags:
  - concept/product-type
aliases:
  - robo_portfolio
  - Robo portfolio
  - portfolio
---

# ProductType — Robo portfolio

> Managed portfolio at a robo-advisor or bank (Fondee Klasická, Portu 5, KB Premium…). Headline = `management_pct` (p.a. fee). Carries [[Performance]] entries.

[[Overlap]]: **direct** — Fondee's core market. Green badge on [[Summary|SummaryCards]].

## Headline shape

- `management_pct` (p.a.)
- `risk_level` (free-text, eg "Klasická", "5")
- `allocation` (free-text)
- `minimum_first` / `minimum_recurring` — usually inherited from Provider [[Minimum]]
- [[Performance]] list — managed Product, expects future entries

## Naming clarity

The word **"portfolio"** is reserved for this ProductType. Bank mutual-fund offerings use [[ProductType - Mutual fund]], not "portfolio". See [[CONTEXT.md|`CONTEXT.md`]] § Flagged ambiguities.

Legacy `Portfolio` dataclass in [[Module - schema]] kept for back-compat — `synthesized_products()` upgrades it to `Product(product_type=ROBO_PORTFOLIO)` on read.

## Providers emitting this type (4)

- [[Provider - Fondee]] — 2 portfolios; legacy schema
- [[Provider - Portu]] — 4 portfolios; Phase D xpath panel
- [[Provider - Indigo]] — flat 1 % from `/pricing`
- [[Provider - KB]] — Buy and Watch fund (alongside savings)

## Relationships

- Parent: [[ProductType]]
- Belongs to a [[Product]] via `product_type=ROBO_PORTFOLIO`
- Wrapped in [[Overlap]] = direct
- Has zero-or-more [[Performance]] entries

## See also

- [[About]] § Product types
- [[Status]] § Robos
- [[ADR-0003 product-centric taxonomy]]
- `CONTEXT.md` § ProductType
