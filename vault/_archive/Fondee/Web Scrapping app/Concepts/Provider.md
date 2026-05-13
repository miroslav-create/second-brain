---
type: concept
tags:
  - concept/domain
aliases:
  - Provider
  - poskytovatel
  - competitor
---

# Provider

> A competitor entity whose offering Fund Snoop tracks. Currently 22 wired Providers across 4 [[ProviderType|ProviderTypes]].

_Avoid_: "competitor", "company", "vendor". Always "Provider".

## Role in the domain

A Provider is the unit of scraping. One [[Module - providers base|ProviderScraper]] subclass per Provider. Each Provider entry in a [[Snapshot]] carries:

- `name`, `provider_type` ([[ProviderType]]), `source_url`
- one [[FeeSchedule]] (provider-wide context)
- one [[Minimum]] (provider-wide context)
- zero-or-more [[Product|Products]]
- legacy `portfolios` / `headline_instruments` lists for back-compat (synthesized into Products on read)

## Roster (22)

Grouped by [[ProviderType]]:

- **Robo (3):** [[Provider - Fondee]], [[Provider - Portu]], [[Provider - Indigo]]
- **Bank (11):** [[Provider - KB]], [[Provider - ČSOB]], [[Provider - Erste ČS]], [[Provider - Air Bank]], [[Provider - Creditas]], [[Provider - Raiffeisen]], [[Provider - Trinity]], [[Provider - Max banka]], [[Provider - mBank]], [[Provider - Moneta]], [[Provider - UniCredit]]
- **Broker (5):** [[Provider - Trading212]], [[Provider - XTB]], [[Provider - eToro]], [[Provider - Revolut]], [[Provider - DEGIRO]]
- **Investment house (3):** [[Provider - JT IS]], [[Provider - Conseq]], [[Provider - Generali]]

## Relationships

- Has one [[ProviderType]]
- Has one [[FeeSchedule]]
- Has one [[Minimum]]
- Has zero-or-more [[Product|Products]] (each with its own [[ProductType]])
- Appears as one card on the [[Summary]] landing
- One row per Product on the faceted `/products` table

## Used by

- [[Module - schema]] — `ProviderEntry` dataclass + `synthesized_products()` helper
- [[Module - providers base]] — `ProviderScraper` ABC
- [[Module - runner]] — iterates `ALL_SCRAPERS`
- [[Module - render report]] — groups Products by Provider for the dashboard

## See also

- [[About]] § Scope → Providers wired
- [[Status]] § Providers wired
- [[Tech stack]] § Data model
- [[ADR-0003 product-centric taxonomy]] (introduces Product-centric model)
- `CONTEXT.md` § Provider

## Source of truth

`CONTEXT.md` + `fund_snoop/schema.py` + `fund_snoop/providers/__init__.py` (`ALL_SCRAPERS`).
