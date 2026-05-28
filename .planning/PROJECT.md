# Toastmasters Pathways App

## What This Is

A self-contained, single-file HTML reference app that maps the complete Toastmasters Pathways educational program — all 8 paths, 5 levels per path, and every project within each level — into a navigable, mobile-friendly interface. Members browse paths and levels to find project requirements without needing to open PDFs or log into Base Camp.

## Core Value

**Every club member can instantly find any project's requirements (name, time, purpose, Required/Elective status) without downloading PDFs or logging in.**

## Context

- **Club**: FTH club website at 4039.toastmastersclubs.org
- **Hosting**: FTH (Free Toast Host) — supports HTML, CSS, JavaScript; single-page upload, max 65MB
- **Audience**: Active Toastmasters club members, any level
- **Data source**: Official Toastmasters Pathways PDF documents (8 paths, provided by user)
- **External assets**: PDFs (e.g., evaluation forms) can be linked from GitHub repo: https://github.com/Bond7010/tm-claude-design-kit

## Design System

**Brand kit**: https://github.com/Bond7010/tm-claude-design-kit

| Token | Value | Use |
|-------|-------|-----|
| `--tm-loyal-blue` | #004165 | Headers, primary backgrounds |
| `--tm-true-maroon` | #772432 | Accent backgrounds, secondary headers |
| `--tm-cool-gray` | #A9B2B1 | Neutral areas |
| `--tm-happy-yellow` | #F2DF74 | Highlights, badges (use sparingly) |
| `--tm-font-headline` | Montserrat (fallback: Gotham, Arial) | Level names, project titles |
| `--tm-font-body` | Source Sans 3 (fallback: Segoe UI, Arial) | Purpose text, descriptions |

Rules: White or Happy Yellow text on dark backgrounds only. Loyal Blue, True Maroon, or Black on light backgrounds. No word art, drop shadows, or typeface manipulation.

## UI Layout (from reference screenshots)

```
┌─────────────────────────────────────────────────┐
│  [Loyal Blue] Top bar — "Pathways" title         │
├─────────────────────────────────────────────────┤
│  [Maroon] Path selector — dropdown with 8 paths  │
├─────────────────────────────────────────────────┤
│  Level tabs: [L1] [L2] [L3] [L4] [L5]          │
├─────────────────────────────────────────────────┤
│  SCROLLABLE CONTENT:                             │
│  Level name (headline)                           │
│  Level completion requirement (body text)        │
│                                                  │
│  ┌─ Enhancement section ─────────────────────┐  │
│  │  Meeting Roles: • Role 1  • Role 2  ...   │  │
│  └───────────────────────────────────────────┘  │
│                                                  │
│  ┌─ Project card ─────────────────── [Badge] ┐  │
│  │  Project Name                              │  │
│  │  5-7 minute speech                         │  │
│  │  Purpose: ...                              │  │
│  └────────────────────────────────────────────┘  │
│  (more cards...)                                 │
│                                    [↑ FAB]       │
└─────────────────────────────────────────────────┘
```

**Badges**: "Required" (blue/maroon badge) | "Elective" (yellow/warm badge)

## Content Structure

```
8 Paths
└── 5 Levels per path (some variation in project count per level)
    ├── Enhancement section (meeting roles — not a speech project)
    └── Project cards:
        ├── Project name
        ├── Required / Elective classification
        ├── Time requirement (e.g., "5-7 minute speech")
        └── Purpose text
```

**Paths (8 total):**
- 6 paths published 2024
- 2 paths published April 2026
- All follow 5-level structure; number of projects per level varies

**Level structure example (Level 1 "Mastering Fundamentals"):**
- Enhancement: Meeting roles to complete before/during level
- Required projects: Must complete all
- Elective projects: Complete N of the listed options

## Navigation Model

1. **Top bar** — static title "Pathways"
2. **Path selector** — dropdown/selector cycling through all 8 paths (no language/legacy toggles)
3. **Level tab strip** — 5 horizontally-scrollable tabs showing truncated level name + "Level N"
4. **Content area** — scrolls vertically showing Enhancement section + all project cards for selected path × level combination
5. **Scroll-to-top FAB** — floating button (bottom-right), appears on scroll

No click-through on project cards — all content is visible inline.

## Technical Constraints

- **Single HTML file** — all CSS and JavaScript embedded (no external CDN dependencies that could break)
- **Max size**: 65MB (easily achievable for this content)
- **No backend** — purely static, no login, no data persistence
- **Responsive**: Desktop (≥768px) and mobile (<768px), touch scroll on mobile
- **FTH compatibility**: Standard HTML5, CSS3, vanilla JavaScript — no frameworks that require build steps

## Requirements

### Validated

(None yet — ship to validate)

### Active

- [ ] Path selector with all 8 paths
- [ ] 5 level tabs per path, scrollable horizontally on mobile
- [ ] Enhancement section per level showing required meeting roles
- [ ] Project cards showing name, Required/Elective badge, time, purpose
- [ ] Toastmasters brand-compliant design (colors, fonts per brand kit)
- [ ] Responsive layout — desktop and mobile
- [ ] Touch scroll on mobile
- [ ] Scroll-to-top FAB
- [ ] All data embedded in the HTML (no external data files)
- [ ] Single self-contained HTML file deployable to FTH

### Out of Scope

- Progress tracking / marking projects complete — keep it simple, reference only
- English / Legacy toggles — not needed for this club's use
- Login / authentication — no backend
- Offline PWA — beyond FTH hosting capability
- Evaluation criteria, speaker tips — not shown in card view (card is the detail)

## Key Decisions

| Decision | Rationale | Outcome |
|----------|-----------|---------|
| Single HTML file | FTH hosts one page; no build pipeline needed | ✓ Locked |
| Brand kit over app colors | App is not TM brand-compliant; club wants compliance | ✓ Locked |
| No click-through on project cards | Card shows full detail inline — simpler UX | ✓ Locked |
| No progress tracking | Reference tool only; Base Camp handles progress | ✓ Locked |
| Vanilla JS, no framework | FTH compatibility, no build step, smaller file | ✓ Locked |
| Data source: PDFs → hardcoded JSON | User provides PDFs; data extracted and embedded | — Pending |

## Evolution

This document evolves at phase transitions and milestone boundaries.

**After each phase transition** (via `/gsd-transition`):
1. Requirements invalidated? → Move to Out of Scope with reason
2. Requirements validated? → Move to Validated with phase reference
3. New requirements emerged? → Add to Active
4. Decisions to log? → Add to Key Decisions
5. "What This Is" still accurate? → Update if drifted

**After each milestone** (via `/gsd-complete-milestone`):
1. Full review of all sections
2. Core Value check — still the right priority?
3. Audit Out of Scope — reasons still valid?
4. Update Context with current state

---
*Last updated: 2026-05-27 after initialization*
