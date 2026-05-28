<!-- GSD:project-start source:PROJECT.md -->
## Project

**Toastmasters Pathways App** — A single self-contained HTML file that maps the complete Toastmasters Pathways educational program (8 paths × 5 levels × all projects) into a navigable, mobile-friendly reference interface. Club members at 4039.toastmastersclubs.org can instantly find project requirements (name, time, purpose, Required/Elective) without PDFs or Base Camp login.

**Hosting:** FTH (Free Toast Host) static file upload — 65MB max, HTML/CSS/JS only, single page.
**Deliverable:** One `.html` file — all CSS and JS embedded. No external dependencies that could break.
**Brand kit:** https://github.com/Bond7010/tm-claude-design-kit
<!-- GSD:project-end -->

<!-- GSD:stack-start source:research/STACK.md -->
## Technology Stack

- **Document:** HTML5 single file — all CSS + JS embedded
- **Data:** `<script type="application/json" id="pathways-data">` — MDN-endorsed inert data block; parsed once into `PATHWAYS` constant
- **Layout:** CSS Grid `grid-template-rows: auto auto auto 1fr` + `height: 100dvh` (NOT `100vh` — iOS Safari address-bar bug)
- **Touch scroll:** `overflow-y: auto` + `overscroll-behavior: contain` (NOT `-webkit-overflow-scrolling: touch` — deprecated iOS 13+)
- **Tab strip:** `overflow-x: auto` + `scroll-snap-type: x proximity`
- **Path selector:** Native `<select>` — free mobile platform picker UX
- **FAB:** `IntersectionObserver` on scroll sentinel — no scroll event listeners
- **JS:** ES2020 vanilla — no libraries, no frameworks, no build step
- **Fonts:** Google Fonts @import (Montserrat 400+700, Source Sans 3 400+600, display=swap) + preconnect links
- **Tokens:** CSS custom properties — two-tier (brand values → semantic intent)

**Brand colors:**
- `--tm-loyal-blue: #004165` — headers, primary backgrounds, Required badge bg
- `--tm-true-maroon: #772432` — accent backgrounds, secondary headers
- `--tm-cool-gray: #A9B2B1` — neutral areas
- `--tm-happy-yellow: #F2DF74` — Elective badge bg ONLY (black text; NEVER use yellow on light backgrounds — fails WCAG at 1.4:1)
<!-- GSD:stack-end -->

<!-- GSD:conventions-start source:CONVENTIONS.md -->
## Conventions

**Data format:** Time requirements → `"N–N min"` (en-dash, abbreviated unit, no "minute" word). Strict enum `"type": "required" | "elective"` — no freeform strings.

**DOM IDs must include path slug + level:** `id="panel-dynamic-leadership-l2"` — "Ice Breaker" appears in all 8 paths' Level 1; IDs without path context will collide.

**No direct DOM reads for state:** State lives in the `state` singleton object only. Never read `innerHTML` or attribute values to determine current path/level.

**Enhancement vs. projects:** `level.enhancement` is a separate field — never add enhancement tasks to `level.projects[]`. Different schema (roles[], notes), different renderer.

**Active state via ARIA:** `[aria-selected="true"]` drives tab styling in CSS — no separate `.active` class.
<!-- GSD:conventions-end -->

<!-- GSD:architecture-start source:research/ARCHITECTURE.md -->
## Architecture

**Data flow (one direction only):**
```
User gesture → event handler → setState({ selectedPath, selectedLevel }) → render()
    → update tab aria-selected (5 classList.toggle calls)
    → build HTML string from PATHWAYS[state.selectedPath].levels[state.selectedLevel]
    → contentPane.innerHTML = builtString
```

**State:** Plain singleton `{ selectedPath: 'dynamic-leadership', selectedLevel: 0 }`. Use `setState()` → `render()` explicit call chain only.

**Render:** On-demand only — one `innerHTML` replacement per navigation. NEVER render all 40 views on load. Full JSON in memory, minimal DOM.

**Components:**
- `PATHWAYS` constant — source of truth; consumed by render functions only
- `state` object — written by event handlers, read by `render()`
- Path `<select>` — direct `change` listener → `setState`
- Level tab strip — `.closest()` delegation → `setState`
- `#content-pane` — owned exclusively by `render()`; never read by events
- FAB — self-contained `IntersectionObserver` + scroll listener

**Build functions:** `buildProjectCardHTML(project)`, `buildEnhancementHTML(enhancement)`, `buildLevelHTML(level)`, `render()`
<!-- GSD:architecture-end -->

<!-- GSD:skills-start source:skills/ -->
## Project Skills

No project skills found.
<!-- GSD:skills-end -->

<!-- GSD:workflow-start source:GSD defaults -->
## GSD Workflow Enforcement

Before using Edit, Write, or other file-changing tools, start work through a GSD command so planning artifacts and execution context stay in sync.

Use these entry points:
- `/gsd-quick` for small fixes, doc updates, and ad-hoc tasks
- `/gsd-debug` for investigation and bug fixing
- `/gsd-execute-phase` for planned phase work

Do not make direct repo edits outside a GSD workflow unless the user explicitly asks to bypass it.
<!-- GSD:workflow-end -->

<!-- GSD:profile-start -->
## Developer Profile

> Profile not yet configured. Run `/gsd-profile-user` to generate your developer profile.
> This section is managed by `generate-claude-profile` — do not edit manually.
<!-- GSD:profile-end -->
