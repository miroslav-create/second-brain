---
type: module
tags:
  - module/core
aliases:
  - fund_snoop.schema
  - schema.py
---

# Module — `fund_snoop.schema`

> File: `fund_snoop/schema.py`. Single source of dataclass truth. Aligns 1:1 with `CONTEXT.md` § Language.

## Purpose

Defines every domain dataclass + Enum the rest of the codebase consumes. Round-trips to/from JSON for [[Snapshot]] persistence. Provides legacy → new migration on read.

## Public surface

Enums:
- `ProviderType` — see [[ProviderType]] (4 values)
- `ProductType` — see [[ProductType]] (6 values)
- `PRODUCT_TYPE_LABELS` — Czech labels per ProductType (dashboard)

Dataclasses:
- `FeeSchedule` — see [[FeeSchedule]]
- `Minimum` — see [[Minimum]]
- `Performance` — see [[Performance]]
- `Product` — see [[Product]]
- `ProviderEntry` — Provider bundle in a [[Snapshot]]
- `Snapshot` — see [[Snapshot]]
- `Portfolio` *(legacy, pre-[[ADR-0003 product-centric taxonomy]])*
- `HeadlineInstrument` *(legacy, pre-[[ADR-0003 product-centric taxonomy]])*

Methods:
- `ProviderEntry.synthesized_products()` — unifies `products` + legacy `portfolios` + legacy `headline_instruments` into a single typed [[Product]] list. Renderer reads via this.
- `Snapshot.to_dict()` / `Snapshot.from_dict()` — JSON round-trip; `from_dict` migrates legacy keys.

Private helpers: `_parse_portfolio`, `_parse_product`, `_parse_performance_list`.

## Depends on

- stdlib only (`dataclasses`, `datetime`, `enum`)

## Used by

- [[Module - runner]] — produces `Snapshot`, `ProviderEntry`
- [[Module - render report]] — consumes everything
- [[Module - providers base]] — `ProviderScraper` references `ProviderEntry` + `ProviderType`
- All 22 scrapers — emit Products, FeeSchedule, Minimum

## Domain concepts handled

[[Provider]] · [[ProviderType]] · [[Product]] · [[ProductType]] (+ 6 children: [[ProductType - Savings account]], [[ProductType - Term deposit]], [[ProductType - Robo portfolio]], [[ProductType - ETF headline]], [[ProductType - Mutual fund]], [[ProductType - DIP]]) · [[FeeSchedule]] · [[Minimum]] · [[Performance]] · [[Snapshot]]

[[Overlap]] is **not** defined here — it's derived in [[Module - render report]].

## Relevant decisions

- [[ADR-0003 product-centric taxonomy]] — defines the schema this module encodes
- [[ADR-0002 broker scope]] — explains why `etf_headline` fields are mostly empty

## Cross-links

- [[Mind map]] § Domain model
- [[Tech stack]] § Data model
- [[About]] § Fields per Product

## Live state

Source file is authoritative. See `HANDOVER.md` § Key files for last known signature shape.

## Source of truth

`fund_snoop/schema.py` (repo).
