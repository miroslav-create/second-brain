---
type: concept
tags:
  - concept/product-type
aliases:
  - term_deposit
  - Termínovaný vklad
  - Term deposit
---

# ProductType — Term deposit

> Czech `termínovaný vklad`. Bank product locking funds for `term_months` against a fixed `rate_pct` (p.a.).

[[Overlap]]: **adjacent** — substitute for cash held outside [[Provider - Fondee|Fondee]].

## Headline shape

- `rate_pct` (p.a.)
- `term_months` (integer)
- `rate_conditions` — sometimes (eg "new clients only")
- No [[Performance]] entries

## Providers emitting this type (none yet — backlog)

No scraper currently emits `term_deposit` Products. The banks already wired for [[ProductType - Savings account]] advertise term-deposit products on adjacent pages, but the scrapers ignore them.

Candidates for a Phase G pass:

- [[Provider - Air Bank]] / [[Provider - Creditas]] / [[Provider - Max banka]] / [[Provider - Trinity]]

Pattern: one probe → scraper per bank yielding `Product(type=TERM_DEPOSIT, term_months=N, rate_pct=X)`. Identical to existing savings scrapers (see [[Module - providers base]]).

## Relationships

- Parent: [[ProductType]]
- Belongs to a [[Product]] via `product_type=TERM_DEPOSIT`
- Wrapped in [[Overlap]] = adjacent

## See also

- [[Status]] § Open backlog → Term deposits
- [[Known issues]] § Term deposits not yet emitted
- [[Pickup checklist]] § Next ideas → Term-deposit scrapers
- [[ADR-0003 product-centric taxonomy]]
- `CONTEXT.md` § ProductType
