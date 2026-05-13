---
type: concept
tags:
  - concept/domain
aliases:
  - product
  - produkt
---

# Product

> A single offering a [[Provider]] sells: one savings account, one term deposit, one robo portfolio, one headline ETF, one mutual fund, one DIP scheme.

Introduced by [[ADR-0003 product-centric taxonomy]]. Replaces the old `Portfolio` + `HeadlineInstrument` split.

## Role in the domain

A Provider has zero-or-more Products. Each Product carries one [[ProductType]] from a shared vocabulary. The dashboard row = Product, filterable by ProductType + Provider + headline %.

_Avoid_: "portfolio" (now reserved for [[ProductType - Robo portfolio]]), "fund" (now reserved for [[ProductType - Mutual fund]]), "plan".

## Fields by category

Generic:
- `name`, `note` (always set), `risk_level`, `allocation`, `minimum_first`, `minimum_recurring`

Fee-bearing ([[ProductType - Robo portfolio|robo_portfolio]] / [[ProductType - Mutual fund|mutual_fund]] / [[ProductType - DIP|dip]]):
- `management_pct`, `entry_fee`, `transaction_fee`

Rate-bearing ([[ProductType - Savings account|savings_account]] / [[ProductType - Term deposit|term_deposit]]):
- `rate_pct`, `rate_ceiling`, `rate_conditions`, `term_months`

Securities ([[ProductType - ETF headline|etf_headline]]):
- `ticker`, `isin`

Managed:
- [[Performance]] list (`robo_portfolio`, `mutual_fund`, `dip`)

## Relationships

- Belongs to one [[Provider]]
- Has one [[ProductType]]
- May have many [[Performance]] entries (managed only)
- Drives [[Overlap]] classification (one-way from ProductType)
- Per-Product fees override Provider-wide [[FeeSchedule]]
- Per-Product `minimum_first`/`minimum_recurring` override Provider [[Minimum]]

## Used by

- [[Module - schema]] — `Product` dataclass + `_parse_product`
- [[Module - render report]] — `ProductRow` flattens it for the faceted table; `SummaryProduct` for cards
- All 22 scrapers emit zero-or-more Products

## See also

- [[About]] § Fields per Product
- [[Status]] § Open backlog
- [[Tech stack]] § Data model
- [[ADR-0003 product-centric taxonomy]]
- `CONTEXT.md` § Product

## Source of truth

`CONTEXT.md` + `fund_snoop/schema.py` (repo).
