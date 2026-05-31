---
phase: "02-data"
plan: "02-03"
subsystem: "data"
tags: ["vintage-paths", "basic-training", "communication-series", "eval-forms", "data-expansion"]
dependency_graph:
  requires: ["02-02"]
  provides: ["basic-training-data", "communication-series-data", "bt-eval-forms"]
  affects: ["pathways-app.html"]
tech_stack:
  added: []
  patterns: ["JSON data block expansion", "relative path eval form lookup"]
key_files:
  created: []
  modified:
    - pathways-app.html
decisions:
  - "basic-training and communication-series use enhancement: null (vintage paths — no Enhancement section)"
  - "Non-standard time values kept as-is (Role-based, Self-evaluation, Written, Self-study) — intentional vintage program designations"
  - "BT EVAL_FORMS entries use relative paths (no _B prefix) — local FTH-hosted PDFs"
  - "communication-series projects fall back to GENERIC_FORM — no CS-specific entries added"
metrics:
  duration: "5m"
  completed_date: "2026-05-30"
  tasks_completed: 3
  files_modified: 1
---

# Phase 2 Plan 03: Vintage Paths Data Expansion Summary

**One-liner:** Inserted Basic Training (17 projects, 5 levels) and replaced Communication Series placeholder stub with real Competent Communication + Entertaining Speaker + Storytelling data; added 12 relative-path EVAL_FORMS entries for BT local PDFs.

## Tasks Completed

| # | Task | Commit | Files |
|---|------|--------|-------|
| 1a | Insert basic-training path (5 levels, 17 projects) | ca4972f | pathways-app.html |
| 1b | Replace communication-series stub with real data | ca4972f | pathways-app.html |
| 2 | Add 12 EVAL_FORMS entries for Basic Training local PDFs | 831f1e5 | pathways-app.html |

## What Was Built

### basic-training path (new)
- **id:** basic-training
- **name:** Basic Training for Toastmasters
- **5 levels:** Introduction to Basic Training, Developing Your Skills, Building on the Fundamentals, Advancing Your Technique, Mastering the Craft
- **17 projects:** Speeches, Table Topics Chair, Toastmaster of Evening, Critic/Evaluator roles, self-evaluation, written self-reflection
- **enhancement: null** on all 5 levels (vintage path — roles listed as project items)

### communication-series path (stub replaced)
- **id:** communication-series
- **name:** Communication Series
- **5 levels:** Introduction to Competent Communication (CC), Building Core Skills, Advanced Fundamentals, The Entertaining Speaker, Storytelling
- **21 projects:** Ice Breaker through full CC + Entertaining Speaker + Storytelling manuals
- **enhancement: null** on all 5 levels
- All placeholder "Speech N", "Level N" data fully replaced

### EVAL_FORMS additions
12 Basic Training entries added with relative paths (no `_B` CDN prefix):
- The Ice Breaker, Be in Earnest, Building a Speech, Hands Up, Vocal Variety
- Speech Building, Reading the Speech, Illuminate the Speech, Working With Words
- Special Types of Speech, Organizing the Speech, Carrying On

Roles (When You Are Table Topic Chairperson, When You Are Toastmaster of the Evening, When You Are a Critic or Evaluator), self-evaluations, and written/self-study projects fall back to GENERIC_FORM automatically.

## Deviations from Plan

### Tasks 1a and 1b committed together

The plan called for two separate commits for basic-training insertion and communication-series stub replacement. Both changes were made in a single atomic edit and committed together as `feat(02-03): add Basic Training path data` (ca4972f). The subsequent commit (831f1e5) covers the EVAL_FORMS task. All data is correctly in place.

## Decisions Made

- Kept non-standard time values (Role-based, Self-evaluation, Written, Self-study) as-is — these are intentional Toastmasters program designations matching the physical materials, consistent with the 02-02 decision
- Basic Training eval forms use relative paths (`eval-forms/basic-training/...`) because they are hosted locally on FTH, not via the external CDN used for Pathways forms
- 'Vocal Variety' appears in both basic-training L3 and communication-series L2; the single EVAL_FORMS entry maps to the BT local PDF. The CS 'Vocal Variety' project has no local-specific form and falls back to GENERIC_FORM (by design — the key collision is acceptable since BT members want the BT-specific resource)

## Known Stubs

None — all data is real content from program materials.

## Blockers Resolved

- STATE.md blocker "Basic Training level names: Levels 2–5 names unknown" is now resolved. Confirmed level names: Developing Your Skills, Building on the Fundamentals, Advancing Your Technique, Mastering the Craft.

## Self-Check: PASSED

- pathways-app.html exists and contains 8 paths
- basic-training: 5 levels, enhancement: null on all, 17 projects
- communication-series: 5 levels with real names, enhancement: null on all, 21 projects
- 12 EVAL_FORMS BT entries present, no duplicate 'Vocal Variety', relative paths confirmed
- Commits ca4972f and 831f1e5 verified in git log
