# TCGViewer — Product Requirements Document

**Version:** 1.0
**Status:** Active
**Last updated:** 2026-03-29

---

## Problem statement

The Pokémon TCG market has matured into a legitimate alternative asset class,
but the tools available to collectors and investors are fragmented, slow,
ad-saturated, and treat cards purely as a hobby rather than as financial
instruments. TCGViewer fills the gap between a card browser and a market
intelligence platform.

---

## Target users

**MVP:** Pokémon TCG collectors who want a fast, clean way to browse the card
catalog and find specific cards.

**Post-MVP:** TCG investors and collectors who want market data, portfolio
tracking, and buy/sell signals alongside card browsing.

---

## MVP scope (Sprint 1–2)

These features ship before anything else. No exceptions.

| Feature          | Description                                                         |
| ---------------- | ------------------------------------------------------------------- |
| Card browsing    | Paginated grid of all cards from PokéTCG API                        |
| Search           | Search cards by name, debounced at 300ms                            |
| Filter by type   | Filter grid by Pokémon type (Fire, Water, etc.)                     |
| Filter by set    | Filter grid by card set                                             |
| Card detail page | Full card view: image, stats, set info, market price (display only) |
| Favorites        | Save/unsave cards, persisted to localStorage                        |

---

## Post-MVP roadmap (in priority order)

1. **Collection manager** — track which cards you own, quantity, condition,
   purchase price
2. **Price tracking** — historical price charts per card, sourced from
   TCGPlayer API or scraping
3. **Market index** — S&P 500-style aggregate index for the Pokémon TCG market,
   tracking overall market cap and movement
4. **Top movers / biggest losers** — daily and weekly price movement leaderboard
5. **Buy / hold / sell indicators** — algorithmic signals based on price
   momentum, rarity, and market sentiment
6. **Price history charts** — candlestick or line charts per card
7. **Most popular cards** — trending by search volume and watchlist adds
8. **Affiliate buy links** — card purchase links to TCGPlayer, eBay, GameStop
   with affiliate tracking
9. **Affiliate partner integrations** — GameStop Power Packs, booster box
   retailers, PSA grading referrals

---

## Success metrics (MVP)

- Card search returns results in under 500ms p95
- First contentful paint under 2 seconds on a standard connection
- App is fully functional on mobile viewport (375px minimum)
- No authentication required for MVP
- Favorites persist across browser sessions

---

## Explicit non-goals (MVP)

- No user accounts or authentication
- No backend or database
- No real-time price data (display static market price from API if available)
- No buying or selling functionality
- No deck builder
- No social features

---

## Open questions

1. TCGPlayer API requires approval — do we use their official API or scrape
   price data for the price tracking feature? Decision needed before Sprint 3.
2. Market index methodology — what basket of cards constitutes the index?
   Needs product definition before Sprint 4.
3. Affiliate program terms for TCGPlayer and GameStop — need to apply and get
   approved before affiliate links can go live.
