---
name: nextjs-frontend
description: Use for any Next.js (App Router) page, layout, server/client component, data-fetching, or route-level work. Covers Server vs Client Component boundaries, next/dynamic, next/image, next/font, caching, and Vercel-specific performance rules. Does not apply to plain React/Vite/CRA projects — use reactjs-frontend for those.
---

# Next.js (App Router) Frontend

## Structure
- `page.tsx` and `layout.tsx` stay thin: compose components only, no inline logic or markup.
- Default to Server Components. Add `"use client"` only at the smallest possible leaf — never at page or layout level.
- Split heavy or rarely-used components (modals, video players, charts) into their own files so they can use `next/dynamic` later.

## Rendering strategy
- Push `"use client"` boundaries as deep as possible — wrap only the interactive part (e.g. a button's `onClick`), not the surrounding card/section.
- Use `next/dynamic` with `ssr: false` for anything not needed on first paint: modals, drawers, charts, video players, rich text editors, below-the-fold widgets.
- Wrap slow or async server components in `<Suspense>` with a lightweight fallback (the shared `Loading` component) so the rest of the page can stream in.

## Data fetching
- Fetch data at the server component level, not in `useEffect` on the client — client-side fetching adds a request waterfall (JS loads → hydrates → then fetches).
- Fetch sibling data in parallel (`Promise.all`), never sequential `await` chains, unless one fetch genuinely depends on another's result.
- Use Next.js caching (`fetch` with `cache`/`revalidate`, or `unstable_cache`) instead of re-fetching identical data on every request.

## Assets & fonts
- Always use `next/image` with explicit `width`/`height` or `fill` + a sized parent, so the browser reserves space and avoids layout shift.
- Use `next/font` so fonts self-host and avoid render-blocking external font requests.
- Prefer modern compressed formats (served automatically via `next/image`) over large raw PNG/JPEG assets.

## Bundle size
- Avoid barrel-file imports (`import { Button } from '@/components'`) — import directly from the component's file so bundlers can tree-shake unused code.
- Before adding any new dependency, check its bundle cost; prefer a small utility or native browser API over a heavy library for something trivial.
- Route-level code-splitting is automatic in Next.js — still manually split rarely-used heavy components per the rule above.

## Re-renders & client-side work
- Don't create new inline function/object/array literals as props to memoized or heavy child components on every render — hoist them or wrap in `useCallback`/`useMemo`.
- Use `React.memo` for pure presentational components that re-render often with unchanged props (e.g. list rows), not as a blanket default.
- Virtualize long lists/tables (100+ rows) instead of rendering every item.
- Debounce or throttle expensive handlers tied to high-frequency events (scroll, resize, input).
- Avoid layout thrashing: don't read layout properties (`offsetWidth`, `getBoundingClientRect`) and write styles in the same tick inside loops; batch reads then writes.

## General principle
Every new component should be evaluated for: does this add unnecessary client JS, unnecessary re-renders, or an unnecessary network request? Before it's considered finished.
