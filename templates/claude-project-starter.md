# Claude Project Starter Template

## The 3 Files You Need

---

### 1. BRIEFING.md
*(Read every session — keep under 30 lines)*

```
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
```

---

### 2. CLAUDE.md
*(Read only for architectural questions)*

```
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
```

---

### 3. HANDOFF.md
*(Emergency recovery — update at 50% context)*

```
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
```

---

## Session Prompt Pattern

Every session starts with:
```
Read BRIEFING.md only.
[one specific task]
Do not read [large files] in full — grep if needed.
Update BRIEFING.md when done.
Commit: "[description]"
Stop and report.
```

---

## At 50% Context Warning

Paste this immediately:
```
Context getting full. Before stopping:
1. Update BRIEFING.md — current position and next step
2. Update HANDOFF.md — full recovery context
3. Commit everything: "Session checkpoint — [what was done]"
4. Push to GitHub
5. Stop
```

---

## After Crash or Cutoff

```
Read BRIEFING.md only.
Resume from: [next task in BRIEFING.md]
Do not redo completed work.
Check file state before assuming anything.
```

---

## Golden Rules

| Rule | Why |
|---|---|
| One task per session | Prevents context blowout |
| Read BRIEFING.md only | Saves 20-40% tokens |
| Grep don't read | Large files kill context fast |
| Commit every task | Nothing lost if session dies |
| Update BRIEFING.md every session | Free continuity |
| Fresh session each task | Clean whiteboard = full capacity |
| Update HANDOFF.md at 50% | Your safety net |

---

## File Size Targets

| File | Max lines | Why |
|---|---|---|
| BRIEFING.md | 30 | Read every session |
| CLAUDE.md | 200 | Read rarely |
| HANDOFF.md | 100 | Emergency only |
| Main deliverable | Any | Never read in full |

---

## New Project Checklist

- [ ] Fill in BRIEFING.md
- [ ] Fill in CLAUDE.md  
- [ ] Fill in HANDOFF.md
- [ ] Initialize git repo
- [ ] First commit: "Project initialized"
- [ ] Start Claude Code: "Read BRIEFING.md only."
