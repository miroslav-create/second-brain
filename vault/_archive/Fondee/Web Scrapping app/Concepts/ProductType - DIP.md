---
type: concept
tags:
  - concept/product-type
aliases:
  - dip
  - DIP
  - Dlouhodobý investiční produkt
  - Pillar 3
---

# ProductType — DIP

> Czech `Dlouhodobý investiční produkt` (Pillar 3, since 2024). Tax-advantaged wrapper around eligible Products. Headline = `management_pct` of the underlying + tax-relief note.

[[Overlap]]: **adjacent** — DIP wraps Products that would otherwise compete with [[Provider - Fondee|Fondee]] directly.

## DIP is a wrapper, not a security

A customer can hold a [[ProductType - Robo portfolio]] *or* a [[ProductType - Mutual fund]] **inside** a DIP. The DIP `Product` row represents the wrapper the Provider sells; the underlying assets are referenced in `note`.

This makes DIP rows unusual: they describe a tax structure plus a pointer to what the operator can put inside. See [[CONTEXT.md|`CONTEXT.md`]] § Flagged ambiguities.

## Headline shape

- `management_pct` — of the underlying (often inherited from the wrapped fund/portfolio)
- `note` — required; describes the wrapper + tax-relief framing + what fits inside
- No fixed [[Performance]] window (depends on the underlying)

## Providers emitting this type (3)

- [[Provider - Conseq]] — ZENIT DIP
- [[Provider - Generali]] — DIP marker alongside premium + listed funds
- [[Provider - ČSOB]] — ČSOB DIP marker (alongside savings)

## Relationships

- Parent: [[ProductType]]
- Belongs to a [[Product]] via `product_type=DIP`
- Wrapped in [[Overlap]] = adjacent
- Wraps a [[ProductType - Robo portfolio]] or [[ProductType - Mutual fund]] conceptually

## See also

- [[About]] § Product types
- [[Status]] § Investment houses + Banks fonds/DIP/robo
- [[ADR-0003 product-centric taxonomy]]
- `CONTEXT.md` § ProductType
