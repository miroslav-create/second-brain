---
type: provider
tags:
  - provider/investment-house
aliases:
  - J&T IS
  - J&T Investiční společnost
  - JT IS
  - jtis
---

# Provider — J&T Investiční společnost

> Česká investiční společnost (J&T Group). Pro Fund Snoop: 3 podílové fondy (Money / Dividend / Bond) s 1y performance.

Patří do kategorie [[ProviderType]] = `investment_house`.
Vztah k Fondee: [[Overlap]] = `adjacent`.

## Produkty (ProductType)

- [[ProductType - Mutual fund|J&T Money]] — 1y perf **4,25 %**
- [[ProductType - Mutual fund|J&T Dividend]] — 1y perf **17,22 %**
- [[ProductType - Mutual fund|J&T Bond]] — 1y perf **5,06 %**

## Pozice na trhu

Phase E (2026-05-10). Reference scraper pro mutual-fund tile parsing pattern (1 perf value per fund). Performance je parsovaná z homepage tile bloků.

`management_pct` chybí — skutečné per-fund mgmt fees sídlí v KIID PDF (viz [[Known issues]] § per-fund mgmt fees behind KIID PDFs).

## Scraper

- Modul: `fund_snoop/providers/jtis.py`
- Vzor: mutual-fund tile parsing (1 perf)
- Phase: E

## Vazby

- Koncept: [[Provider]] · [[ProviderType]] = investment_house · [[Performance]]
- Produkty: [[ProductType - Mutual fund]]
- Narrative: [[About]] § Scope · [[Status]] § Live data → J&T IS · [[Pickup checklist]] § Reference scrapers → Mutual-fund tile parsing

## Live state

Autoritativní: `HANDOVER.md` § "Scraper accuracy" → **J&T IS**. Mirror v [[Status]] § Live data.

## Source of truth

Repo `HANDOVER.md` + `fund_snoop/providers/jtis.py`.
