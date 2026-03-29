# Architecture Decision Records

---

## ADR-001 — No backend for MVP

**Date:** 2026-03-29
**Status:** Accepted

**Decision:** Ship MVP as a fully client-side app with no backend.

**Rationale:** Favorites can use localStorage. Card data comes from
PokéTCG public API. A backend adds deployment complexity with no user
value at this stage.

**Consequences:** When user accounts and price tracking ship, a backend
will be introduced. The API layer in `src/api/` is designed to be
retargeted from the public API to our own backend with minimal component
changes.

---

## ADR-002 — Zustand over Context API

**Date:** 2026-03-29
**Status:** Accepted

**Decision:** Use Zustand for global client state rather than React Context.

**Rationale:** The post-MVP roadmap includes a collection manager, market
dashboard, and user portfolios. Context API causes re-render cascades at
that scale. Zustand is lightweight (no provider wrapping), has built-in
localStorage middleware, and devtools support.

---

## ADR-003 — TanStack Query from day one

**Date:** 2026-03-29
**Status:** Accepted

**Decision:** Use TanStack Query for all server state, even though MVP
only has one data source.

**Rationale:** Post-MVP will have multiple async data sources: PokéTCG API,
price data API, user collection API. TanStack Query's caching, background
sync, and pagination support will be essential. Retrofitting it later is
painful. Adding it now costs nothing.
