# Features Research — Toastmasters Pathways Reference App

**Domain:** Educational curriculum reference app (single-file, static HTML)
**Researched:** 2026-05-27
**Confidence:** HIGH (verified against NN/G, W3C WCAG, DOJ 2024 rule)

---

## Table Stakes

Features users expect. Missing = product feels incomplete or broken.

| Feature | Why Expected | Complexity | Notes |
|---------|--------------|------------|-------|
| Path selector (all 8 paths) | Core navigation entry point | Low | Dropdown; no grid picker needed at this scale |
| Level tab strip (L1–L5) | Tabs are the standard pattern for a fixed small set of parallel sections | Low | Horizontal scroll on mobile; truncated level name + "Level N" label |
| Project cards showing name, time, purpose, badge | The entire value proposition | Low | Required/Elective badge must be visually distinct, not text-only |
| Level completion requirement prominently displayed | Members cannot satisfy a level without knowing the rule | Low | Above project list, plain text — never buried in a tooltip |
| Enhancement section per level | Meeting roles are a real curricular requirement | Low | Visually distinct from project cards via labeled section block |
| Scroll-to-top FAB | 15–20 cards per level; returning to top on mobile without it is painful | Low | Show after ~300px scroll; bottom-right; `aria-label="Back to top"` |
| Responsive layout (mobile-first) | Primary use is on phones at club meetings | Medium | Touch scroll on level tabs; single-column on mobile |
| Toastmasters brand compliance | Club requirement | Low | Already locked: colors, fonts, contrast rules |
| Keyboard navigation for all controls | WCAG 2.1 AA / DOJ 2024 rule — legally significant for public educational pages | Medium | Native `<select>` for path (free); tab strip needs arrow-key JS + ARIA |
| Visible focus indicators | WCAG 2.4.7 AA | Low | Never remove `:focus` outline; 2px Loyal Blue ring |
| ARIA roles on tab strip | Screen readers need `role="tablist"`, `role="tab"`, `aria-selected`, `aria-controls` | Medium | Path `<select>` is free; tab strip needs explicit ARIA |
| Sufficient color contrast (4.5:1) | WCAG 1.4.3 AA | Low | Verify white on Loyal Blue and white on True Maroon |

---

## Differentiators

| Feature | Value Proposition | Complexity | Notes |
|---------|-------------------|------------|-------|
| Shareable URL hash routing | Members can link peers directly to a specific path × level | Medium | `location.hash` encodes `#path=2&level=3`; works on FTH without server config |
| Print-friendly CSS | Members who want an offline reference for meetings | Low | `@media print` hides nav chrome, `page-break-inside: avoid` on cards |
| Sticky level tab strip | Tabs always visible during long card scrolls | Low | `position: sticky; top: 0` — CSS-only, no JS |
| Required/Elective count summary per level | "4 Required, 3 Elective (complete 1)" saves counting | Low | Static data — counts known at build time |
| Enhancement section inline explainer | Members new to Pathways may not understand what Enhancement means | Low | Single sentence of inline text — never a tooltip |

---

## Anti-Features

| Anti-Feature | Why Avoid | What to Do Instead |
|--------------|-----------|-------------------|
| Keyword search / filter | Dataset is bounded to ~15 cards per level; NN/G: filtering earns complexity only when data volume overwhelms navigation | Path selector + level tabs are the filter |
| Dark mode | Brand palette has no sensible dark inversion; FTH has no user preference storage | Ensure light-mode contrast is excellent |
| Progress tracking | Base Camp already tracks progress; duplicating creates a second source of truth | Reference only |
| Accordion-collapsible project cards | Hiding content defeats the quick-reference purpose | All card content visible inline (locked decision) |
| Breadcrumb navigation | Two-dimension nav (path × level) always visible in header; NN/G: breadcrumbs add no value for shallow hierarchies on mobile | Path dropdown + level tabs show current location |
| Animated transitions | Delays content display on phones; user is looking something up at a meeting | Instant content swap |
| PWA / offline | Beyond FTH hosting capability | Bookmark the page |
| English / legacy toggles, auth, evaluation criteria, speaker tips | All explicitly out of scope | — |

---

## Feature Dependencies

```
Path selector
  └── Level tabs (need selected path to show correct level names)
        └── Project cards (need path × level)
              └── Required/Elective badge (part of each card)
              └── Level completion requirement (displayed once above cards)
              └── Enhancement section (displayed once above cards)

Shareable URL hash routing
  └── Encodes: path selector state + level tab state
  └── Enables: external links to specific path × level

Sticky level tabs — CSS-only, no JS dependency

Scroll-to-top FAB — depends on scroll event listener (minimal JS)

Print CSS — pure @media print, no dependencies
```

---

## MVP Recommendation

### Phase 1 (v1)
1. Path selector, level tabs, project cards, badges
2. Level completion requirement text (prominently above cards)
3. Enhancement section per level
4. Scroll-to-top FAB
5. ARIA roles on tab strip and FAB
6. Keyboard navigation + visible focus indicators
7. Color contrast verification against brand palette
8. Responsive mobile-first layout

### Phase 2 (after v1 validation)
- Shareable URL hash routing (highest value addition)
- Sticky level tab strip (CSS-only, low risk)
- Required/Elective count summary per level
- Print-friendly CSS

### Defer indefinitely
- Everything in Anti-Features list

---

## Accessibility Requirements (Table Stakes Detail)

| Requirement | WCAG Criterion | Implementation |
|-------------|---------------|----------------|
| All controls keyboard-operable | 2.1.1 Level A | Native `<select>` for path (free); arrow-key JS for tab strip |
| Visible focus indicator | 2.4.7 Level AA | 2px Loyal Blue outline; never remove `:focus` |
| Color contrast 4.5:1 normal text | 1.4.3 Level AA | Verify all brand-on-background combinations |
| Tab strip announces selected state | 4.1.2 Level A | `aria-selected="true"` on active tab; `aria-controls` to content pane |
| Content region announced on tab switch | 4.1.3 Level AA | `role="region"` + `aria-label` on content pane |
| FAB accessible | 2.4.6 Level AA | `aria-label="Back to top"` on button element |

---

## Open Questions for Data Phase

1. How are the 8 paths identified internally? (IDs vs names — affects hash routing)
2. Do all 8 paths have exactly 5 levels, or any variation?
3. What are the exact "complete N required + M elective" rules per level per path?
