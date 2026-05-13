---
type: provider
tags:
  - provider/bank
aliases:
  - Max banka
  - maxbanka
  - Max
---

# Provider — Max banka

> Brand pod Banka Creditas po fúzi. Pro Fund Snoop: Spořicí účet TOP s tiered sazbou (3,0 % do 500k / 2,8 % nad).

Patří do kategorie [[ProviderType]] = `bank`.
Vztah k Fondee: [[Overlap]] = `adjacent`.

## Produkty (ProductType)

- [[ProductType - Savings account|Spořicí účet TOP]] — **3,0 %** do 500 000 Kč / **2,8 %** nad

## Pozice na trhu

Phase E (2026-05-10). Po fúzi s Creditas přesun na doménu `maxbanka.eu`. Tiered savings — anchor na tier label + nejbližší %. Reference scraper pro tier-pattern (ceiling + over-tier).

Kandidát pro [[ProductType - Term deposit|term-deposit]] scraper.

## Scraper

- Modul: `fund_snoop/providers/maxbanka.py`
- Vzor: tiered savings (ceiling + over-tier)
- Phase: E
- URL po fúzi: `maxbanka.eu`

## Vazby

- Koncept: [[Provider]] · [[ProviderType]] = bank
- Produkty: [[ProductType - Savings account]]
- Související: [[Provider - Creditas]] (parent brand)
- Narrative: [[About]] § Scope · [[Status]] § Live data → Max banka · [[Pickup checklist]] § Reference scrapers

## Live state

Autoritativní: `HANDOVER.md` § "Scraper accuracy" → **Max banka**. Mirror v [[Status]] § Live data.

## Source of truth

Repo `HANDOVER.md` + `fund_snoop/providers/maxbanka.py`.
