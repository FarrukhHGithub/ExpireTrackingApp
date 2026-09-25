---
trigger: always_on
---

# Frontend Rules (Universal)

Applies regardless of framework. Framework-specific rendering/data-fetching/asset rules live in `nextjs-frontend` or `reactjs-frontend` skills and load automatically for that kind of work — do not duplicate them here.

## Structure & Composition
- Every UI element must be responsive: mobile, tablet, desktop. No desktop-only work.
- One component per folder:
  ```
  components/
    Button/
      Button.tsx
      Button.module.css
  ```
- **120-line hard limit per file** — counts imports, types, comments, JSX. Check before marking a component "done"; split immediately if over, don't defer.
  - Split into sub-components nested inside the same folder (e.g. `Button/ButtonIcon.tsx`), never promoted to top-level `components/`.
  - A heavy import list (8+ modules, several unrelated concerns) is a split signal even under 120 lines.
- Reuse existing components (Button, Table, Loading, etc.) — don't create new/duplicate variants unless explicitly requested.
- Styling: Vanilla CSS Modules (`*.module.css`) per component. No other styling approach.

## Scaling / Zoom Correctness
- At 100% browser zoom, components must render at their intended real-world size. Test every new component at 100% zoom, not just editor/browser default zoom.
- Never hardcode large fixed `px` values for font-size, padding, or width/height on root-level containers — use `rem`/`em`.
- For fluid sizing, use `clamp(min, preferred, max)` instead of raw `vw` alone.
- Don't rely on browser zoom or OS scaling to "fix" a layout — fix the CSS.
- Check common dev monitor widths (1440px, 1920px, 2560px) in addition to standard breakpoints.
- Any container holding text or cards needs a `max-width` (rem or a design-token value) so it doesn't stretch on wide viewports.
