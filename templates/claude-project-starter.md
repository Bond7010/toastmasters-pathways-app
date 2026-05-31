# Claude Project Starter Template

## The 3 Files You Need

### 1. BRIEFING.md (read every session — keep under 30 lines)
---
# BRIEFING

## Project
[One sentence: what this project is and its main constraint]

## Current Position
Phase: 1
Status: Ready to start
Next task: [first task]

## Last Session
Project just initialized.

## Key Files
- [main deliverable] — never read in full, grep only
- CLAUDE.md — read only for architectural questions
- HANDOFF.md — emergency recovery only

## Session Rules
- Read BRIEFING.md only at session start
- Never read [main deliverable] in full — grep for specific sections
- Use /gsd-execute-phase for planned work, /gsd-quick for small fixes
- Update BRIEFING.md at end of every session
- One task per session
- Commit after every completed task
---

### 2. CLAUDE.md (read only for architectural questions)
---
# Project Context

## What This Is
[2-3 sentences: goal, deliverable, hosting/platform constraint]

## Technology Stack
[Language, framework or none, key libraries, build process]

## Architecture
[Data flow, key components, state management]

## Conventions
[Naming rules, formatting rules, patterns to follow]

## Critical Decisions
| Decision | Why |
|---|---|
| [decision] | [reason — locked, do not revisit] |

## Key Files
| File | Purpose |
|---|---|
| [file] | [what it does] |
---

### 3. HANDOFF.md (emergency recovery — update at 50% context)
---
# Handoff

## Date
[Date]

## Project
[One line summary]

## Current State
[What exists, what works]

## Next Task
[Exact next step]

## Critical Context
[Anything Claude must know not in other files]

## Known Blockers
[Anything needing human action]
---

## Session Prompt Pattern

Every session:
Assistant: [Read BRIEFING.md]
Then: [Read HANDOFF.md if continuing emergency state]
Then: [Start work via /gsd-quick or /gsd-execute-phase]


## Why This Works

- **BRIEFING.md** = fast orientation (30 lines, read every session)
- **CLAUDE.md** = deep context (read on-demand only)
- **HANDOFF.md** = emergency recovery (50% context trigger)
