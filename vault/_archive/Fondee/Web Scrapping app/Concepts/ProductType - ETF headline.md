---
type: concept
tags:
  - concept/product-type
aliases:
  - etf_headline
  - Headline ETF
  - flagship ETF
---

# ProductType — ETF headline

> Flagship ETF a broker promotes — per [[ADR-0002 broker scope]], the headline only, not the full instrument catalogue.

[[Overlap]]: **off** — brokers' headline ETFs; Fondee uses ETFs as instruments *inside* portfolios, not as user-pickable products. Grey badge.

## Headline shape

- `ticker` — eg "VWCE", "CSPX"
- `isin` — sometimes
- `note` — trustworthy plain-text context (eg "ETF zaměřený na globální akcie")
- No `management_pct` (broker doesn't charge a Provider-side fee for it)
- No [[Performance]] entries

## Provenance — metadata, not scraped

Per [[ADR-0002 broker scope]], broker headline ETFs are populated from **hardcoded class constants** inside each broker scraper. `_parse(text)` does **not** extract them from page text. If a broker URL succeeds but the page is wrong, the dashboard still shows the curated ETF list. See [[Known issues]] § Broker headline_instruments are metadata.

Legacy `HeadlineInstrument` dataclass in [[Module - schema]] kept for back-compat — `synthesized_products()` upgrades it to `Product(product_type=ETF_HEADLINE)` on read.

## Providers emitting this type (5)

- [[Provider - Trading212]]
- [[Provider - XTB]]
- [[Provider - eToro]]
- [[Provider - Revolut]]
- [[Provider - DEGIRO]]

## Relationships

- Parent: [[ProductType]]
- Belongs to a [[Product]] via `product_type=ETF_HEADLINE`
- Wrapped in [[Overlap]] = off
- Constrained by [[ADR-0002 broker scope]]

## See also

- [[About]] § Out of scope
- [[Known issues]] § Broker headline_instruments
- [[ADR-0002 broker scope]] / [[ADR-0003 product-centric taxonomy]]
- `CONTEXT.md` § ProductType
