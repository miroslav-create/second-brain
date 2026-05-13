---
type: moc
tags:
  - moc
---

# Fund Snoop — Index

MOC for the **Web Scrapping app** folder. Graph view should be dense from this node. Static reference; live state lives in repo `HANDOVER.md` (see [[Status]] for the Obsidian-side mirror).

Top-level: [[About]] · [[Status]] · [[Known issues]] · [[Pickup checklist]] · [[Tech stack]] · [[Mind map]] · [[Name]]

## Concepts

Domain language atoms (from `CONTEXT.md`):

- [[Provider]] · [[ProviderType]]
- [[Product]] · [[ProductType]]
  - [[ProductType - Savings account]]
  - [[ProductType - Term deposit]]
  - [[ProductType - Robo portfolio]]
  - [[ProductType - ETF headline]]
  - [[ProductType - Mutual fund]]
  - [[ProductType - DIP]]
- [[FeeSchedule]] · [[Minimum]] · [[Performance]]
- [[Snapshot]] · [[Report]] · [[Summary]] · [[Overlap]]

## Providers (22)

### Robo poradci

- [[Provider - Fondee]] · [[Provider - Portu]] · [[Provider - Indigo]]

### Banky — fondy / DIP / robo

- [[Provider - KB]] · [[Provider - ČSOB]] · [[Provider - Erste ČS]]

### Banky — spořicí účty

- [[Provider - Air Bank]] · [[Provider - Creditas]] · [[Provider - Raiffeisen]] · [[Provider - Trinity]] · [[Provider - Max banka]] · [[Provider - mBank]] · [[Provider - Moneta]] · [[Provider - UniCredit]]

### Brokeři

- [[Provider - Trading212]] · [[Provider - XTB]] · [[Provider - eToro]] · [[Provider - Revolut]] · [[Provider - DEGIRO]]

### Investiční společnosti

- [[Provider - JT IS|J&T IS]] · [[Provider - Conseq]] · [[Provider - Generali]]

## ADRs

- [[ADR-0001 on-demand only]]
- [[ADR-0002 broker scope]]
- [[ADR-0003 product-centric taxonomy]]
- [[ADR-0004 summary as landing]]

## Core modules

- [[Module - schema]] — dataclasses, `ProductType` / `ProviderType` Enums, `Snapshot` round-trip
- [[Module - runner]] — orchestrates 22 scrapers, writes Snapshot, emits events
- [[Module - providers base]] — `ProviderScraper` ABC, `_get_body_text`, percent regex helpers
- [[Module - render report]] — Summary + faceted Products HTML, `_overlap` mapping
- [[Module - web app]] — FastAPI launcher, SSE log
- [[Module - cli]] — `python -m fund_snoop` entry, `--render-only` flag
- [[Module - paths]] — repo / frozen-mode path resolver

## Source of truth

Repo: `c:\Users\miroslav.zachar\OneDrive - Direct\Projects\Scrapping\`

- Live state: `HANDOVER.md`
- Domain language: `CONTEXT.md`
- Decisions: `docs/adr/`
