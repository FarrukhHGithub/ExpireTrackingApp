---
name: reactjs-frontend
description: Use for any plain React.js work with no Next.js server/App Router layer — Vite, CRA, or an embedded widget/SPA. Covers client-side code-splitting, data fetching without a server layer, manual image/font optimization, and virtualization. Does not apply to Next.js projects — use nextjs-frontend for those.
---

# React.js (non-Next) Frontend

Use this instead of `nextjs-frontend` when the project has no Next.js server or App Router layer.

## Code-splitting & lazy loading
- Use `React.lazy()` + `<Suspense fallback={<Loading />}>` for modals, drawers, charts, video players, and any below-the-fold or route-level component — the manual equivalent of `next/dynamic`.
- Split routes with your router's lazy-loading (e.g. React Router's `lazy()` route objects) so each route ships its own chunk.

## Data fetching
- Fetch through a dedicated data layer (React Query, SWR, or a fetch hook) rather than ad-hoc `useEffect` + `useState` scattered across components.
- Fetch sibling data in parallel (`Promise.all` or parallel queries), never sequential `await` chains, unless one genuinely depends on another's result.
- Cache and dedupe requests via your data-fetching library's cache (React Query's `staleTime`, SWR's dedup) instead of re-fetching identical data on every mount.

## Assets & fonts
- There's no `next/image` equivalent — use `<img>` with explicit `width`/`height` attributes (or CSS `aspect-ratio`) so the browser reserves layout space.
- Self-host fonts (`@font-face` + `<link rel="preload">`) rather than a render-blocking third-party stylesheet link — the manual equivalent of `next/font`.
- Prefer modern compressed formats (WebP/AVIF) and serve responsive images via `srcset`/`sizes` manually.

## Bundle size
- Avoid barrel-file imports — import directly from the component's file so the bundler (Vite/webpack) can tree-shake unused code.
- Check a dependency's bundle cost before adding it; prefer a small utility or native browser API over a heavy library for something trivial.
- Manually code-split with `React.lazy` per route or heavy component — this isn't automatic without a meta-framework.

## Re-renders & client-side work
- Don't pass new inline function/object/array literals as props to memoized or heavy child components on every render — hoist them or wrap in `useCallback`/`useMemo`.
- Use `React.memo` for pure presentational components that re-render often with unchanged props (e.g. list rows), not as a blanket default.
- Virtualize long lists/tables (100+ rows) with a windowing library (`react-window`/`react-virtual`) instead of rendering every item.
- Debounce or throttle expensive handlers tied to high-frequency events (scroll, resize, input).
- Avoid layout thrashing: don't read layout properties (`offsetWidth`, `getBoundingClientRect`) and write styles in the same tick inside loops; batch reads then writes.

## General principle
Every new component should be evaluated for: does this add unnecessary client JS, unnecessary re-renders, or an unnecessary network request? Before it's considered finished.
