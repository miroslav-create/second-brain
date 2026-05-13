---
type: provider
tags:
  - provider/robo
aliases:
  - Indigo
  - indigo
---

# Provider — Indigo

> Český robo-poradce. Plochý poplatek 1 % p.a. napříč portfolii.

Patří do kategorie [[ProviderType]] = `robo`.
Vztah k Fondee: [[Overlap]] = `direct`.

## Produkty (ProductType)

- [[ProductType - Robo portfolio|Robo portfolio]] — plochý management **1,0 %** z `/pricing` stránky

## Pozice na trhu

URL ukazuje na `/pricing` (ne `/poplatky` jak měla Phase B). Anchor `Roční poplatek pouze X %` (žádná `p.a.` markace na live stránce). Phase D fix shipnut 2026-05-10.

## Scraper

- Modul: `fund_snoop/providers/indigo.py`
- Vzor: text-extraction přes [[Module - providers base|_scrape_via_text]]
- Phase: D (fixed 2026-05-10)

## Vazby

- Koncept: [[Provider]] · [[ProviderType]] = robo
- Produkty: [[ProductType - Robo portfolio]]
- Narrative: [[About]] § Scope · [[Status]] § Live data → Indigo · [[Known issues]] § Closed in Phase E/F (URL fix)

## Live state

Autoritativní: `HANDOVER.md` § "Scraper accuracy" → **Indigo**. Mirror v [[Status]] § Live data.

## Source of truth

Repo `HANDOVER.md` + `fund_snoop/providers/indigo.py`.
