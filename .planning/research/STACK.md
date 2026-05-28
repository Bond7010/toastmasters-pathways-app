# Technology Stack — Toastmasters Pathways Reference App

**Researched:** 2026-05-27
**Overall Confidence:** HIGH — all recommendations are native web platform features, widely supported since 2022 or earlier.

---

## Recommended Stack

| Layer | Choice | Why |
|-------|--------|-----|
| Document | HTML5 single file | FTH constraint, zero build pipeline |
| Data embedding | `<script type="application/json">` | MDN-endorsed data block, inert to browser |
| Page layout | CSS Grid (`grid-template-rows`) | Fixed header rows + 1fr scrollable content |
| Component layout | CSS Flexbox | Single-axis alignment within cards/strips |
| Responsive | 768px breakpoint, mobile-first | One breakpoint covers all cases |
| Touch scroll | `overflow-y: auto` + `overscroll-behavior: contain` | Native momentum; -webkit prefix deprecated |
| Tab strip scroll | `overflow-x: auto` + `scroll-snap-type: x proximity` | Baseline Widely Available; no JS |
| Path selector | Native `<select>` + CSS wrapper | Mobile platform picker UX, zero JS |
| FAB show/hide | `IntersectionObserver` on sentinel | ~23% less scripting vs scroll listener |
| FAB scroll action | `scrollTo({ behavior: 'smooth' })` | Native API, no library |
| Fonts | Google Fonts @import (Latin, 400+700) | No file size penalty; audience is online |
| Design tokens | CSS custom properties on `:root` | Native, no build step, runtime-modifiable |
| JavaScript | ES2020 vanilla | No transpilation; modern-browser audience |

No libraries. No frameworks. No build pipeline. The entire deliverable is a single `.html` file.

---

## Decision Details

### 1. Data Embedding — `<script type="application/json">`

MDN-endorsed pattern. Browser treats the block as inert data, not executable code.

```html
<script type="application/json" id="pathways-data">
[
  {
    "id": "dynamic-leadership",
    "name": "Dynamic Leadership",
    "levels": [ ... ]
  }
]
</script>

<script>
  const PATHWAYS = JSON.parse(document.getElementById('pathways-data').text);
</script>
```

**Critical:** The only string requiring escaping inside `<script>` is `</script>` (any case). Escape as `<\/script>` in JSON values. No other escaping needed.

**What NOT to use:**
- `const DATA = { ... }` bare JS literal — conflates data and code
- External `.json` via `fetch()` — breaks single-file, CORS issues on FTH
- Base64-encoded JSON — unnecessary obscuring of data

---

### 2. Page Layout — CSS Grid

```css
body {
  display: grid;
  grid-template-rows: auto auto auto 1fr;
  /* top-bar | path-selector | tab-strip | content-area */
  height: 100dvh;   /* dvh = dynamic viewport, avoids iOS address-bar bug */
  overflow: hidden; /* body never scrolls; only content-area scrolls */
}
```

**Why `100dvh` not `100vh`:** iOS Safari intentionally calculates `100vh` as the full height with address bar collapsed. Content overflows or clips under browser chrome. `dvh` tracks actual visible viewport. Use `dvh` universally for this layout.

---

### 3. Touch Scrolling

```css
.content-area {
  overflow-y: auto;
  overscroll-behavior: contain;
}
```

**DO NOT use `-webkit-overflow-scrolling: touch`** — deprecated since iOS 13 (2019). May cause GPU memory issues. Modern iOS applies momentum scrolling natively to all `overflow: auto` elements.

For the tab strip:
```css
.tab-strip {
  overflow-x: auto;
  overscroll-behavior-x: contain;
  scroll-snap-type: x proximity;
  scrollbar-width: none;
}
.tab-strip::-webkit-scrollbar { display: none; }
```

---

### 4. Level Tab Strip

- `scroll-snap-type: x proximity` (NOT mandatory — too aggressive for tabs)
- Each tab: `flex-shrink: 0; scroll-snap-align: start; white-space: nowrap`
- Minimum touch target: 44px height (`padding: 0.75rem 1rem`)
- Active state driven by `aria-selected` attribute (CSS: `[aria-selected="true"] { border-bottom: 3px solid var(--tm-loyal-blue); }`)
- Test at **320px viewport width** — tabs must scroll, not wrap

---

### 5. Path Selector — Native `<select>`

```css
.path-selector-wrapper {
  position: relative;
  background: var(--tm-true-maroon);
  padding: 0.5rem 1rem;
}
.path-selector-wrapper::after {
  content: "▾";
  position: absolute;
  right: 1rem;
  top: 50%;
  transform: translateY(-50%);
  color: white;
  pointer-events: none;
}
.path-selector {
  appearance: none;
  -webkit-appearance: none;
  width: 100%;
  background: transparent;
  border: none;
  color: white;
  font-family: var(--tm-font-headline);
  font-size: 1rem;
}
```

On mobile, native `<select>` renders as platform picker (iOS wheel / Android modal) — correct UX for 8 choices. Zero ARIA complexity vs. custom dropdown.

---

### 6. Floating Action Button (Scroll-to-Top)

```javascript
const sentinel = document.getElementById('scroll-sentinel'); // div at top of content area
const fab = document.getElementById('fab-scroll-top');
const contentArea = document.getElementById('content-area');

const observer = new IntersectionObserver(([entry]) => {
  fab.hidden = entry.isIntersecting;
}, { root: contentArea, threshold: 0 });
observer.observe(sentinel);

fab.addEventListener('click', () => {
  contentArea.scrollTo({ top: 0, behavior: 'smooth' });
});
```

```css
#fab-scroll-top {
  position: fixed;
  bottom: calc(1.5rem + env(safe-area-inset-bottom)); /* iOS home indicator */
  right: 1.5rem;
  width: 3rem;
  height: 3rem;
  border-radius: 50%;
  background: var(--tm-loyal-blue);
  color: white;
  border: none;
  cursor: pointer;
  box-shadow: 0 4px 8px rgba(0,0,0,0.3);
  z-index: 100;
}
#fab-scroll-top[hidden] { display: none; }
```

Add `<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">` for `env()` to work.

---

### 7. Fonts

```css
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@400;700&family=Source+Sans+3:wght@400;600&display=swap&subset=latin');
```

Add preconnect to avoid DNS lookup delay:
```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
```

Load only: Montserrat 400+700, Source Sans 3 400+600. No other weights needed.

**If offline is required later:** Use base64 woff2 embedding (~106KB per variant added to file). For now, Google Fonts CDN with `display=swap` is correct — audience is online.

---

### 8. Design Token System

```css
:root {
  /* Tier 1 — Brand values */
  --tm-loyal-blue:   #004165;
  --tm-true-maroon:  #772432;
  --tm-cool-gray:    #A9B2B1;
  --tm-happy-yellow: #F2DF74;
  --tm-white:        #FFFFFF;

  --tm-font-headline: 'Montserrat', 'Gotham', Arial, sans-serif;
  --tm-font-body:     'Source Sans 3', 'Segoe UI', Arial, sans-serif;

  /* Tier 2 — Semantic intent */
  --color-header-bg:           var(--tm-loyal-blue);
  --color-header-text:         var(--tm-white);
  --color-selector-bg:         var(--tm-true-maroon);
  --color-selector-text:       var(--tm-white);
  --color-tab-active:          var(--tm-loyal-blue);
  --color-badge-required-bg:   var(--tm-loyal-blue);
  --color-badge-required-text: var(--tm-white);
  --color-badge-elective-bg:   var(--tm-happy-yellow);
  --color-badge-elective-text: #000000; /* Black on yellow per brand rules */

  /* 4px base spacing unit */
  --space-1: 0.25rem;
  --space-2: 0.5rem;
  --space-3: 0.75rem;
  --space-4: 1rem;
  --space-6: 1.5rem;
  --space-8: 2rem;
}
```

---

## What NOT to Use

| Technology | Reason |
|------------|--------|
| React / Vue / Svelte | Require build step; CDN adds 40–200 KB |
| Bootstrap / Tailwind | CDN required or build step |
| jQuery | Unnecessary; all features in ES2020 |
| `-webkit-overflow-scrolling: touch` | Deprecated since iOS 13 |
| Custom JS scroll handlers | Always inferior to native on mobile |
| SCSS / SASS | Requires build step; CSS custom properties sufficient |
| External JSON via fetch() | Breaks single-file; CORS on FTH |
| Base64 fonts (by default) | ~106 KB added; unjustified unless offline |
| `scroll-snap-type: x mandatory` | Too aggressive for tabs; use `proximity` |
| `window.scrollY` scroll events | Every-pixel firing; use IntersectionObserver |

---

## Browser Target

iOS Safari 14+ (2020), Android Chrome 90+ (2021), Desktop Chrome/Firefox/Edge current.
All techniques are Baseline Widely Available. No polyfills required.
