# Handoff: Toastmasters Pathways App

**Date:** 2026-05-28  
**From:** Claude Code (Desktop App)  
**To:** Claude Code (VS Code Extension)  
**Task:** Execute Phase 1 — build the working HTML app

---

## What This Project Is

A single self-contained `pathways-app.html` file that maps the complete Toastmasters Pathways program (8 paths × 5 levels × all projects) into a navigable, mobile-friendly reference. Club members at 4039.toastmastersclubs.org will be able to find any project's requirements without PDFs or Base Camp login.

**Constraint that shapes everything:** The file is hosted on FTH (Free Toast Host) as a static HTML upload. All CSS and JS must be embedded — no external files except Google Fonts. No frameworks, no build step.

---

## Current State

**No code has been written yet.** The project has been fully planned:

- `CLAUDE.md` — project context, stack decisions, architecture, conventions (read this first)
- `.planning/ROADMAP.md` — 3 phases overview
- `.planning/STATE.md` — current position (Phase 1, 0/3 plans complete)
- `.planning/phases/01-foundation/` — all 3 Phase 1 plans, ready to execute

The repo is on branch `master`. There is no `pathways-app.html` yet.

---

## What To Do

Execute Phase 1. It has 3 sequential plans. Use `/gsd-execute-phase` to run them — do not edit files directly outside the GSD workflow (CLAUDE.md says so).

### Plan 01-01 — FTH gate test + HTML shell + CSS design system

**Start here.** File: `.planning/phases/01-foundation/01-01-PLAN.md`

This plan has **2 tasks + 1 human checkpoint:**

1. **Task 1 (human action required):** Create `fth-test.html` and upload it to the live FTH site at 4039.toastmastersclubs.org via File Management (not the CMS editor — the editor strips script tags). Confirm that the page displays "JavaScript ran successfully." This gates the whole project. **Do not skip this.**

2. **Task 2 (auto):** Create `pathways-app.html` with the complete 4-zone CSS Grid layout, all Toastmasters brand tokens, Google Fonts, badge styles, and full iOS viewport handling (`100dvh`, `env(safe-area-inset-bottom)`, `overscroll-behavior`). Exact CSS is specified in the plan.

3. **Task 3 (human verify):** Open `pathways-app.html` in Chrome at 320px viewport. Confirm the layout renders: blue top bar, maroon path selector, scrollable tab strip, gray content area.

### Plan 01-02 — JSON data + render pipeline

**File:** `.planning/phases/01-foundation/01-02-PLAN.md`  
Depends on 01-01. Fully autonomous.

Embeds placeholder JSON for 2 paths (Dynamic Leadership + Communication Series) and implements the render pipeline: `buildProjectCardHTML()`, `buildEnhancementHTML()`, `buildLevelHTML()`, `render()`. After this plan, opening `pathways-app.html` in a browser shows Dynamic Leadership Level 1 content automatically.

### Plan 01-03 — Event wiring + ARIA keyboard + FAB

**File:** `.planning/phases/01-foundation/01-03-PLAN.md`  
Depends on 01-01 and 01-02. Ends with a human verification checkpoint.

Wires all user interactions: tab click delegation, path selector change, full W3C APG arrow-key keyboard navigation, IntersectionObserver FAB. After this plan the app is fully interactive.

Human verify at the end: click tabs, use keyboard navigation (ArrowLeft/Right + Enter), change path selector, confirm FAB appears on scroll.

---

## Critical Decisions Already Made

These are locked — do not revisit:

| Decision | Why |
|---|---|
| `height: 100dvh` (NOT `100vh`) | iOS Safari address-bar clips content with `100vh` |
| `overflow-y: auto` + `overscroll-behavior: contain` on content area | `-webkit-overflow-scrolling: touch` is deprecated iOS 13+ |
| Native `<select>` for path picker | Free mobile platform picker UX, no custom dropdown needed |
| `IntersectionObserver` for FAB | No scroll event listeners — better performance |
| `[aria-selected="true"]` drives tab CSS — no `.active` class | ARIA-first pattern; state is in attributes, not class names |
| `enhancement` is a separate level field, never inside `projects[]` | Different schema, different renderer |
| DOM IDs must include path slug (e.g., `tab-dynamic-leadership-l0`) | "Ice Breaker" appears in all 8 paths — IDs without path context collide |
| State lives only in `state` singleton — never read from DOM | Architectural gate; no `innerHTML` reads anywhere |

---

## Key Files

| File | Purpose |
|---|---|
| `CLAUDE.md` | Full project context — stack, architecture, conventions. Read before executing. |
| `.planning/ROADMAP.md` | Phase overview and success criteria |
| `.planning/STATE.md` | Current position and blockers |
| `.planning/research/STACK.md` | Every technology choice with exact implementation patterns |
| `.planning/research/PITFALLS.md` | What goes wrong and how to avoid it |
| `.planning/research/ARCHITECTURE.md` | Data flow diagram, component map, anti-patterns |

---

## Known Blockers

1. **FTH script-tag test** must be done manually before any real code is committed. Plan 01-01 Task 1 is a human-action checkpoint with clear pass/fail criteria. If it fails, stop and consult the user — the architecture depends on inline script tags working.

2. **Basic Training level names** (Levels 2–5) are unknown. Communication Series is used as a proxy for the vintage path in Phase 1 placeholder data. Phase 2 (Data) will transcribe the real names from Base Camp.
