---
type: provider
tags:
  - provider/bank
aliases:
  - Trinity Bank
  - Trinity
  - trinity
---

# Provider — Trinity Bank

> Menší česká retail banka. Pro Fund Snoop: Narozeninový spořicí účet 3,30 % z homepage stat-bloku.

Patří do kategorie [[ProviderType]] = `bank`.
Vztah k Fondee: [[Overlap]] = `adjacent`.

## Produkty (ProductType)

- [[ProductType - Savings account|Narozeninový spořicí účet]] — **3,30 %** p.a. z homepage stat block

## Pozice na trhu

Phase E (2026-05-10). Inner pages SPA-routují → 404 na direct GET. Pouze homepage stat block exponuje sazbu. Pokud Trinity přejmenuje produkt nebo přeskládá hero, scraper tiše rozbije se.

Kandidát pro [[ProductType - Term deposit|term-deposit]] scraper.

## Scraper

- Modul: `fund_snoop/providers/trinity.py`
- Vzor: homepage stat block
- Phase: E
- Křehkost: SPA — wording-dependent

## Vazby

- Koncept: [[Provider]] · [[ProviderType]] = bank
- Produkty: [[ProductType - Savings account]]
- Narrative: [[About]] § Scope · [[Status]] § Live data → Trinity · [[Known issues]] § Trinity Bank inner pages

## Live state

Autoritativní: `HANDOVER.md` § "Scraper accuracy" → **Trinity Bank**. Mirror v [[Status]] § Live data.

## Source of truth

Repo `HANDOVER.md` + `fund_snoop/providers/trinity.py`.
