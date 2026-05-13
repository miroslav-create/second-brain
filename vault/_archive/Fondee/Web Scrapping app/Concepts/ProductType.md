---
type: concept
tags:
  - concept/domain
aliases:
  - ProductType
  - product type
---

# ProductType

> Shared taxonomy a [[Product]] belongs to. Drives which fields are populated and which filters/columns the dashboard exposes.

Introduced by [[ADR-0003 product-centric taxonomy]].

## Values (6)

| Value | Note | Czech label |
|---|---|---|
| [[ProductType - Savings account]] | Bank rate product (tiered) | Spořicí účet |
| [[ProductType - Term deposit]] | Locked-term rate product | Termínovaný vklad |
| [[ProductType - Robo portfolio]] | Robo-advisor managed portfolio | Robo portfolio |
| [[ProductType - ETF headline]] | Broker flagship ETF (metadata) | Headline ETF |
| [[ProductType - Mutual fund]] | Investment-house fund | Podílový fond |
| [[ProductType - DIP]] | Pillar 3 tax wrapper | DIP (Pilíř III) |

Czech labels exposed via `PRODUCT_TYPE_LABELS` in [[Module - schema]].

## Role in the domain

ProductType is **the** discriminator. It drives:

- Which `Product` fields scrapers populate (rate-bearing vs fee-bearing vs securities).
- Which dashboard filters apply (`/products` sidebar checkboxes).
- The [[Overlap]] classification per Product (one-way derivation in [[Module - render report]]).
- Section grouping on the [[Summary]] landing page (alongside [[ProviderType]]).

## Relationships

- Each [[Product]] has exactly one ProductType
- Each [[ProviderType]] tends to emit certain ProductTypes (robo → robo_portfolio; bank → savings_account / mutual_fund / dip; broker → etf_headline; investment_house → mutual_fund + dip)
- Maps deterministically to [[Overlap]]

## Used by

- [[Module - schema]] — `ProductType` Enum + `PRODUCT_TYPE_LABELS`
- [[Module - render report]] — facet options, section grouping, overlap mapping
- All 22 scrapers tag every Product with one value

## See also

- [[About]] § Product types
- [[Tech stack]] § Data model
- [[ADR-0003 product-centric taxonomy]]
- `CONTEXT.md` § ProductType

## Source of truth

`CONTEXT.md` + `fund_snoop/schema.py` (`ProductType` Enum).
