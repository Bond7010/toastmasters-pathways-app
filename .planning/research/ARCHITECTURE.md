# Architecture Research — Toastmasters Pathways Reference App

**Project:** Toastmasters Pathways Reference App
**Researched:** 2026-05-27
**Confidence:** HIGH — vanilla JS patterns are mature and stable

---

## Key Findings

1. **Data structure: nested object keyed by path slug.** `PATHWAYS['dynamic-leadership'].levels[0]` gives O(1) access. Flat arrays require filtering across 800+ entries per tab click.

2. **Render strategy: render-on-demand via single innerHTML assignment to the content pane.** At 5-25 cards per level, one innerHTML replacement per tab click is cheap. The objection to innerHTML (losing form focus) does not apply since there are no inputs.

3. **Template method: template literal + `map().join('')` + single innerHTML write.** cloneNode with `<template>` adds management overhead without benefit at this card count.

4. **State: plain singleton object + explicit render call.** `setState({ selectedLevel: 2 })` → `render()`. Observer/Pub-Sub adds indirection for one mutable state and one consumer.

5. **Event delegation: `.closest()` on the tab container, direct listener on the `<select>`.** `.closest()` handles clicks on child elements inside tabs.

6. **Enhancement section: separate field on the level object, not a sentinel in the projects array.** Different schema (no badge, no time, roles[] instead of purpose). Separate `buildEnhancementHTML` function prevents type-checking branches in the card renderer.

7. **Build order: Data → State skeleton → Shell HTML/CSS → Render functions → Events → FAB/polish → QA.**

---

## Component Boundaries

| Component | Responsibility | Communicates With |
|-----------|---------------|-------------------|
| `PATHWAYS` constant | Source of truth for all content | Consumed by render functions only |
| `state` object | selectedPath (slug) + selectedLevel (0–4) | Written by event handlers, read by render() |
| Path selector `<select>` | Displays 8 paths | Direct change listener → setState |
| Level tab strip | 5 tabs, active class toggling | Click delegation → setState; render() sets active class |
| Content pane `#content-pane` | Level header + enhancement + cards | Owned exclusively by render(); never read by events |
| FAB | Show on scroll, scroll-to-top on click | Self-contained scroll/IntersectionObserver listener |

---

## Data Flow (one direction only)

```
User gesture
    → event handler reads DOM value (e.target.value / dataset.level)
    → setState({ selectedPath, selectedLevel })
    → render()
        → update tab active classes (5 classList.toggle calls)
        → build HTML string from PATHWAYS[state.selectedPath].levels[state.selectedLevel]
        → contentPane.innerHTML = builtString
```

No reverse flow. Events never read `contentPane`. Render never fires events.

---

## Data Schema

```javascript
// Embedded as <script type="application/json" id="pathways-data">
[
  {
    "id": "dynamic-leadership",           // slug for state & URL hash
    "name": "Dynamic Leadership",
    "levels": [
      {
        "level": 1,
        "name": "Mastering Fundamentals",
        "completionRequirement": "Complete 4 required projects and enhancement tasks",
        "enhancement": {
          "roles": [
            "Table Topic Speaker",
            "Evaluator",
            "Timer OR Ah-Counter"
          ],
          "notes": "Table Topics Speaker must be completed before delivering an Ice Breaker speech"
        },
        "projects": [
          {
            "name": "Ice Breaker",
            "type": "Required",            // "Required" | "Elective"
            "time": "4-6 minute speech",
            "purpose": "Deliver a speech on any topic to introduce yourself to the club and learn the basic structure of public speaking"
          }
        ]
      }
    ]
  }
]
```

**Why enhancement is separate from projects:** Enhancement tasks have a different schema (roles list, not a speech project). Mixing them forces type checks in every card renderer. Separate field, separate renderer, zero branching.

---

## Build Order (with dependency notes)

| Step | What | Depends On | Verify By |
|------|------|------------|-----------|
| 1 | `PATHWAYS` data constant (all 8 paths) | Nothing | `PATHWAYS[0].levels[0].projects.length` in console |
| 2 | `state` + `setState` + stub `render()` | Step 1 | Console log on page load shows state object |
| 3 | Static HTML shell + all CSS | Nothing (parallel with Step 2) | Shell renders visually with brand colors |
| 4 | `buildLevelHTML`, `buildProjectCardHTML`, `buildEnhancementHTML`; real `render()` | Steps 1, 3 | Default path/level renders cards on load |
| 5 | Path selector listener + tab delegation + keyboard nav + ARIA | Steps 2, 3, 4 | Switching path resets to level 0 and re-renders; tabs update active class; arrow keys work |
| 6 | FAB (IntersectionObserver) + scroll-to-top | Step 3 | FAB appears on scroll, click scrolls to top |
| 7 | Full content QA — all 40 path/level combinations | Steps 1–6 | No blank panes, no missing enhancements, badges correct |

---

## Prescriptive Decisions Summary

| Question | Answer |
|----------|--------|
| Data structure | Nested array, path slug `id` field, enhancement as level field (not in projects array) |
| Render strategy | Render-on-demand; innerHTML replace content pane on every navigation |
| Template method | Template literal string build + `map().join('')` + single innerHTML assignment |
| State management | Plain singleton object, `setState()` → `render()` explicit call chain |
| Event delegation | `.closest()` on tab container; direct listener on `<select>` |
| CSS active state | `aria-selected` attribute drives CSS via `[aria-selected="true"]` selector |
| Enhancement vs cards | Separate schema, separate field, separate render function |
| Build order | Data first, shell/CSS second, render functions third, events fourth, FAB fifth |

---

## Anti-Patterns to Avoid

| Anti-Pattern | Why |
|-------------|-----|
| Render all 40 views on load + show/hide | 800+ cards in DOM at page load, all unused |
| Read state from the DOM | DOM is the view layer, not the state store |
| Per-tab addEventListener in a loop | Must be re-run on every render; delegation from stable parent avoids this |
| Enhancement as sentinel in projects array | Causes type-checking branches in every card renderer |
| Post-render DOM traversal for badge color | Set badge class during HTML string build; let CSS handle the color |
| Storing path/level in global variables without setState | Bypasses the explicit render() call chain; leads to stale UI |
