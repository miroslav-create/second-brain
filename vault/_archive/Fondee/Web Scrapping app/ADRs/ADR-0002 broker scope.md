---
type: adr
tags:
  - adr/accepted
aliases:
  - ADR-0002
  - broker scope
  - headline ETFs only
---

# ADR-0002 — Broker scope: headline ETFs only

> Status: **Accepted**. Repo: `docs/adr/0002-broker-scope-headline-etfs.md`.

## Context

Brokers ([[Provider - Trading212]], [[Provider - XTB]], [[Provider - eToro]], [[Provider - Revolut]], [[Provider - DEGIRO]]) list 10k+ instruments each. Full-catalogue scrapes are 10× larger, more brittle (anti-bot, login walls, churn), and produce noise that swamps the comparison signal vs Fondee.

## Decision

For broker [[Provider]] entries, scrape only the ETFs / indices the broker promotes as **flagship** offerings. Encoded as `ProductType.ETF_HEADLINE`. See [[ProductType - ETF headline]].

## Consequences

- **Positive:** stable, low-cadence scraping. Apples-to-apples comparison against [[Provider - Fondee]] portfolio fees. No login walls.
- **Trade-off:** lose visibility into edge instruments. Headline lists for brokers are populated from hardcoded class constants — `_parse(text)` does not extract them from page text. Documented metadata, not scraped data.

This shapes the [[Overlap]] taxonomy: `etf_headline` Products are tagged `off` (off-scope vs Fondee's core robo offering).

## Impacts

- Concepts: [[ProductType - ETF headline]], [[Overlap]], [[Product]].
- Modules: [[Module - providers base]] (broker scrapers subclass it).
- Providers (5): [[Provider - Trading212]], [[Provider - XTB]], [[Provider - eToro]], [[Provider - Revolut]], [[Provider - DEGIRO]].

## Cross-links

- [[About]] § Out of scope
- [[Known issues]] § Broker headline_instruments are metadata
- Related: [[ADR-0003 product-centric taxonomy]] (introduces `etf_headline` as a ProductType value).

## Source of truth

`docs/adr/0002-broker-scope-headline-etfs.md` (repo).
