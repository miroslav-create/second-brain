---
type: provider
tags:
  - provider/bank
aliases:
  - Raiffeisenbank
  - Raiffeisen
  - raiffeisen
---

# Provider — Raiffeisenbank

> Česká retail banka (RBI group). Pro Fund Snoop: Bonusový spořicí účet 4,2 % z homepage hera.

Patří do kategorie [[ProviderType]] = `bank`.
Vztah k Fondee: [[Overlap]] = `adjacent`.

## Produkty (ProductType)

- [[ProductType - Savings account|Bonusový spořicí účet]] — **4,2 %** p.a. z homepage hero

## Pozice na trhu

Phase E (2026-05-10). Single-tier headline z hero panelu. Anchor např. `až 4,2 % p. a.`.

## Scraper

- Modul: `fund_snoop/providers/raiffeisen.py`
- Vzor: single-tier savings z homepage
- Phase: E

## Vazby

- Koncept: [[Provider]] · [[ProviderType]] = bank
- Produkty: [[ProductType - Savings account]]
- Narrative: [[About]] § Scope · [[Status]] § Live data → Raiffeisen · [[Pickup checklist]] § Reference scrapers

## Live state

Autoritativní: `HANDOVER.md` § "Scraper accuracy" → **Raiffeisenbank**. Mirror v [[Status]] § Live data.

## Source of truth

Repo `HANDOVER.md` + `fund_snoop/providers/raiffeisen.py`.
