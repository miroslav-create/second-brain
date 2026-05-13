---
type: concept
tags:
  - concept/product-type
aliases:
  - savings_account
  - Spořicí účet
  - Savings account
---

# ProductType — Savings account

> Czech `spořicí účet`. Bank product paying a headline rate (`rate_pct` p.a.), often tiered with a `rate_ceiling` (eg "do 500 000 Kč") and `rate_conditions` (eg "při aktivním používání karty").

[[Overlap]]: **adjacent** — substitute for cash held outside [[Provider - Fondee|Fondee]].

## Headline shape

- `rate_pct` (p.a.)
- `rate_ceiling` — tier limit free-text
- `rate_conditions` — bonus / loyalty constraints
- No [[Performance]] entries (rate is the headline)

## Providers emitting this type (10)

Pure savings:
- [[Provider - Air Bank]]
- [[Provider - Creditas]]
- [[Provider - Raiffeisen]]
- [[Provider - Trinity]]
- [[Provider - Max banka]]
- [[Provider - mBank]] (three products: Plus / Děti / mSpoření)
- [[Provider - Moneta]]
- [[Provider - UniCredit]] (WAF fallback — see [[Known issues]])

Savings alongside other products:
- [[Provider - KB]] (KB Spořicí + Dětský)
- [[Provider - ČSOB]] (ČSOB Spořicí účet promo)

## Relationships

- Parent: [[ProductType]]
- Belongs to a [[Product]] via `product_type=SAVINGS_ACCOUNT`
- Wrapped in [[Overlap]] = adjacent
- Provider-wide [[FeeSchedule]] rarely applies; per-Product `rate_*` fields carry the truth

## See also

- [[About]] § Product types
- [[Status]] § Banks — savings accounts
- [[ADR-0003 product-centric taxonomy]]
- `CONTEXT.md` § ProductType
