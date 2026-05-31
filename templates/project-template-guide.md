# Project Template Guide

## How to Use the Starter Template

### Step 1: Copy BRIEFING.md
Copy the BRIEFING.md template into your project root.
Fill in:
- Project one-liner
- Phase number
- Next task
- Key files list

### Step 2: Copy CLAUDE.md
Copy the CLAUDE.md template into your project root.
Fill in:
- What this is (goal + deliverable + constraint)
- Technology stack
- Architecture overview
- Conventions
- Critical decisions that must not change

### Step 3: Write HANDOFF.md
Write it when context is 50% full.
Update it at the end of every session.
It is your emergency recovery document.

### Step 4: Session Rules
At every session start:
1. Read BRIEFING.md only
2. Start work
3. Read HANDOFF.md only if something seems wrong

### Step 5: End of Session
Update BRIEFING.md:
- Current position
- Last session summary
- Any new key files

### When to Update Each File
| File | When to update |
|---|---|
| BRIEFING.md | End of every session |
| CLAUDE.md | Only when architecture changes |
| HANDOFF.md | 50% context trigger + end of session |

## Common Mistakes

- Reading the full HTML file at session start — always grep instead
- Updating CLAUDE.md every session — it should be stable
- Not updating BRIEFING.md — it must stay current
- Reading HANDOFF.md every session — only for emergencies

## Template Folder Structure

/templates/
├── claude-project-starter.md   # The templates
└── project-template-guide.md   # This guide

Your project root:
/BRIEFING.md       # read every session
/CLAUDE.md         # architecture reference
/HANDOFF.md        # emergency recovery

Both files are reference documents — they do not need to be committed to source control unless you want them versioned.
