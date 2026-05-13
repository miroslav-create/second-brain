---
type: provider
tags:
  - provider/broker
aliases:
  - DEGIRO
  - Degiro
  - degiro
---

# Provider — DEGIRO

> Online broker (NL, flatexDEGIRO). Pro Fund Snoop: jen headline ETFs jako metadata (viz [[ADR-0002 broker scope]]).

Patří do kategorie [[ProviderType]] = `broker`.
Vztah k Fondee: [[Overlap]] = `off`.

## Produkty (ProductType)

- [[ProductType - ETF headline|Headline ETFs]] — kurátorovaný seznam z class konstanty (DEGIRO core selection)

## Pozice na trhu

Phase C scaffold. DEGIRO má core selection ETFs (bezpoplatkové) — to je pro headline-only scope dobře aproximovatelné kurátorovaným seznamem.

## Scraper

- Modul: `fund_snoop/providers/degiro.py`
- Vzor: broker — class-konstanty
- Phase: C

## Vazby

- Koncept: [[Provider]] · [[ProviderType]] = broker
- Produkty: [[ProductType - ETF headline]]
- Decisions: [[ADR-0002 broker scope]]
- Narrative: [[About]] § Scope · [[Status]] § Live data → Brokers

## Live state

Autoritativní: `HANDOVER.md` § "Scraper accuracy" → **Brokers**. Mirror v [[Status]] § Live data.

## Source of truth

Repo `HANDOVER.md` + `fund_snoop/providers/degiro.py`.
