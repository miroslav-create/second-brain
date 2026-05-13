---
type: provider
tags:
  - provider/investment-house
aliases:
  - Generali Investments
  - Generali Investments CEE
  - Generali
  - generali
---

# Provider — Generali Investments CEE

> Česká pobočka italské skupiny Generali. Pro Fund Snoop: 3 prémiové + 6 listed fondů + DIP. Performance ve třech oknech (12m / 3y / 5y).

Patří do kategorie [[ProviderType]] = `investment_house`.
Vztah k Fondee: [[Overlap]] = `adjacent`.

## Produkty (ProductType)

- [[ProductType - Mutual fund|Prémiové fondy]] — 3 fondy, 12m / 3y / 5y perf parsed
- [[ProductType - Mutual fund|Listed fondy]] — 6 fondů
- [[ProductType - DIP|DIP]] — marker

## Pozice na trhu

Phase E (2026-05-10). Tile parsing s třemi performance windows (12m, 3y, 5y) per prémiový fond. Pattern shodný s [[Provider - JT IS]], ale rozšířen o multiwindow [[Performance]].

`management_pct` per-fund chybí (KIID PDF).

## Scraper

- Modul: `fund_snoop/providers/generali.py`
- Vzor: mutual-fund tile parsing (3 perfs per tile)
- Phase: E

## Vazby

- Koncept: [[Provider]] · [[ProviderType]] = investment_house · [[Performance]]
- Produkty: [[ProductType - Mutual fund]] · [[ProductType - DIP]]
- Narrative: [[About]] § Scope · [[Status]] § Live data → Generali · [[Pickup checklist]] § Reference scrapers → Mutual-fund tile parsing

## Live state

Autoritativní: `HANDOVER.md` § "Scraper accuracy" → **Generali**. Mirror v [[Status]] § Live data.

## Source of truth

Repo `HANDOVER.md` + `fund_snoop/providers/generali.py`.
