# Domain Pitfalls — Toastmasters Pathways Reference App

**Researched:** 2026-05-27
**Confidence:** HIGH for mobile/ARIA/font/brand pitfalls; MEDIUM for FTH-specific behavior (forum evidence, no official doc)

---

## Critical Pitfalls

### C-1: FTH Page Editor Strips Inline `<script>` Tags

**What goes wrong:** FTH's CMS WYSIWYG editor sanitizes `<script>` blocks out of page content. A documented forum case shows a club website freezing after adding JavaScript through the HTML editor.

**Prevention:**
- Upload the HTML as a **static file via FTH's File Management tool**, not through the page content editor
- **Test in Phase 1 first** with a throwaway 10-line file containing `<script>alert('ok')</script>` before investing in real content
- Open question: whether FTH serves a CSP header blocking inline scripts in uploaded files — the throwaway test will reveal this

**Phase:** Phase 1 — test the upload mechanism before any real work.

---

### C-2: Rendering All ~800 Project Nodes on Page Load

**What goes wrong:** Creating DOM nodes for all 8 paths × 5 levels × ~20 projects on load causes 2–4 second white screen on mid-range Android. Lighthouse warns at >800 DOM nodes.

**Prevention:**
- Render only the currently selected path + level
- On navigation: `contentArea.innerHTML = ''` then append a `DocumentFragment` for the new selection
- Full JSON data lives in memory; only the rendered DOM stays minimal

**Phase:** Phase 1 (architecture). Wrong choice here means a full refactor later.

---

### C-3: `100vh` Breaks Fixed-Header Layout on iOS Safari

**What goes wrong:** iOS Safari calculates `100vh` as full height with address bar hidden. Content area overflows or clips under browser chrome. Apple marked this "not a bug" in 2015; it persists.

**Prevention:**
- Use `100dvh` instead of `100vh` on the app container
- Scrollable content region: `flex: 1; overflow-y: auto; min-height: 0` — not a calculated height
- Add `env(safe-area-inset-bottom)` to FAB and content area bottom padding
- `<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">`
- **Test on real iOS hardware**, not only Chrome DevTools

**Phase:** Phase 1 (layout). Build correctly from the start.

---

### C-4: Required/Elective Classification Errors in Transcribed Data

**What goes wrong:** PDF layout uses visual cues (shading, column position) that are easy to misread. R/E patterns differ across levels (Level 3/4 have more electives). Errors are trust-destroying — member marks wrong project, discovers level isn't recognized.

**Prevention:**
- Cross-reference against the official "Pathways Path and Project Matrix" PDF (shows R/E for all paths in one grid)
- Programmatically validate required + elective counts per level against known totals before embedding
- Strict enum in JSON: `"type": "required" | "elective"` — never freeform string
- Second person spot-checks 2–3 levels per path from source PDF

**Phase:** Phase 2 (data entry). Build the validation script before transcribing.

---

### C-5: Time Requirement Values Transcribed Inconsistently

**What goes wrong:** PDFs state time in varying formats: "5 to 7 minutes," "5-7 minutes," "five to seven minutes." Inconsistent transcription produces unprofessional UI and breaks any future filtering.

**Prevention:**
- Canonical format established before transcription: `"5–7 min"` (en-dash, abbreviated unit)
- Post-transcription regex audit: flag any time field not matching `^\d+[–-]\d+ min$`
- Known universal projects: Ice Breaker always 4–6 min; Evaluation always 2–3 min

**Phase:** Phase 2 (data entry). Define the format alongside the JSON schema in Phase 1.

---

## Moderate Pitfalls

### M-1: Happy Yellow on Light Backgrounds Fails WCAG Contrast

**Verified contrast ratios:**
- White on Loyal Blue (#004165): ~8.5:1 — **PASSES AA and AAA**
- White on True Maroon (#772432): ~5.9:1 — **PASSES AA**
- Black on Happy Yellow (#F2DF74): ~8.1:1 — **PASSES AA and AAA**
- Happy Yellow on white: ~1.4:1 — **FAILS** (never use)
- Happy Yellow on Cool Gray (#A9B2B1): ~1.6:1 — **FAILS** (never use)

**Rule:** Happy Yellow = background only (on light surfaces) or text only on dark (Loyal Blue/Maroon) backgrounds. For "Elective" badge: dark text on yellow background.

**Phase:** Phase 1 (design system). Lock allowed pairings before building any component.

---

### M-2: Tab Strip Wraps Instead of Scrolling at 320px

**Prevention:**
- Tab container: `display: flex; overflow-x: auto; scroll-snap-type: x proximity`
- Tab items: `flex-shrink: 0; scroll-snap-align: start; white-space: nowrap`
- Test at 320px viewport width from the first prototype

**Phase:** Phase 1.

---

### M-3: Font Loading Causes Layout Shift

**Prevention:**
- Preconnect links for `fonts.googleapis.com` and `fonts.gstatic.com` (with `crossorigin`)
- Load only: Montserrat 400+700, Source Sans 3 400+600
- `font-display: swap` (already in @import with `display=swap`)

**Phase:** Phase 1.

---

### M-4: Toastmasters Publishes Content Updates Without Warning

**Known changes:** Level 1 restructured October 2024. Two new paths added April 2026.

**Prevention:**
- Add `"dataVersion": "2026-05"` and `"lastVerified": "2026-05-27"` at top of embedded JSON
- Display "Data current as of [month/year]" in UI footer
- Key JSON by path slug (not positional index) so individual paths update in isolation

**Phase:** Phase 1 (schema). Add version field from day one.

---

### M-5: ARIA Tab Pattern Incomplete — Screen Reader Navigation Broken

**Full required pattern:**
- Container: `role="tablist"`
- Each tab: `role="tab"`, `aria-selected`, `tabindex="0"` (active) or `tabindex="-1"` (inactive), `aria-controls="[panelId]"`
- Panel: `role="tabpanel"`, `id`, `aria-labelledby="[tabId]"`, `tabindex="0"`
- JS: arrow keys move focus between tabs; active tab: `aria-selected="true"` + `tabindex="0"`

Implement the W3C APG tabs pattern verbatim. ARIA is easier to add during construction than retrofit.

**Phase:** Phase 1 (tab component).

---

### M-6: Smart Quotes and Trailing Spaces in Transcribed Names

**Prevention:**
- `.trim()` all string values in a post-transcription pass
- Lint all path IDs for non-ASCII characters before embedding
- Use programmatically derived slugs, never typed manually

**Phase:** Phase 2 (data validation).

---

## Minor Pitfalls

### N-1: Pretty-Printed JSON Inflates File Size
Minify JSON in the final file. No base64 font embedding. Target < 500KB total HTML.

### N-2: FAB Under iOS Safe Area
Use `bottom: calc(1.5rem + env(safe-area-inset-bottom))`. Requires `viewport-fit=cover` in meta viewport.

### N-3: Enhancement Section Rendered as a Project Card
Model as `level.enhancement` (separate field), never inside `level.projects[]`. Use a separate render function.

### N-4: Path Selector Tap Targets Too Small
Native `<select>` handles this on mobile. If custom: `min-height: 44px; padding: 12px 16px`.

### N-5: Duplicate DOM IDs from Shared Project Names Across Paths
"Ice Breaker" appears in Level 1 of all 8 paths. All DOM IDs referencing project data must include path slug + level: `id="panel-effective-coaching-l2-p0"`.

---

## Phase-Specific Priority Summary

| Phase | Pitfall | Priority |
|-------|---------|----------|
| Phase 1 | FTH script-tag test (C-1) | **Critical — test before anything** |
| Phase 1 | Render-on-demand only (C-2) | **Critical — architectural decision** |
| Phase 1 | `100dvh` layout + `env()` safe area (C-3) | **Critical** |
| Phase 1 | Happy Yellow contrast rules (M-1) | High |
| Phase 1 | ARIA full tab pattern (M-5) | High |
| Phase 1 | Tab strip at 320px (M-2) | High |
| Phase 1 | Font preconnect + swap (M-3) | Medium |
| Phase 1 | dataVersion field in schema (M-4) | Medium |
| Phase 1 | DOM ID naming convention (N-5) | Low |
| Phase 1 | FAB safe area (N-2) | Low |
| Phase 2 | R/E classification validation (C-4) | **Critical** |
| Phase 2 | Time format canonical standard (C-5) | High |
| Phase 2 | Smart quote normalization (M-6) | Medium |
| Phase 3 | File size audit (N-1) | Low |
