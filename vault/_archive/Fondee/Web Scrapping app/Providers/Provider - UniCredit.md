---
type: provider
tags:
  - provider/bank
aliases:
  - UniCredit
  - UniCredit Bank
  - unicredit
---

# Provider — UniCredit Bank

> Italská skupina, ČR pobočka. Pro Fund Snoop: SAVE účet 2,0 % — **WAF fallback** (BrightCloud blokuje headless Chromium).

Patří do kategorie [[ProviderType]] = `bank`.
Vztah k Fondee: [[Overlap]] = `adjacent`.

## Produkty (ProductType)

- [[ProductType - Savings account|SAVE účet]] — **2,0 %** (hardcoded fallback, flagged v `raw_notes`)

## Pozice na trhu

Phase E (2026-05-10). BrightCloud anti-bot WAF blokuje headless Chromium → scraper detekuje challenge text a vrací hardcoded hodnotu. Reálná sazba se nečte. Pro odblokování by bylo třeba reziduální UA, stealth plugin nebo scrapingbee fallback (zatím deferred — viz [[Pickup checklist]] § 5).

## Scraper

- Modul: `fund_snoop/providers/unicredit.py`
- Vzor: WAF fallback — detect challenge, return hardcoded value, mark v `raw_notes`
- Reference scraper pro WAF-fallback pattern
- Phase: E

## Vazby

- Koncept: [[Provider]] · [[ProviderType]] = bank · [[FeeSchedule]] (`raw_notes`)
- Produkty: [[ProductType - Savings account]]
- Narrative: [[About]] § Scope · [[Status]] § Live data → UniCredit · [[Known issues]] § UniCredit WAF fallback · [[Pickup checklist]] § Reference scrapers → WAF fallback

## Live state

Autoritativní: `HANDOVER.md` § "Scraper accuracy" → **UniCredit**. Mirror v [[Status]] § Live data.

## Source of truth

Repo `HANDOVER.md` + `fund_snoop/providers/unicredit.py`.
