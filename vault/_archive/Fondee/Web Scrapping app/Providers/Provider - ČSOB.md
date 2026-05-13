---
type: provider
tags:
  - provider/bank
aliases:
  - ČSOB
  - CSOB
  - csob
---

# Provider — ČSOB

> Velká retail banka (KBC group). Pro Fund Snoop: ČSOB Spořicí účet (3-měsíční promo) + DIP marker. Homepage pivot.

Patří do kategorie [[ProviderType]] = `bank`.
Vztah k Fondee: [[Overlap]] = `adjacent`.

## Produkty (ProductType)

- [[ProductType - Savings account|Spořicí účet]] — ČSOB Spořicí účet **3,80 %** (3-měsíční promo sazba)
- [[ProductType - DIP|DIP]] — marker
- [[ProductType - Mutual fund|Podílové fondy]] — distribuce KBC fondů (omezený extract; mgmt fees v KIID PDF)

## Pozice na trhu

Phase B → Phase D pivot. Hluboké fee-stránky nedostupné, scraper čte homepage produkt-tiles.

## Scraper

- Modul: `fund_snoop/providers/csob.py`
- Vzor: homepage pivot
- Phase: D (2026-05-10)

## Vazby

- Koncept: [[Provider]] · [[ProviderType]] = bank
- Produkty: [[ProductType - Savings account]] · [[ProductType - DIP]] · [[ProductType - Mutual fund]]
- Narrative: [[About]] § Scope · [[Status]] § Live data → ČSOB · [[Known issues]] § per-fund mgmt fees behind KIID PDFs

## Live state

Autoritativní: `HANDOVER.md` § "Scraper accuracy" → **ČSOB**. Mirror v [[Status]] § Live data.

## Source of truth

Repo `HANDOVER.md` + `fund_snoop/providers/csob.py`.
