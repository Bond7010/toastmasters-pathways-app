---
phase: "02"
plan: "01"
subsystem: "data-validation"
tags: [validator, data-quality, console-tool]
dependency_graph:
  requires: []
  provides: [window.validatePathwaysData]
  affects: [pathways-app.html]
tech_stack:
  added: []
  patterns: [vanilla-es2020, console-styled-output, iife-validator]
key_files:
  created: []
  modified:
    - pathways-app.html
decisions:
  - "Validator placed immediately after PATHWAYS constant definition so it has access to PATHWAYS without import or param"
  - "Returns violations array in addition to console output so callers can programmatically check results"
  - "Used styled console.log with brand colors (#004165 for PASS, #772432 for FAIL) matching project brand kit"
  - "VINTAGE_IDS kept as local constant inside function so validator is fully self-contained"
metrics:
  duration: "5m"
  completed: "2026-05-31T00:03:54Z"
  tasks_completed: 1
  files_modified: 1
---

# Phase 02 Plan 01: Add validatePathwaysData() Console Validator Summary

**One-liner:** Adds `window.validatePathwaysData()` — a zero-dependency console validator checking time format, type enum, smart quotes, enhancement nullability, and duplicate project names across all PATHWAYS data.

## What Was Built

A pure read-only validator function inserted into the `<script>` block of `pathways-app.html`, placed directly after `var PATHWAYS = _pd.paths;`. The function is callable from the browser DevTools console as `validatePathwaysData()`.

### Five Check Categories Implemented

| # | Check | Description |
|---|-------|-------------|
| 1 | Time format | `project.time` must match `/^\d+–\d+ min$/` (en-dash U+2013, not hyphen) |
| 2 | Type enum | `project.type` must be exactly `"Required"` or `"Elective"` (case-sensitive) |
| 3 | Smart quotes | No `"`, `"`, `'`, `'` in `project.name`, `project.time`, `project.purpose`, `level.name`, `level.completionRequirement`, `path.name` |
| 4 | Enhancement field | Vintage paths (`basic-training`, `communication-series`) must have `null` enhancement; all other paths must have non-null enhancement on every level |
| 5 | Duplicate project names | No two projects within the same path+level may share a name |

### Output Behavior

- **PASS:** Styled blue banner (`#004165`) + count of paths validated
- **FAIL:** Styled maroon banner (`#772432`) + `console.warn()` for each violation with path name, level number, and project name as context
- **Returns:** `violations` array (empty on PASS, populated on FAIL) — enables programmatic use

## Deviations from Plan

None - plan executed exactly as written.

## Known Stubs

None.

## Threat Flags

None — validator is read-only, no new network endpoints or auth paths introduced.

## Self-Check: PASSED

- `window.validatePathwaysData` exists at line 742 of pathways-app.html
- Commit d2cab69 confirmed in git log
- All five check categories present in implementation
- Function is callable from console with no arguments
