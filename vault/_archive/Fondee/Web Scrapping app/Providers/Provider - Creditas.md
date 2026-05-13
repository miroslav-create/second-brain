---
type: provider
tags:
  - provider/bank
aliases:
  - Banka Creditas
  - Creditas
  - creditas
---

# Provider — Banka Creditas

> Česká retail banka. Pro Fund Snoop: Spořicí účet + s headline 4,0 % (3 % base + 1 % bonus pro investující klienty).

Patří do kategorie [[ProviderType]] = `bank`.
Vztah k Fondee: [[Overlap]] = `adjacent`.

## Produkty (ProductType)

- [[ProductType - Savings account|Spořicí účet +]] — headline **4,0 %** (3 % base + 1 % investing bonus)

## Pozice na trhu

Phase E (2026-05-10). 2024 sloučila Max banku → ta dnes běží pod doménou `maxbanka.eu` jako samostatný produktový brand pod Creditas.

Kandidát pro [[ProductType - Term deposit|term-deposit]] scraper.

## Scraper

- Modul: `fund_snoop/providers/creditas.py`
- Vzor: single-tier savings
- Phase: E

## Vazby

- Koncept: [[Provider]] · [[ProviderType]] = bank
- Produkty: [[ProductType - Savings account]]
- Související: [[Provider - Max banka]] (sloučená banka)
- Narrative: [[About]] § Scope · [[Status]] § Live data → Creditas

## Live state

Autoritativní: `HANDOVER.md` § "Scraper accuracy" → **Creditas**. Mirror v [[Status]] § Live data.

## Source of truth

Repo `HANDOVER.md` + `fund_snoop/providers/creditas.py`.
