---
type: provider
tags:
  - provider/robo
aliases:
  - Portu
  - portu
---

# Provider — Portu

> Český robo-poradce (WOOD & Company). Nabízí [[ProductType - Robo portfolio|robo portfolia]] Portu 5 / 10 / Premium. Headline reference pro Phase D selector pattern.

Patří do kategorie [[ProviderType]] = `robo`.
Vztah k Fondee: [[Overlap]] = `direct` (přímý konkurent).

## Produkty (ProductType)

- [[ProductType - Robo portfolio|Robo portfolio]] — 4 portfolia, management **1,0 %** p.a.

## Pozice na trhu

Phase D referenční scraper. Fee panel je za záložkou — scraper kliká na tab + čte xpath panel. Anchor `Bez slevy X %` rozlišuje skutečnou fee od dočasných slev. Reálný DOM fixture v `tests/fixtures/portu.txt`.

## Scraper

- Modul: `fund_snoop/providers/portu.py`
- Vzor: vlastní `scrape()` (přepisuje default ze [[Module - providers base]]) — tab click + xpath panel, anchor `Bez slevy X %`
- Phase: D (per-site CSS selectors shipped 2026-05-10)

## Vazby

- Koncept: [[Provider]] · [[ProviderType]] = robo
- Produkty: [[ProductType - Robo portfolio]]
- Decisions: [[ADR-0003 product-centric taxonomy]]
- Narrative: [[About]] § Scope · [[Status]] § Live data → Portu · [[Pickup checklist]] § Reference scrapers

## Live state

Autoritativní: `HANDOVER.md` § "Scraper accuracy" → **Portu**. Mirror v [[Status]] § Live data.

## Source of truth

Repo `HANDOVER.md` + `fund_snoop/providers/portu.py`.
