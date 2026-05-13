---
type: concept
tags:
  - concept/domain
aliases:
  - ProviderType
  - provider type
---

# ProviderType

> The category a [[Provider]] belongs to. 4 values, used for [[Summary]] section grouping and per-Provider tag/colour conventions in the dashboard.

## Values (4)

### robo

Robo-advisors. Core competitors of Fondee. Emit [[ProductType - Robo portfolio]].
Czech section label: **Robo poradci**.

Members: [[Provider - Fondee]], [[Provider - Portu]], [[Provider - Indigo]].

### bank

Retail banks. Emit a mix of [[ProductType - Savings account]], [[ProductType - Mutual fund]], [[ProductType - DIP]], occasionally [[ProductType - Robo portfolio]].
Czech section label: **Banky** (split visually into fonds/DIP/robo + savings-only on the [[Summary]] landing).

Members: [[Provider - KB]], [[Provider - ČSOB]], [[Provider - Erste ČS]], [[Provider - Air Bank]], [[Provider - Creditas]], [[Provider - Raiffeisen]], [[Provider - Trinity]], [[Provider - Max banka]], [[Provider - mBank]], [[Provider - Moneta]], [[Provider - UniCredit]].

### broker

Online brokers. Emit [[ProductType - ETF headline]] only, per [[ADR-0002 broker scope]] (curated metadata, not scraped).
Czech section label: **Brokeři**.

Members: [[Provider - Trading212]], [[Provider - XTB]], [[Provider - eToro]], [[Provider - Revolut]], [[Provider - DEGIRO]].

### investment_house

Mutual-fund issuers. Emit [[ProductType - Mutual fund]] + sometimes [[ProductType - DIP]] markers.
Czech section label: **Investiční společnosti**.

Members: [[Provider - JT IS]], [[Provider - Conseq]], [[Provider - Generali]].

## Section ordering

[[Module - render report]] enforces this order on the [[Summary]] landing (`PROVIDER_TYPE_SECTION` mapping):

`Robo poradci → Banky → Brokeři → Investiční společnosti`.

## Relationships

- Each [[Provider]] has exactly one ProviderType
- Each ProviderType tends to emit a stable subset of [[ProductType|ProductTypes]]
- Drives [[Summary]] section grouping (not [[Overlap]] — that derives from ProductType)

## Used by

- [[Module - schema]] — `ProviderType` Enum (4 values)
- [[Module - render report]] — `PROVIDER_TYPE_SECTION` Czech labels
- All 22 scrapers set it on `ProviderEntry`

## See also

- [[About]] § Scope
- [[Status]] § Providers wired
- [[ADR-0003 product-centric taxonomy]] (adds `INVESTMENT_HOUSE`)
- `CONTEXT.md` § ProviderType

## Source of truth

`CONTEXT.md` + `fund_snoop/schema.py` (`ProviderType` Enum).
