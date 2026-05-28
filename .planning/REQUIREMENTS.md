# Requirements: Toastmasters Pathways App

**Defined:** 2026-05-27
**Core Value:** Every club member can instantly find any project's requirements (name, time, purpose, Required/Elective status) without downloading PDFs or logging in.

## v1 Requirements

### Navigation

- [ ] **NAV-01**: App displays a path selector showing all 8 paths (6 current + 2 vintage)
- [ ] **NAV-02**: Path selector renders as a native `<select>` element (mobile platform picker UX)
- [ ] **NAV-03**: App displays 5 level tabs (Level 1–Level 5) for the currently selected path
- [ ] **NAV-04**: Level tabs scroll horizontally at narrow viewports (320px minimum) without wrapping
- [ ] **NAV-05**: Selecting a different path resets the view to Level 1 of that path
- [ ] **NAV-06**: Selecting a level tab renders that level's content instantly (no page reload)
- [ ] **NAV-07**: Scroll-to-top FAB appears after the user scrolls past the top of the content area
- [ ] **NAV-08**: Clicking the FAB scrolls the content area back to the top

### Content Display

- [ ] **CONT-01**: Each path+level view shows the level name and completion requirement above the project list
- [ ] **CONT-02**: Each path+level view shows an Enhancement section listing required meeting roles (current paths only)
- [ ] **CONT-03**: Each project is displayed as a card showing: name, Required/Elective badge, time requirement, purpose text
- [ ] **CONT-04**: Project cards show all content inline — no click-through or expandable detail
- [ ] **CONT-05**: Required projects display a visually distinct "Required" badge
- [ ] **CONT-06**: Elective projects display a visually distinct "Elective" badge
- [ ] **CONT-07**: Vintage paths (Basic Training, Communication Series) show no Enhancement section

### Data

- [ ] **DATA-01**: All 8 paths' data (names, levels, projects) is embedded in the single HTML file
- [ ] **DATA-02**: All projects have correct Required/Elective classifications matching official Toastmasters materials
- [ ] **DATA-03**: All time requirements use the canonical format: `"N–N min"` (en-dash, no "minute" word)
- [ ] **DATA-04**: Embedded data includes a `dataVersion` field (e.g., `"2026-05"`) and `lastVerified` date
- [ ] **DATA-05**: Enhancement data is stored as a separate `level.enhancement` field (not inside `level.projects[]`)

### Visual Design

- [ ] **DSGN-01**: App uses Toastmasters brand colors: Loyal Blue `#004165`, True Maroon `#772432`, Happy Yellow `#F2DF74`
- [ ] **DSGN-02**: App uses Toastmasters brand fonts: Montserrat (headlines), Source Sans 3 (body)
- [ ] **DSGN-03**: "Required" badge uses Loyal Blue background with white text
- [ ] **DSGN-04**: "Elective" badge uses Happy Yellow background with black text
- [ ] **DSGN-05**: Layout: top bar → path selector → level tabs → scrollable content (matches reference screenshot structure)
- [ ] **DSGN-06**: Layout is fully responsive: desktop (≥768px) and mobile (<768px)
- [ ] **DSGN-07**: Touch scrolling works natively on iOS Safari and Android Chrome (no `-webkit-overflow-scrolling`)

### Accessibility

- [ ] **ACCSS-01**: Level tabs implement the full W3C APG ARIA tab pattern: `tablist`, `tab`, `tabpanel`, `aria-selected`, `tabindex`, arrow-key keyboard navigation
- [ ] **ACCSS-02**: All interactive elements have minimum 44px touch targets on mobile
- [ ] **ACCSS-03**: All text meets WCAG 2.1 AA contrast: 4.5:1 for normal text, 3:1 for large text
- [ ] **ACCSS-04**: Scroll-to-top FAB has `aria-label="Back to top"`
- [ ] **ACCSS-05**: Path selector is fully keyboard accessible via native `<select>`

### Deployment

- [ ] **DEPL-01**: Deliverable is a single self-contained `.html` file (all CSS and JS inline, no external dependencies)
- [ ] **DEPL-02**: File size is well under the 65MB FTH upload limit (target: <500KB)
- [ ] **DEPL-03**: App functions correctly when uploaded as a static file to FTH (not through the CMS page editor)
- [ ] **DEPL-04**: FTH static file upload has been tested with a throwaway `<script>` file before building real content

### Performance

- [ ] **PERF-01**: Only the currently selected path+level's project cards are rendered in the DOM at any time
- [ ] **PERF-02**: App is interactive in under 3 seconds on a mid-range mobile device

## v2 Requirements

### Enhanced Navigation

- **ENHV-01**: Shareable URL hash routing — selected path+level encoded in URL fragment (e.g., `#dynamic-leadership-l2`)
- **ENHV-02**: Sticky level tab strip — tabs remain visible when scrolling the content area

### Content Enhancements

- **ENHE-01**: R/E count summary per level (e.g., "3 Required + 2 Elective")
- **ENHE-02**: Print-friendly CSS layout for project cards

## Out of Scope

| Feature | Reason |
|---------|--------|
| Progress tracking / marking projects complete | Reference tool only; Base Camp handles progress |
| English / Legacy path toggles | Not needed for this club's use case |
| Login / authentication | No backend; purely static |
| Offline PWA / service worker | Beyond FTH hosting capability |
| Dark mode | Brand palette has no sensible dark inversion |
| Evaluation criteria / speaker notes | Not in card view; cards are the full detail |
| Search / filter across projects | Out of scope for v1 reference tool |
| External JSON via fetch() | Breaks single-file constraint; CORS issues on FTH |

## Traceability

| Requirement | Phase | Status |
|-------------|-------|--------|
| NAV-01 | Phase 1 | Pending |
| NAV-02 | Phase 1 | Pending |
| NAV-03 | Phase 1 | Pending |
| NAV-04 | Phase 1 | Pending |
| NAV-05 | Phase 1 | Pending |
| NAV-06 | Phase 1 | Pending |
| NAV-07 | Phase 1 | Pending |
| NAV-08 | Phase 1 | Pending |
| CONT-01 | Phase 1 | Pending |
| CONT-02 | Phase 1 | Pending |
| CONT-03 | Phase 1 | Pending |
| CONT-04 | Phase 1 | Pending |
| CONT-05 | Phase 1 | Pending |
| CONT-06 | Phase 1 | Pending |
| CONT-07 | Phase 1 | Pending |
| DATA-01 | Phase 2 | Pending |
| DATA-02 | Phase 2 | Pending |
| DATA-03 | Phase 2 | Pending |
| DATA-04 | Phase 1 | Pending |
| DATA-05 | Phase 1 | Pending |
| DSGN-01 | Phase 1 | Pending |
| DSGN-02 | Phase 1 | Pending |
| DSGN-03 | Phase 1 | Pending |
| DSGN-04 | Phase 1 | Pending |
| DSGN-05 | Phase 1 | Pending |
| DSGN-06 | Phase 1 | Pending |
| DSGN-07 | Phase 1 | Pending |
| ACCSS-01 | Phase 1 | Pending |
| ACCSS-02 | Phase 1 | Pending |
| ACCSS-03 | Phase 1 | Pending |
| ACCSS-04 | Phase 1 | Pending |
| ACCSS-05 | Phase 1 | Pending |
| DEPL-01 | Phase 1 | Pending |
| DEPL-02 | Phase 3 | Pending |
| DEPL-03 | Phase 1 | Pending |
| DEPL-04 | Phase 1 | Pending |
| PERF-01 | Phase 1 | Pending |
| PERF-02 | Phase 3 | Pending |

**Coverage:**
- v1 requirements: 37 total
- Mapped to phases: 37
- Unmapped: 0 ✓

---
*Requirements defined: 2026-05-27*
*Last updated: 2026-05-27 after initial definition*
