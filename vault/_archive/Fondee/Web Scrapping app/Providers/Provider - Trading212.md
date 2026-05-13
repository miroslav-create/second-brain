---
type: provider
tags:
  - provider/broker
aliases:
  - Trading212
  - Trading 212
  - trading212
---

# Provider — Trading212

> Online broker (UK). Pro Fund Snoop: jen headline ETFs jako metadata (viz [[ADR-0002 broker scope]]).

Patří do kategorie [[ProviderType]] = `broker`.
Vztah k Fondee: [[Overlap]] = `off` (ETF jako instrument, ne user-pickable produkt).

## Produkty (ProductType)

- [[ProductType - ETF headline|Headline ETFs]] — kurátorovaný seznam z class konstanty (ne scrapováno z page textu)

## Pozice na trhu

Phase C scaffold. Broker scraper neextrahuje fee — jen `management_note` + curated `headline_instruments`. ADR-0002 trade-off: ztrácíme viditelnost edge instrumentů, ale stabilní porovnání vs Fondee headline fee.

## Scraper

- Modul: `fund_snoop/providers/trading212.py`
- Vzor: broker — class-konstanty + `management_note`, žádný `_parse(text)` pro instruments
- Phase: C

## Vazby

- Koncept: [[Provider]] · [[ProviderType]] = broker
- Produkty: [[ProductType - ETF headline]]
- Decisions: [[ADR-0002 broker scope]] (zakotvuje scope)
- Narrative: [[About]] § Scope · [[Status]] § Live data → Brokers · [[Known issues]] § Broker headline_instruments

## Live state

Autoritativní: `HANDOVER.md` § "Scraper accuracy" → **Brokers**. Mirror v [[Status]] § Live data.

## Source of truth

Repo `HANDOVER.md` + `fund_snoop/providers/trading212.py`.
