# Research Summary — Toastmasters Pathways Reference App

**Synthesized:** 2026-05-27
**Overall Confidence:** HIGH

---

## Executive Summary

A mobile-first, single-file HTML reference app for Toastmasters members to look up Pathways curriculum (8 paths × 5 levels × ~20 projects) during club meetings. FTH hosting eliminates all frameworks, build tools, and multi-file architectures — the entire deliverable is one `.html` file with embedded JSON data. Vanilla ES2020, CSS custom properties, and native browser APIs cover every requirement without a single dependency.

Two architectural decisions that everything else depends on: (1) embed curriculum data as `<script type="application/json">` and render only the current path × level on demand; (2) use CSS Grid with `100dvh` for the fixed-header/scrollable-content layout. Both must be made in Phase 1.

---

## Stack

- **HTML5 single file** — uploaded via FTH File Management (never the CMS editor, which strips `<script>` tags)
- **`<script type="application/json">` data embedding** — MDN-endorsed inert data block; parsed once on load into a `PATHWAYS` constant
- **CSS Grid + `100dvh`** — `grid-template-rows: auto auto auto 1fr`; `dvh` avoids the iOS Safari `100vh` address-bar bug
- **Vanilla ES2020** — IntersectionObserver, `scrollTo()`, `.closest()`; no libraries
- **CSS custom properties** — two-tier token system (brand values → semantic intent); locked palette: Loyal Blue `#004165`, True Maroon `#772432`, Happy Yellow `#F2DF74`
- **Google Fonts** — Montserrat (headline) + Source Sans 3 (body), 400+700 only, `display=swap`, preconnect links

---

## Table Stakes (v1 Must-Haves)

| Feature | Notes |
|---------|-------|
| Path selector (all 8 paths) | Native `<select>` — free mobile picker UX |
| Level tab strip (L1–L5) with horizontal scroll | `scroll-snap-type: x proximity`; test at 320px |
| Project cards: name, time, purpose, Required/Elective badge | Badge visually distinct — not text-only |
| Level completion requirement text | Above the card list; never a tooltip |
| Enhancement section per level | Separate schema + separate render function |
| Scroll-to-top FAB | IntersectionObserver on sentinel; `aria-label="Back to top"` |
| Mobile-first responsive layout | Single-column on phones; 768px breakpoint |
| Toastmasters brand compliance | Locked palette, Montserrat + Source Sans 3 |
| Full WCAG 2.1 AA accessibility | ARIA tab pattern, visible focus indicators, 4.5:1 contrast |
| Keyboard navigation | Arrow keys for tabs; `<select>` is free |

**Defer to v2:** Shareable URL hash routing, sticky tabs, R/E count summary, print CSS.
**Defer indefinitely:** Search/filter, dark mode, progress tracking, PWA/offline.

---

## Architecture

- **Data:** Array of path objects with `id` slug, each with `levels[]` containing `completionRequirement`, `enhancement` (object with `roles[]`), and `projects[]`
- **Render:** On-demand only — build HTML string via template literals + `map().join('')`, single `innerHTML` assignment per navigation
- **State:** Plain singleton `{ selectedPath, selectedLevel }` → `setState()` → `render()`; no Pub/Sub
- **Events:** `.closest()` delegation on tab container; direct `change` listener on `<select>`
- **CSS active state:** `[aria-selected="true"]` drives styling — no separate `.active` class
- **Enhancement:** `level.enhancement` is a separate field, never mixed into `level.projects[]`

---

## Watch Out For (Priority Order)

1. **FTH strips `<script>` from CMS editor** — upload as static file; test with throwaway file FIRST before writing real code
2. **Rendering all 800 nodes on load** — render-on-demand only; wrong choice = full rewrite
3. **`100vh` breaks iOS Safari layout** — use `100dvh`; test on real iOS hardware
4. **Required/Elective classification errors** — build validation script before transcribing any data
5. **Incomplete ARIA tab pattern** — follow W3C APG verbatim: `tablist`, `tab`, `tabpanel`, `aria-selected`, `tabindex`, arrow-key JS

**Also:** Happy Yellow on light backgrounds fails WCAG (1.4:1); tab items need `flex-shrink: 0; white-space: nowrap`; DOM IDs must include path slug + level index.

---

## Build Order

### Phase 1 — Foundation
1. FTH deploy test (throwaway file with `<script>`) — GATE
2. JSON schema design (slug format, `enhancement` structure, `dataVersion`, time format canonical)
3. HTML shell + full CSS (Grid, `100dvh`, safe areas, brand tokens, 320px tab test, font preconnect)
4. PATHWAYS constant with 1–2 paths (schema validation)
5. Render pipeline (`buildProjectCardHTML`, `buildEnhancementHTML`, `buildLevelHTML`, `render()`)
6. Events + full ARIA tab pattern + arrow-key keyboard navigation
7. FAB (IntersectionObserver, iOS safe-area bottom)
8. QA: real iOS, 320px, keyboard-only, screen reader smoke test

**Exit criteria:** Navigation works for 1–2 paths, ARIA correct, layout holds on iOS, FTH deployment confirmed.

### Phase 2 — Data
1. Build validation script (R/E counts, time format, slug linter, smart quote detector)
2. Transcribe all 8 paths × 5 levels from PDFs
3. Cross-reference against official Pathways Matrix PDF
4. Spot-check + full 40-combination render QA

### Phase 3 — Polish
1. URL hash routing, sticky tabs, R/E count summary, print CSS
2. File size audit (minify JSON, target < 500KB)
3. Full WCAG 2.1 AA audit

---

## Open Questions

- FTH CSP behavior on uploaded static files — unknown; Phase 1 throwaway test is the gate
- Whether the two Vintage Paths (April 2026) have the same October 2025 enhancements — confirmed NO (vintage paths are enhancement-exempt)
- Exact R/E completion counts per level per path — needed for Phase 2 validation script
