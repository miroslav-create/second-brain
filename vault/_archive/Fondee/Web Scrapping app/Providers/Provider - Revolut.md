---
type: provider
tags:
  - provider/broker
aliases:
  - Revolut
  - revolut
---

# Provider — Revolut

> Neobanka + broker (UK). Pro Fund Snoop: jen headline ETFs jako metadata (viz [[ADR-0002 broker scope]]).

Patří do kategorie [[ProviderType]] = `broker`.
Vztah k Fondee: [[Overlap]] = `off`.

## Produkty (ProductType)

- [[ProductType - ETF headline|Headline ETFs]] — kurátorovaný seznam z class konstanty

## Pozice na trhu

Phase C scaffold. Revolut má vlastní robo (Robo) i savings vault, ale Fund Snoop ho klasifikuje jako brokera podle hlavního investičního produktu pro CZ uživatele. ADR-0002 limituje scope na headline ETFs.

## Scraper

- Modul: `fund_snoop/providers/revolut.py`
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

Repo `HANDOVER.md` + `fund_snoop/providers/revolut.py`.
