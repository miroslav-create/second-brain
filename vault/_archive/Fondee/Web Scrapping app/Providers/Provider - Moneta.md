---
type: provider
tags:
  - provider/bank
aliases:
  - Moneta
  - Moneta Money Bank
  - moneta
---

# Provider — Moneta Money Bank

> Česká retail banka. Pro Fund Snoop: Spořicí účet s tiered sazbou (2,6 % do 1 mil. / 0,5 % nad), garance do 30. 6. 2026.

Patří do kategorie [[ProviderType]] = `bank`.
Vztah k Fondee: [[Overlap]] = `adjacent`.

## Produkty (ProductType)

- [[ProductType - Savings account|Spořicí účet]] — **2,6 %** p.a. do 1 000 000 Kč, **0,5 %** nad

## Pozice na trhu

Phase E (2026-05-10). Tiered savings s ceilingem + dočasnou garancí sazby. Pattern shodný s [[Provider - Max banka]].

## Scraper

- Modul: `fund_snoop/providers/moneta.py`
- Vzor: tiered savings (ceiling + over-tier)
- Phase: E

## Vazby

- Koncept: [[Provider]] · [[ProviderType]] = bank
- Produkty: [[ProductType - Savings account]]
- Narrative: [[About]] § Scope · [[Status]] § Live data → Moneta · [[Pickup checklist]] § Reference scrapers

## Live state

Autoritativní: `HANDOVER.md` § "Scraper accuracy" → **Moneta**. Mirror v [[Status]] § Live data.

## Source of truth

Repo `HANDOVER.md` + `fund_snoop/providers/moneta.py`.
