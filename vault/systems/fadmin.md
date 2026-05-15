---
type: system
slug: fadmin
owner: TBD (Fondee internal)
contract: internal admin UI
last_touched: 2026-05-15
domain: "[[growth]]"
---

# Fadmin

## What

Fondee interný back-office SW. CC (Customer Care) tým rieši klientske tickety priamo cez Fadmin — pohľad na konto klienta, manuálne korekcie, audit log, manual reward credit. Pri nových features sa BE zvyčajne musí postarať o exposnutie state/history v API alebo DB tabuľkách, aby ich Fadmin vedel zobraziť.

## Contract / interface

Interný SW, žiadny verejný API. CC tím má read + selective write access. Pri nových integráciách: BE doplní state expose (typicky DB view alebo dedikovaný admin endpoint) → Fadmin frontend doplní view.

[TODO: confirm — či má Fadmin vlastný owner / team alebo je súčasť BE responsibility]

## Owners

- **Internal:** TBD — overiť pri ďalšom kontakte
- **CC users:** support tím Fondee (denní používatelia)

## Gotchas

- Žiadne (zatiaľ — dopisovať pri inteagrácii s [[referral-akce]])

## Consumers

- [[referral-akce]] — referral state history + reason flags v stave `IN_PROGRESS` (chýba podpis / chýba vklad)
- (Ďalšie growth + ops initiativy historicky)

## Change log

- **2026-05-15** — Pridaný do vault. Pôvod: [[referral-akce]] /spar — CC observability nepotrebuje nový tool, Fadmin pokrýva use case ak BE doplní state expose.
