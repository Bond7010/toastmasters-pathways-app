# Handoff: Toastmasters Pathways App

**Date:** 2026-05-31
**Status:** Minification complete — ready for FTH upload and Phase 3

---

## What This Project Is

Single `pathways-app.html` — all 8 Toastmasters Pathways paths, 5 levels each, all projects.
Mobile-friendly reference for Club 4039 members. Hosted on FTH (static HTML upload only).

**Constraint:** FTH file size limit is 65,533 chars. File is now 57,194 chars (8,339 margin).

---

## Current State

| Phase | Status |
|-------|--------|
| Phase 1: Foundation (HTML shell, render pipeline, event wiring) | COMPLETE ✓ |
| Phase 2: Data (all 8 paths transcribed from eval forms) | COMPLETE ✓ |
| Minification (FTH upload constraint) | COMPLETE ✓ |
| Phase 3: Polish | NOT STARTED |

**Files on disk:**
- `pathways-app.html` — minified, 57,194 chars, ready for FTH upload
- `pathways-app.full.html` — readable backup, 118,980 chars, original field names
- `eval-forms/current-paths/` — all 8 paths' evaluation form PDFs
- `_temp_js.js`, `_temp_js_min.js` — terser temp files, safe to delete

---

## Minification Summary (for debugging if needed)

The minified file is NOT human-readable. Key transformations applied:

**JSON key renames** (update JS if you ever edit field access):
| Original key | Minified key |
|---|---|
| name | n |
| purpose | p (now an INTEGER index into root pL array) |
| type | y (values: "R" = Required, "E" = Elective) |
| time | t |
| levels | v |
| level | l |
| projects | j |
| enhancement | e |
| completionRequirement | c |
| roles | r |
| notes | z |

**Purpose deduplication:**
- 244 purpose strings reduced to 105 unique entries in `data.pL` array at JSON root
- Each project's `p` field is an integer index, not a string
- JS: `var PL=_pd.pL;` defined at startup; access as `PL[project.p]`

**Badge display:** type values are stored as "R"/"E" but decoded to "Required"/"Elective"
for display via: `e.y=='R'?'Required':'Elective'` in the template

**To inspect the readable source:** use `pathways-app.full.html` — it has original
field names, CSS comments, JS comments, and unminified JSON.

---

## Immediate Next Step

1. **Upload `pathways-app.html` to FTH** via File Management at 4039.toastmastersclubs.org
2. **Smoke-test live:** verify path selector, tabs, project cards, modal popover, FAB
3. **Run `validatePathwaysData()` in browser console** — should print PASS
4. **Start Phase 3** (Polish) — run `/gsd-plan-phase` to create the plan

---

## Architecture Quick Reference

**Data flow:** User gesture → `setState({selectedPath, selectedLevel})` → `render()`
→ tab `aria-selected` update → `contentPane.innerHTML` replacement

**State:** singleton `{ selectedPath: 'dynamic-leadership', selectedLevel: 0 }`

**Key DOM IDs:** `pathways-path-selector`, `pathways-tab-strip`, `pathways-content-pane`,
`pathways-sentinel` (FAB observer), `pathways-modal`, `pathways-fab`

**JS global:** `window.validatePathwaysData()` — run from console to check data integrity

---

## Known Issues / Watch Points

- The `time` field regex in `validatePathwaysData` uses an en-dash that may appear
  garbled in some editors — that's pre-existing, not a minification artifact
- Two `<style>` blocks in `<head>` — both are minified, this is intentional
- `pathways-app.full.html` uses LF line endings; git may show CRLF warning on Windows
