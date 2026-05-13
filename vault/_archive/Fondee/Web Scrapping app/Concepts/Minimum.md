---
type: concept
tags:
  - concept/domain
aliases:
  - minimum
  - minimální vklad
---

# Minimum

> [[Provider]]'s minimum first deposit and minimum recurring contribution. Per-Product overrides live on the [[Product]] itself.

## Role in the domain

A Minimum belongs to one Provider. When all of a Provider's Products share the same threshold (typical for [[Provider - Fondee]], [[Provider - Portu]] etc.), the Minimum captures it once. Banks with rate-varying tiers per product set `Minimum(None, None)` and let each [[ProductType - Savings account]] Product carry its own `minimum_first` / `minimum_recurring`.

## Fields

- `first_deposit` — free-text (eg "1 000 Kč", "100 EUR", "žádný")
- `recurring` — free-text (eg "od 500 Kč/měs.")

## Relationships

- One [[Provider]] → one Minimum
- Per-Product overrides on [[Product]] (`minimum_first`, `minimum_recurring`)
- Surfaces in the [[Summary|SummaryCard]] minimum line + faceted dashboard column

## Used by

- [[Module - schema]] — `Minimum` dataclass
- [[Module - render report]] — `_format_minimum()` helper
- All robo + investment-house scrapers (banks usually leave it blank)

## See also

- [[About]] § Fields per Product
- [[Known issues]] § Minimums hardcoded per scraper, not actually scraped
- `CONTEXT.md` § Minimum

## Source of truth

`CONTEXT.md` + `fund_snoop/schema.py` (repo).
