# New Project Setup — One Prompt

Copy this into Claude Code at the start of any new project.
Replace all [bracketed] text before pasting.

---

## SETUP PROMPT — paste into first Claude Code session:

```
Single task: Initialize this project with the Lean Session framework.

Do NOT read any existing files.

Create these 3 files:

1. BRIEFING.md:
---
# BRIEFING
## Project
[one sentence description and main constraint]
## Current Position
Phase: 1 of [total]
Status: Ready to start
Next task: [very first task]
## Last Session
Project just initialized.
## Key Files
- [main deliverable] — grep only, never read in full
- CLAUDE.md — architectural questions only
- HANDOFF.md — emergency recovery only
## Session Rules
- Read BRIEFING.md only at session start
- Use /gsd-execute-phase for planned work, /gsd-quick for small fixes
- Update BRIEFING.md every session
- One task per session
- Commit after every task
---

2. CLAUDE.md:
---
# Project Context
## What This Is
[2-3 sentences]
## Stack
[list technologies]
## Architecture
[data flow and key components]
## Conventions
[naming and formatting rules]
## Critical Decisions
| Decision | Why |
|---|---|
| [decision] | [reason — locked] |
---

3. HANDOFF.md:
---
# Handoff
## Date
[today's date]
## Project
[one line summary]
## Current State
Not started.
## Next Task
[first task]
## Critical Context
[anything essential]
## Known Blockers
None.
---

After creating all 3 files:
- Run: git init (if not already initialized)
- Create .gitignore with: node_modules, .DS_Store, Thumbs.db, *.log
- git add .
- git commit -m "Project initialized with Lean Session framework"
- Stop and confirm all 3 files created.
```
