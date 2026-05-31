---
quick_id: 260530-lean
slug: lean-templates
date: 2026-05-30
status: complete
---

# Summary: Replace templates with complete Lean Session framework files

## What Was Done

Replaced the two original template files with a complete, usable Lean Session framework:

1. **`templates/claude-project-starter.md`** — updated with:
   - Proper code fences around each template (copy-paste ready)
   - Session Prompt Pattern block
   - At 50% Context Warning prompt
   - After Crash or Cutoff recovery prompt
   - Golden Rules table (7 rules with rationale)
   - File Size Targets table
   - New Project Checklist

2. **`templates/new-project-setup.md`** — added as a one-prompt project initializer:
   - Single paste-ready prompt that creates all 3 files, runs git init, creates .gitignore, makes first commit

3. **`templates/project-template-guide.md`** — deleted (superseded by new-project-setup.md)

## Commit

`54a6503` — docs(templates): replace with complete Lean Session framework
