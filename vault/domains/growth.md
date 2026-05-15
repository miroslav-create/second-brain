---
type: domain
slug: growth
owner: PO / Growth team
---

# Growth

## Mission

Akvizícia + retencia klientov Fondee. Marketing, referral, conversion funnel, signup flow optimization, A/B testovanie. Cross-cutting cez klientsku zónu, mobile app, signup flow, batch reward processy.

## Active initiatives

- [[referral-akce]] — redesign referral programu (2026-05)

## Systems in scope

- [[fadmin]] — CC tool, používa sa pre manuálne korekcie referral state, audit log

(Ďalšie systems pribudnú pri dev kickoff: klientska-zona, fondee-app, referral-batch-service.)

## Decisions / patterns

- **2026-05-15** — Cross-country odmeny pripisujú sa v mene účtu príjemcu, nie podľa krajiny doporučujúceho. Eliminuje FX komplikácie v batch processe. (Pôvod: [[referral-akce]])
- **2026-05-15** — Pre features s display-of-third-party-data uplatňovať hard rule GDPR: žiadne PII tretích strán pred explicit consent event (`registration_completed` / podobné). (Pôvod: [[referral-akce]] Q8.)

## Notes

Growth initiativy často zasahujú do regulovaných oblastí (GDPR, finančný audit). Pri každej novej feature: skontrolovať legal basis pre spracovanie a uloženie dát tretích strán pred kickoff.
