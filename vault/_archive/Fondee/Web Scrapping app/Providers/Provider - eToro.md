---
type: provider
tags:
  - provider/broker
aliases:
  - eToro
  - etoro
---

# Provider — eToro

> Online broker / social-trading platforma (IL/UK). Pro Fund Snoop: jen headline ETFs jako metadata (viz [[ADR-0002 broker scope]]).

Patří do kategorie [[ProviderType]] = `broker`.
Vztah k Fondee: [[Overlap]] = `off`.

## Produkty (ProductType)

- [[ProductType - ETF headline|Headline ETFs]] — kurátorovaný seznam z class konstanty

## Pozice na trhu

Phase C scaffold. Stejný broker pattern. eToro je zvláštní v tom, že má i copy-trading a kryptoaktiva — Fund Snoop tyto produkty ignoruje (mimo scope per ADR-0002).

## Scraper

- Modul: `fund_snoop/providers/etoro.py`
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

Repo `HANDOVER.md` + `fund_snoop/providers/etoro.py`.
