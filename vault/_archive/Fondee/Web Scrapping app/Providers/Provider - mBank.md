---
type: provider
tags:
  - provider/bank
aliases:
  - mBank
  - mbank
---

# Provider — mBank

> Online retail banka (Commerzbank → polský původ). Pro Fund Snoop: tři spořicí produkty paralelně (Plus / Děti / mSpoření).

Patří do kategorie [[ProviderType]] = `bank`.
Vztah k Fondee: [[Overlap]] = `adjacent`.

## Produkty (ProductType)

- [[ProductType - Savings account|mKonto Plus]] — **4,01 %** p.a.
- [[ProductType - Savings account|mKonto Děti]] — **5,01 %** p.a.
- [[ProductType - Savings account|mSpoření]] — **3,01 %** p.a.

## Pozice na trhu

Phase E (2026-05-10). Reference scraper pro multi-product savings pattern — scraper iteruje pojmenované produkty na jedné stránce a anchoruje nejbližší %.

## Scraper

- Modul: `fund_snoop/providers/mbank.py`
- Vzor: multi-product savings na jedné stránce
- Phase: E

## Vazby

- Koncept: [[Provider]] · [[ProviderType]] = bank
- Produkty: [[ProductType - Savings account]]
- Narrative: [[About]] § Scope · [[Status]] § Live data → mBank · [[Pickup checklist]] § Reference scrapers → Multi-product savings

## Live state

Autoritativní: `HANDOVER.md` § "Scraper accuracy" → **mBank**. Mirror v [[Status]] § Live data.

## Source of truth

Repo `HANDOVER.md` + `fund_snoop/providers/mbank.py`.
