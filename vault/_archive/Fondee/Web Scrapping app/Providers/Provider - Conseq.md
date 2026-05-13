---
type: provider
tags:
  - provider/investment-house
aliases:
  - Conseq
  - Conseq Investment Management
  - conseq
---

# Provider — Conseq

> Česká investiční společnost. Pro Fund Snoop: ~14 fondů (Active Invest + Conseq Invest) + ZENIT DIP.

Patří do kategorie [[ProviderType]] = `investment_house`.
Vztah k Fondee: [[Overlap]] = `adjacent`.

## Produkty (ProductType)

- [[ProductType - Mutual fund|Conseq fondy]] — Active Invest tier + Conseq Invest tier; NAV-based extraction (~14 fondů)
- [[ProductType - DIP|ZENIT DIP]] — Pillar 3 wrapper

## Pozice na trhu

Phase E (2026-05-10). Reference scraper pro fund-list NAV table pattern — multiline regex na `<name>\n<NAV> CZK`.

`management_pct` per-fund chybí (KIID PDF).

## Scraper

- Modul: `fund_snoop/providers/conseq.py`
- Vzor: fund-list NAV table; multiline regex
- Phase: E

## Vazby

- Koncept: [[Provider]] · [[ProviderType]] = investment_house
- Produkty: [[ProductType - Mutual fund]] · [[ProductType - DIP]]
- Narrative: [[About]] § Scope · [[Status]] § Live data → Conseq · [[Pickup checklist]] § Reference scrapers → Fund-list NAV table

## Live state

Autoritativní: `HANDOVER.md` § "Scraper accuracy" → **Conseq**. Mirror v [[Status]] § Live data.

## Source of truth

Repo `HANDOVER.md` + `fund_snoop/providers/conseq.py`.
