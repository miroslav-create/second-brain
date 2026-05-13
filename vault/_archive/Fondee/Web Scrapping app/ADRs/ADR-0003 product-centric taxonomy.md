---
type: adr
tags:
  - adr/accepted
aliases:
  - ADR-0003
  - product-centric taxonomy
  - Product
  - ProductType
---

# ADR-0003 — Product-centric data model and shared taxonomy

> Status: **Accepted**. Repo: `docs/adr/0003-product-centric-taxonomy.md`.

## Context

Phase A modelled a [[Provider]] as a container of managed-portfolio offerings only ([[ProductType - Robo portfolio|robo_portfolios]] + headline ETFs). The dashboard had to compare a 4,3 % p.a. savings account to a 0,9 % robo management fee — fundamentally different products. Forcing them into separate report sections hid the trade-off Fondee staff actually want to see.

## Decision

[[Provider]] becomes a container of zero-or-more **Products**. Each [[Product]] carries a [[ProductType]] from a shared vocabulary of six values:

- [[ProductType - Savings account]]
- [[ProductType - Term deposit]]
- [[ProductType - Robo portfolio]]
- [[ProductType - ETF headline]]
- [[ProductType - Mutual fund]]
- [[ProductType - DIP]]

The dataclass is **denormalized** (one `Product` with type-specific optional fields) rather than a class hierarchy. Lives in [[Module - schema]].

## Consequences

- **Positive:** dashboard renders one filterable table — `provider`, `type`, `headline_pct`. [[Overlap]] taxonomy derives directly from ProductType. New ProductTypes are additive.
- **Trade-off:** denormalized schema → many fields nullable. Legacy `Portfolio` + `HeadlineInstrument` records must be migrated on read. Migration path: `ProviderEntry.synthesized_products()` unifies legacy lists into typed Products; `Snapshot.from_dict` maps old JSON keys to new model. Old `data/snapshots/*.json` remain readable.

## Impacts

- Concepts: [[Product]], [[ProductType]] + all 6 child notes, [[Provider]], [[ProviderType]], [[Overlap]], [[FeeSchedule]], [[Minimum]], [[Performance]].
- Modules: [[Module - schema]] (definitions), [[Module - render report]] (consumes synthesized Products).
- Providers: all 22 emit Products of one or more ProductTypes — see [[Status]] § "Providers wired".

## Cross-links

- [[About]] § Product types
- [[CONTEXT.md|`CONTEXT.md`]] § Language
- [[Tech stack]] § Data model
- [[Mind map]] § Domain model
- Related: [[ADR-0002 broker scope]] (introduces `etf_headline`), [[ADR-0004 summary as landing]] (uses [[Overlap]] derived from ProductType).

## Source of truth

`docs/adr/0003-product-centric-taxonomy.md` (repo).
