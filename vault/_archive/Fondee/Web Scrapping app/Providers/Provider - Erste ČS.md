---
type: provider
tags:
  - provider/bank
aliases:
  - Erste
  - Česká spořitelna
  - ČS
  - erste
---

# Provider — Erste / ČS

> Česká spořitelna (Erste Group). Pro Fund Snoop: transaction fee 0,35 %; mgmt potlačeno (skutečná data v KIID PDF).

Patří do kategorie [[ProviderType]] = `bank`.
Vztah k Fondee: [[Overlap]] = `adjacent`.

## Produkty (ProductType)

- [[FeeSchedule|FeeSchedule]] — `transaction_fee = 0,35 %`
- [[ProductType - Mutual fund|Podílové fondy]] — distribuované fondy (mgmt v KIID PDF — neextrahováno)

## Pozice na trhu

Phase B původně vracela 8,47 (performance figure misread jako mgmt). Phase D fix: anchor na `poplatek za správu`; safety cap odhodí hodnoty >5 % p.a.; `transaction_fee=0,35 %` z homepage.

## Scraper

- Modul: `fund_snoop/providers/erste.py`
- Vzor: text-extraction + anchor `poplatek za správu` + safety cap
- Phase: D (2026-05-10 fix)

## Vazby

- Koncept: [[Provider]] · [[ProviderType]] = bank · [[FeeSchedule]]
- Produkty: [[ProductType - Mutual fund]]
- Narrative: [[About]] § Scope · [[Status]] § Live data → Erste/ČS · [[Known issues]] § Closed in Phase E/F

## Live state

Autoritativní: `HANDOVER.md` § "Scraper accuracy" → **Erste / ČS**. Mirror v [[Status]] § Live data.

## Source of truth

Repo `HANDOVER.md` + `fund_snoop/providers/erste.py`.
