---
description: Initialize the note-taking protocol in the current project
argument-hint: <required-arg> [optional-arg]
allowed-tools: [Read, Write, Glob, Grep, Bash]
---

You are helping the user set up the note-taking protocol for their project.

## Task

Create the complete directory structure and files for the progressive disclosure note-taking system.

## Steps

1. **Check current directory**
   - Verify we're in a project root (look for .git or ask user)
   - Check if CLAUDE.md already exists (warn if it does)

2. **Create directory structure**
   ```bash
   mkdir -p notes/{experiments,decisions,troubleshooting,research}
   mkdir -p configs
   mkdir -p templates
   ```

3. **Create CLAUDE.md**
   - Full protocol with progressive disclosure rules
   - Context efficiency guidelines
   - Workflow instructions

4. **Create notes/INDEX.md**
   - Master index template
   - Dated with current date
   - Empty sections ready to fill

5. **Create notes/quick-reference.md**
   - Current state template
   - Dated with current date
   - Sections for what works, what doesn't, open questions

6. **Create templates**
   - templates/experiment-template.md
   - templates/decision-template.md
   - templates/troubleshooting-template.md

7. **Create notes/README.md**
   - Quick guide to the system
   - How to use the directories

8. **Optional: Add to .gitignore**
   - Ask user if they want notes/local/ in .gitignore for personal notes

9. **Confirm completion**
   - List all created files
   - Explain next steps
   - Remind them Claude will auto-read CLAUDE.md on next session

## CLAUDE.md Content

Use this EXACT content for CLAUDE.md:

```markdown
# Project Note-Taking Protocol

## Context Efficiency Protocol (CRITICAL)

**Progressive Disclosure System**: Never load all notes at once. Follow this strict hierarchy:

1. **ALWAYS start by reading** `notes/INDEX.md` (quick reference, ~200 lines max)
2. **Then read** `notes/quick-reference.md` (current working state)
3. **Ask before loading** any other files
4. **Use view tool with line ranges** when you only need specific sections
5. **Never load** entire `notes/` directory at once

## Workflow for Every Session

### Session Start
1. Read `notes/INDEX.md` - mandatory
2. Read `notes/quick-reference.md` - current state
3. Ask user: "What are you working on today?"
4. Based on answer, suggest relevant notes to load
5. Load only relevant files using `view` tool

### Session End
Ask user which type of note to create:
1. Experiment note → `notes/experiments/YYYY-MM-DD-description.md`
2. Decision note → `notes/decisions/YYYY-MM-topic.md`
3. Troubleshooting note → `notes/troubleshooting/topic-errors.md`

After creating note, ask:
- "Should I update `notes/INDEX.md`?"
- "Should I update `notes/quick-reference.md`?"

## File Naming Conventions
- Experiments: `YYYY-MM-DD-brief-description.md` (chronological)
- Decisions: `YYYY-MM-topic-name.md` (topical)
- Troubleshooting: `topic-name-errors.md` (topical)
- Research: `topic-name.md` (topical)

## Critical Reminders
1. INDEX.md is your map - always read it first
2. Failed attempts are valuable - document thoroughly
3. Be specific - exact errors, exact parameters
4. Ask before loading - don't waste context
```

## Templates Content

### experiment-template.md
```markdown
# YYYY-MM-DD: Descriptive Title

## Goal
What I was trying to achieve

## What I Tried
1. Approach A → Result
2. Approach B → Result

## Key Findings
- What worked
- What surprised me

## Failed Approaches (CRITICAL)
| What I Tried | Why It Failed | Lesson Learned |
|--------------|---------------|----------------|
| Thing | Error | Takeaway |

## Configuration Used
```yaml
# Copy-pasteable config
```

## Next Steps
- [ ] Follow-up task
```

### decision-template.md
```markdown
# Decision: Title

Date: YYYY-MM-DD

## Context
Why decision needed

## Options Considered
1. Option A - Pros/Cons
2. Option B - Pros/Cons

## Decision
**Chose: X**

## Rationale
- Reason 1
- Reason 2

## Trade-offs
- What we're giving up

## When to Revisit
- Conditions for reconsideration
```

### troubleshooting-template.md
```markdown
# Troubleshooting: Error Name

## Symptom
```
Exact error message
```

## Cause
Why this happens

## Solution
1. Step one
2. Step two

## Prevention
- How to avoid
```

## INDEX.md Template

```markdown
# Project Notes Index

Last updated: [TODAY'S DATE]

## Quick Links
- [Current Best Config](../configs/)
- [Quick Reference](quick-reference.md)

## What Works (Summary)
- Add successful approaches here

## Known Issues  
- Add problems here

## Recent Activity (Last 7 days)
- [TODAY]: Initialized note-taking system

## All Experiments
<!-- Chronological list -->

## All Decisions
<!-- Topical list -->

## All Troubleshooting Guides
<!-- Topical list -->
```

## quick-reference.md Template

```markdown
# Quick Reference - Current Project State

Last updated: [TODAY'S DATE]

## What's Working Right Now
```yaml
# Add current working config
```

## Don't Do These (Failed Approaches)
- ❌ Add failures here

## Current Best Practices
- ✓ Add successes here

## Open Questions
- [ ] Things to investigate

## Last Updated
- [TODAY]: Initialized system
```

## After Setup

Tell the user:

"✅ Note-taking protocol installed!

📁 Created:
- CLAUDE.md (protocol instructions)
- notes/INDEX.md (your knowledge map)
- notes/quick-reference.md (current state)
- notes/{experiments,decisions,troubleshooting,research}/ (organized by type)
- templates/ (note templates)

🚀 Next steps:
1. Start your next Claude Code session
2. I'll automatically read CLAUDE.md and follow the protocol
3. At the end of your session, I'll ask if you want to create a note

💡 How it works:
- I'll read INDEX.md first every session (small, fast)
- I'll ask before loading other notes (progressive disclosure)
- Create notes using the templates
- Update INDEX.md when you discover something important

The system optimizes for findability - you'll never repeat the same mistake twice!"

## Error Handling

- If CLAUDE.md exists: Offer to back it up first
- If directories exist: Ask before overwriting
- If not in project root: Warn and ask for confirmation
