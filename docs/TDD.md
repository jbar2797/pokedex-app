# TCGViewer — Technical Design Document

**Version:** 1.0
**Status:** Active
**Last updated:** 2026-03-29

---

## System overview

TCGViewer is a client-side React application for MVP. There is no backend.
All data comes from the PokéTCG public API. The architecture is designed to
accommodate a backend, price data sources, and user accounts in post-MVP
without requiring a rewrite.

---

## Tech stack

| Layer        | Choice                         | Rationale                                                                                          |
| ------------ | ------------------------------ | -------------------------------------------------------------------------------------------------- |
| Framework    | React 18 + TypeScript          | Industry standard, strong ecosystem                                                                |
| Build tool   | Vite                           | Fast HMR, modern defaults                                                                          |
| Styling      | Tailwind CSS                   | Utility-first, no CSS file sprawl                                                                  |
| Routing      | React Router v6                | Standard, supports nested routes for future dashboard                                              |
| Server state | TanStack Query (React Query)   | Critical for post-MVP price data; handles caching, refetching, pagination                          |
| Client state | Zustand                        | Lightweight global state for favorites and filters; scales better than Context API for our roadmap |
| Testing      | Vitest + React Testing Library | Fast, Vite-native, industry standard                                                               |
| Linting      | ESLint + Prettier              | Enforced on CI                                                                                     |
| CI/CD        | GitHub Actions                 | Automated checks on every PR                                                                       |

---

## Primary data source

**PokéTCG API v2** — https://pokemontcg.io

- Free tier: 1000 requests/day without API key, higher with key
- Endpoints used:
  - `GET /v2/cards` — paginated card list with search/filter params
  - `GET /v2/cards/{id}` — single card detail
  - `GET /v2/sets` — all sets for filter dropdown
  - `GET /v2/types` — all types for filter dropdown

API key stored in `.env.local` as `VITE_POKETCG_API_KEY`.
All API calls go through `src/api/` — never called inline in components.

---

## Folder structure

```
src/
  api/
    cards.ts          ← all card-related API calls
    sets.ts           ← set API calls
    types.ts          ← type API calls
    client.ts         ← axios instance with base URL and API key header
  components/
    ui/               ← generic reusable components (Button, Badge, Spinner)
    cards/            ← card-specific components (CardGrid, CardTile, CardDetail)
    filters/          ← filter components (TypeFilter, SetFilter, SearchBar)
    layout/           ← Layout, Header, Footer
  hooks/
    useCards.ts       ← React Query hook for card list
    useCard.ts        ← React Query hook for single card
    useSets.ts        ← React Query hook for sets
    useFavorites.ts   ← Zustand-backed hook for favorites with localStorage sync
    useFilters.ts     ← Zustand-backed hook for active filter state
  pages/
    HomePage.tsx      ← card grid + search + filters
    CardDetailPage.tsx ← single card view
    FavoritesPage.tsx ← saved cards
  store/
    favoritesStore.ts ← Zustand store for favorites
    filtersStore.ts   ← Zustand store for active filters
  types/
    card.ts           ← Card, CardSet, CardType TypeScript interfaces
    api.ts            ← API response shape interfaces
  utils/
    localStorage.ts   ← typed localStorage read/write helpers
    formatters.ts     ← price formatting, date formatting utilities
  App.tsx
  main.tsx
```

---

## Routing structure

```
/                     → HomePage (card grid)
/card/:id             → CardDetailPage
/favorites            → FavoritesPage
```

Post-MVP additions will include:

```
/market               → Market index dashboard
/movers               → Top movers / biggest losers
/collection           → User collection manager
```

---

## State management

Two categories of state:

**Server state** (TanStack Query) — anything that comes from an API.
Cards, sets, types, eventually price data. React Query handles caching,
background refetching, loading and error states.

**Client state** (Zustand) — anything that lives on the client only.
Active filters, favorites list. Zustand stores are small and focused.

Do not use `useState` for things that should be in a store. Do not use
a store for things that should be React Query.

---

## Favorites implementation (MVP)

Favorites are card IDs stored as a `Set<string>` in Zustand, with a
localStorage sync middleware. On app load, the store hydrates from
localStorage. On any change, it writes back immediately.

When user accounts ship post-MVP, the `useFavorites` hook interface stays
identical — only the backing store changes from localStorage to an API call.
Components never know the difference.

---

## Error handling strategy

- API errors surface via React Query's `isError` / `error` state
- Every page that fetches data must handle three states: loading, error, success
- Loading state: skeleton UI (not spinners)
- Error state: inline error message with retry button
- Empty state: illustrated empty state with CTA

---

## Performance considerations

- Card images are lazy loaded with `loading="lazy"`
- Card grid is paginated — 20 cards per page, no infinite scroll for MVP
- React Query caches responses for 5 minutes by default
- Debounce search input at 300ms before firing API call

---

## Architecture decisions log

See `docs/DECISIONS.md`

---

## Testing strategy

- Unit tests for all utility functions and custom hooks
- Component tests for interactive components (SearchBar, TypeFilter, CardTile)
- No e2e tests for MVP — added post-MVP with Playwright
- Minimum coverage target: 70% on `src/utils/` and `src/hooks/`
