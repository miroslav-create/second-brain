---
type: provider
tags:
  - provider/bank
aliases:
  - Air Bank
  - airbank
---

# Provider — Air Bank

> Online retail banka (PPF group). Pro Fund Snoop: Spořicí účet s 2,6 % bonus, stropem 500 000 Kč a podmínkami aktivity.

Patří do kategorie [[ProviderType]] = `bank`.
Vztah k Fondee: [[Overlap]] = `adjacent`.

## Produkty (ProductType)

- [[ProductType - Savings account|Spořicí účet]] — **2,6 %** p.a., `rate_ceiling = 500 000 Kč`, `rate_conditions` parsované z hero

## Pozice na trhu

Phase E (2026-05-10). Single-tier savings — anchor jediná fráze typu `úroku 2,6 % ročně`. Reference scraper pro headline-savings-rate pattern.

Kandidát pro budoucí [[ProductType - Term deposit|term-deposit]] scraper.

## Scraper

- Modul: `fund_snoop/providers/airbank.py`
- Vzor: single-tier savings, [[Module - providers base|first_percent_pa]] anchor
- Phase: E

## Vazby

- Koncept: [[Provider]] · [[ProviderType]] = bank
- Produkty: [[ProductType - Savings account]]
- Narrative: [[About]] § Scope · [[Status]] § Live data → Air Bank · [[Pickup checklist]] § Reference scrapers → Headline savings rate

## Live state

Autoritativní: `HANDOVER.md` § "Scraper accuracy" → **Air Bank**. Mirror v [[Status]] § Live data.

## Source of truth

Repo `HANDOVER.md` + `fund_snoop/providers/airbank.py`.
