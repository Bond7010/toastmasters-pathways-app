# Roadmap: Toastmasters Pathways App

## Overview

Build a single self-contained HTML file that gives Toastmasters club members instant access to all 8 paths × 5 levels × project details — without PDFs or Base Camp login. Phase 1 builds the complete navigation shell and render pipeline with minimal placeholder data. Phase 2 transcribes and validates all real content. Phase 3 polishes performance and accessibility to production quality.

## Phases

- [ ] **Phase 1: Foundation** — Complete navigation shell, brand design, and render pipeline with placeholder data
- [ ] **Phase 2: Data** — Transcribe, validate, and embed all 8 paths × 5 levels of real content
- [ ] **Phase 3: Polish** — File size audit, WCAG audit, performance verification, FTH production deployment

## Phase Details

### Phase 1: Foundation
**Goal**: A working, navigable HTML file — correct layout, brand design, full ARIA tab pattern, and render pipeline — verified on FTH and on real mobile. Content is placeholder data (1–2 paths) sufficient to prove the system works end to end.

**Depends on**: Nothing (first phase)

**Requirements**: NAV-01, NAV-02, NAV-03, NAV-04, NAV-05, NAV-06, NAV-07, NAV-08, CONT-01, CONT-02, CONT-03, CONT-04, CONT-05, CONT-06, CONT-07, DATA-04, DATA-05, DSGN-01, DSGN-02, DSGN-03, DSGN-04, DSGN-05, DSGN-06, DSGN-07, ACCSS-01, ACCSS-02, ACCSS-03, ACCSS-04, ACCSS-05, DEPL-01, DEPL-03, DEPL-04, PERF-01

**Success Criteria** (what must be TRUE):
  1. FTH throwaway test file confirmed: `<script>` tag survives static file upload
  2. Switching paths and levels renders the correct placeholder content in under 200ms
  3. Level tabs scroll horizontally at 320px viewport without wrapping or overflow
  4. Keyboard: arrow keys navigate tabs; Enter/Space selects; `<select>` keyboard-accessible
  5. Screen reader announces tab state changes (`aria-selected`) correctly
  6. Layout holds correctly on iOS Safari with no address-bar clip (100dvh)
  7. FAB appears on scroll and returns to top on click
  8. Brand colors and fonts match Toastmasters brand kit exactly

**Plans**: 3 plans

Plans:
- [ ] 01-01-PLAN.md — FTH throwaway gate test + HTML shell + complete CSS design system (brand tokens, Grid, 100dvh, badges, font preconnect)
- [ ] 01-02-PLAN.md — Embedded JSON placeholder data (2 paths) + render pipeline (buildProjectCardHTML, buildEnhancementHTML, buildLevelHTML, render())
- [ ] 01-03-PLAN.md — Event wiring + full W3C APG ARIA tab pattern + arrow-key keyboard navigation + IntersectionObserver FAB

### Phase 2: Data
**Goal**: All 8 paths × 5 levels of real content embedded and validated. Every project has correct name, time, purpose, and Required/Elective classification. Enhancement sections populated for all 6 current paths.

**Depends on**: Phase 1

**Requirements**: DATA-01, DATA-02, DATA-03

**Success Criteria** (what must be TRUE):
  1. All 40 path+level combinations render without blank panes or missing sections
  2. Required/Elective classifications match the official Pathways Path and Project Matrix PDF
  3. All time requirements follow the `"N–N min"` canonical format (no variations)
  4. Enhancement roles are present for all 5 levels of each of the 6 current paths
  5. Vintage paths (Basic Training, Communication Series) correctly show no Enhancement section
  6. `dataVersion` and `lastVerified` fields set in embedded JSON

**Plans**: TBD

Plans:
- [x] 02-01: Data validation script (R/E counts, time format regex, slug linter, smart-quote detector)
- [x] 02-02: Transcribe 5 current paths (Engaging Humor, Motivational Strategies, Persuasive Influence, Presentation Mastery, Visionary Communication) × 5 levels + expand EVAL_FORMS
- [ ] 02-03: Transcribe 2 vintage paths (Basic Training, Communication Series) + verify level breakdown against Base Camp
- [ ] 02-04: Full 40-combination render QA — spot-check R/E classifications against Matrix PDF

### Phase 3: Polish
**Goal**: Production-ready file — verified accessible, correctly sized, and fully deployed to the club FTH website.

**Depends on**: Phase 2

**Requirements**: DEPL-02, PERF-02, ACCSS-01, ACCSS-02, ACCSS-03, ACCSS-04, ACCSS-05

**Success Criteria** (what must be TRUE):
  1. Single HTML file under 500KB
  2. Full WCAG 2.1 AA audit passes (contrast ratios, ARIA, keyboard, focus indicators)
  3. App loads and is interactive in under 3 seconds on mid-range mobile (3G simulation)
  4. File is live at 4039.toastmastersclubs.org and accessible to club members

**Plans**: TBD

Plans:
- [ ] 03-01: JSON minification + file size audit (target <500KB)
- [ ] 03-02: WCAG 2.1 AA audit (contrast, ARIA, keyboard, focus indicators)
- [ ] 03-03: Production FTH deployment + smoke test on real iOS and Android devices

## Progress

| Phase | Plans Complete | Status | Completed |
|-------|----------------|--------|-----------|
| 1. Foundation | 0/3 | Planned | - |
| 2. Data | 2/4 | In progress | - |
| 3. Polish | 0/3 | Not started | - |

---
*Roadmap created: 2026-05-27*
*Plans for Phase 1 written: 2026-05-28*
