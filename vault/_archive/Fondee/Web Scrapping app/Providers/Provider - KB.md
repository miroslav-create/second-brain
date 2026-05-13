---
type: provider
tags:
  - provider/bank
aliases:
  - KB
  - Komerční banka
  - KB Investice
  - kb
---

# Provider — KB (Komerční banka)

> Velká retail banka. Pro Fund Snoop kombinace: spořicí účty, řízený fond Buy and Watch, DIP marker. Homepage pivot — hluboké fee-stránky `/poplatky` jsou JS-walled.

Patří do kategorie [[ProviderType]] = `bank`.
Vztah k Fondee: [[Overlap]] = mix (`direct` u Buy and Watch, `adjacent` u spořicích).

## Produkty (ProductType)

- [[ProductType - Savings account|Spořicí účet]] — KB Spořicí **3,5 %** p.a.
- [[ProductType - Savings account|Spořicí účet]] — KB Dětský **3,5 %** p.a.
- [[ProductType - Robo portfolio|Robo portfolio / fond]] — Buy and Watch s **4,9 %** expected return
- [[ProductType - DIP|DIP]] — marker (Pillar 3 wrapper)

## Pozice na trhu

Phase B → Phase D pivot. Hluboké stránky nedostupné; scraper čte homepage produkt-tiles. Headline scrapeny z homepage hero panels.

## Scraper

- Modul: `fund_snoop/providers/kb.py`
- Vzor: homepage pivot — tile-by-tile parsing z body textu
- Phase: D (2026-05-10)

## Vazby

- Koncept: [[Provider]] · [[ProviderType]] = bank
- Produkty: [[ProductType - Savings account]] · [[ProductType - Robo portfolio]] · [[ProductType - DIP]]
- Decisions: [[ADR-0003 product-centric taxonomy]]
- Narrative: [[About]] § Scope · [[Status]] § Live data → KB · [[Pickup checklist]] § Homepage pivot pattern

## Live state

Autoritativní: `HANDOVER.md` § "Scraper accuracy" → **Komerční banka**. Mirror v [[Status]] § Live data.

## Source of truth

Repo `HANDOVER.md` + `fund_snoop/providers/kb.py`.
