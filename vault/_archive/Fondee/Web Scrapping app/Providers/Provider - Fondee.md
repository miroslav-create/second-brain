---
type: provider
tags:
  - provider/robo
aliases:
  - Fondee
  - fondee
---

# Provider — Fondee

> Český robo-poradce. Spravuje [[ProductType - Robo portfolio|robo portfolia]] (Klasická, Udržitelná atd.). Pro Fund Snoop slouží jako referenční baseline — ostatní providery se proti Fondee zelená/červeně vyznačují v dashboardu `/products`.

Patří do kategorie [[ProviderType]] = `robo`.
Vztah ke konkurenci: Fondee sám sobě — ostatní providery se měří proti němu.

## Produkty (ProductType)

- [[ProductType - Robo portfolio|Robo portfolio]] — 2 portfolia, management 0,9 % p.a.

## Pozice na trhu

Phase A robot. Headline `management_pct = 0,9 %` z fee-page. Legacy schéma — emituje stále `Portfolio` + `HeadlineInstrument` (back-compat); renderer dotahuje přes `synthesized_products()` jako [[ProductType - Robo portfolio]].

Performance jen naznačena — `X,Y % p.a.` regex blízko názvu portfolia (viz [[Performance]] limity).

## Scraper

- Modul: `fund_snoop/providers/fondee.py`
- Vzor: text-extraction přes [[Module - providers base|_scrape_via_text]] + `first_percent_pa`
- Phase: A (skeleton)

## Vazby

- Koncept: [[Provider]] · [[ProviderType]] = robo
- Produkty: [[ProductType - Robo portfolio]]
- Decisions: [[ADR-0003 product-centric taxonomy]] (legacy → synthesized)
- Narrative: [[About]] § Scope · [[Status]] § Live data → Fondee · [[Mind map]] § Inputs

## Live state

Autoritativní: `HANDOVER.md` § "Scraper accuracy — live run 2026-05-10" → řádek **Fondee**. Mirror v [[Status]] § Live data.

## Source of truth

Repo `HANDOVER.md` + `fund_snoop/providers/fondee.py`.
