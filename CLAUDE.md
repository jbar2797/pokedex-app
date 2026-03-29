# TCGViewer — Claude Instructions

Read this entire file before writing any code.

---

## What this project is

TCGViewer is a Pokémon TCG card browser and market intelligence platform.
MVP is a client-side React app with no backend. Users browse, search, and
filter cards using the PokéTCG public API and save favorites to localStorage.

Post-MVP adds price tracking, market index, buy/hold/sell signals,
collection management, and affiliate monetization.

---

## Tech stack

- React 18 + TypeScript (strict mode)
- Vite
- Tailwind CSS (utility classes only — no custom CSS files)
- React Router v6
- TanStack Query v5 (React Query)
- Zustand v4
- Vitest + React Testing Library
- ESLint + Prettier

---

## Folder structure — follow this exactly

```
src/
  api/           ← ALL API calls live here. Never call fetch/axios in a component.
  components/
    ui/           ← generic, reusable (Button, Badge, Spinner, Skeleton)
    cards/        ← card-specific components
    filters/      ← filter and search components
    layout/       ← Layout, Header, Footer
  hooks/          ← custom React hooks only. No JSX in this folder.
  pages/          ← route-level components only. Thin — logic lives in hooks.
  store/          ← Zustand stores only
  types/          ← TypeScript interfaces only. No logic.
  utils/          ← pure functions only. No React.
```

---

## Code conventions — non-negotiable

- TypeScript strict mode. Zero `any` types. If you don't know the type, ask.
- Named exports only. No default exports anywhere.
- Every component gets a `.test.tsx` file in the same folder.
- All API calls go through `src/api/`. Never inline.
- Pages are thin. All logic lives in hooks.
- Use TanStack Query for all async data. No `useEffect` + `useState` for fetching.
- Use Zustand for all shared client state. No prop drilling.
- Tailwind classes only. No inline styles. No separate CSS files.
- Skeleton loading states, not spinners.
- Every data-fetching component handles: loading, error, empty, and success states.

---

## Naming conventions

- Components: PascalCase (`CardTile.tsx`)
- Hooks: camelCase prefixed with `use` (`useCards.ts`)
- Stores: camelCase suffixed with `Store` (`favoritesStore.ts`)
- API files: camelCase noun (`cards.ts`, `sets.ts`)
- Types: PascalCase interfaces (`Card`, `CardSet`)
- Utils: camelCase verb (`formatPrice.ts`)

---

## Git conventions

- Branch from main: `feat/description`, `fix/description`, `refactor/description`
- Commit format: `feat: add search bar with debounce`
- Never commit directly to main
- Every feature gets a PR

---

## Current sprint

**Sprint 1 goal:** Core card browsing experience

Tickets in scope:

1. Project scaffolding — install deps, configure Tailwind, set up router
2. API client — axios instance with API key header
3. Cards API — `getCards()` with search and filter params
4. Sets API — `getSets()` for filter dropdown
5. Types API — `getTypes()` for filter dropdown
6. `useCards` hook — TanStack Query wrapper
7. `CardTile` component — card image, name, type badges
8. `CardGrid` component — responsive grid with pagination
9. `SearchBar` component — debounced input
10. `TypeFilter` component — type selector
11. `SetFilter` component — set selector
12. `HomePage` — composes grid + filters + search
13. `CardDetailPage` — full card view
14. `useFavorites` hook — localStorage-backed Zustand store
15. `FavoritesPage` — saved cards grid

---

## Environment variables

```
VITE_POKETCG_API_KEY=     ← PokéTCG API key
```

---

## Open questions

- PokéTCG API rate limits at free tier — monitor and add caching layer
  if needed before Sprint 2.
