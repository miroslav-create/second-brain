---
type: concept
tags:
  - concept/domain
aliases:
  - performance
  - výkonnost
---

# Performance

> A return figure tied to a [[Product]], with time window and disclosure date. Applies to **managed** Products only.

## Role in the domain

Performance entries hang off Products of type [[ProductType - Robo portfolio]], [[ProductType - Mutual fund]], [[ProductType - DIP]]. They do **not** apply to:

- [[ProductType - Savings account]] / [[ProductType - Term deposit]] — these expose `rate_pct` instead.
- [[ProductType - ETF headline]] — per [[ADR-0002 broker scope]], brokers' headline ETFs are tracked as metadata, no scraped performance.

_Avoid_: "returns" or "yields" (overloaded). Always "performance".

## Fields

- `window` — free-text window ("1y", "3y", "12m", "5y")
- `value_pct` — numeric return %
- `as_of` — disclosure date (ISO `YYYY-MM-DD`)
- `raw` — diagnostic snippet from the page

## Relationships

- A managed [[Product]] has zero-or-more Performance entries
- Captured at scrape time; values move every snapshot

## Used by

- [[Module - schema]] — `Performance` dataclass (+ `_parse_performance_list`)
- [[Provider - Generali]] — emits 12m / 3y / 5y per premium fund
- [[Provider - JT IS]] — emits 1y per fund
- [[Provider - Fondee]] — `X,Y % p.a.` regex near portfolio name (sketch)

## See also

- [[About]] § Fields per Product
- [[Known issues]] § Performance scraping only sketched in Fondee
- `CONTEXT.md` § Performance

## Source of truth

`CONTEXT.md` + `fund_snoop/schema.py` (repo).
