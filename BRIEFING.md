# BRIEFING

## Project
Single HTML file (pathways-app.html) — Toastmasters Pathways reference app.
Hosted on FTH at 4039.toastmastersclubs.org. All CSS/JS embedded. No frameworks.

## Current Position
Phase 2: COMPLETE ✓
Minification: COMPLETE ✓ (64,964 chars — 569 under FTH 65,533 limit)
Phase 3: Polish — in progress
Eval forms: All 66 current-path PDFs wired up in EVAL_FORMS (13 new entries added 2026-05-31)
Footer fix: COMPLETE (df2ba0e) — replaced <footer> tag with <div style="display:block!important;...">
Next task: Upload pathways-app.html to FTH and verify footer is visible

## Last Session (2026-05-31)
- Minified pathways-app.html from 118,980 → 57,194 chars (now 58,101 after eval forms)
- Techniques: CSS/JS comment strip, terser, JSON key renames (name→n etc.),
  type values Required→R/Elective→E, purpose deduplication (244→105 unique, pL lookup)
- validatePathwaysData() intact and uses renamed fields
- Badge display decodes R/E back to "Required"/"Elective" for users
- Backup saved as pathways-app.full.html (original readable source)
- Committed and pushed (69c3840)
- Fixed footer modal bleed: added #pathways-modal footer{display:none!important} to CSS
- Fixed corrupted unicode in footer (u{1F514}, u{2014}, u{2019} → proper UTF-8)
- Removed bell emoji from footer to save chars (8ce3a1b)

## Key Files
- pathways-app.html — minified app for FTH upload (57,194 chars)
- pathways-app.full.html — full readable backup (118,980 chars, original field names)
- eval-forms/ — all evaluation form PDFs
- .planning/phases/ — plan files
- STATE.md — detailed state

## Session Rules
- Never read pathways-app.html in full — grep for specific sections
- Read pathways-app.full.html for human-readable source inspection
- Never read CLAUDE.md unless hitting architectural question
- Always read BRIEFING.md only at session start
- Update BRIEFING.md at end of every session
- Keep commits small and descriptive

## JSON Schema (minified keys)
name→n, purpose→p, type→y (values: R/E), time→t, levels→v, level→l,
projects→j, enhancement→e, completionRequirement→c, roles→r, notes→z
Purpose strings deduplicated: integer index into root pL array
JS: PL=_pd.pL; then PL[project.p] to get purpose text
