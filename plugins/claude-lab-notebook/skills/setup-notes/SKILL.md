---
description: Initialize the note-taking protocol in the current project
argument-hint: [flavor]
allowed-tools: [Read, Write, Glob, Grep, Bash, AskUserQuestion]
---

You are helping the user set up the note-taking protocol for their project.

## Task

Create the complete directory structure and files for the progressive disclosure note-taking system, customized for the user's domain.

## Flavors

Flavors customize the system for different domains. Available flavors:

- **ml** (default): Machine learning experiments, hyperparameter tracking, research
- **software-engineering**: Feature spikes, architecture decisions, API development
- **devops-sre**: Incident response, runbooks, infrastructure decisions

Flavor files are located in `flavors/` directory relative to this skill.

## Steps

1. **Check for flavor argument**
   - If argument provided (e.g., `/setup-notes ml`), use that flavor
   - If no argument, prompt user to select:
     ```
     Which flavor best matches your project?
     - ml (Machine Learning) - experiments, training runs, hyperparameters
     - software-engineering - spikes, architecture, API development
     - devops-sre - incidents, runbooks, infrastructure
     ```

2. **Load the flavor file**
   - Read `flavors/{flavor}.md` to get:
     - Directory structure
     - Protocol examples
     - INDEX.md section templates
   - If flavor file not found, warn and use `ml` as default

3. **Check current directory**
   - Verify we're in a project root (look for .git or ask user)

4. **Create directory structure**
   - Create `.claude/rules` directory
   - Create `configs` directory
   - Create `notes/` with subdirectories from flavor's directory list
   - Example for ml: `notes/{experiments,decisions,troubleshooting,research}`
   - Example for software-engineering: `notes/{spikes,decisions,troubleshooting,architecture,research}`

5. **Save flavor choice**
   - Write the selected flavor name to `.claude/notebook-flavor.txt`
   - This allows other skills to know which flavor is in use

6. **Create .claude/rules/note-taking-protocol.md**
   - Use the protocol template below
   - Replace `{{DIRECTORY_STRUCTURE}}` with flavor's directory tree
   - Replace `{{PROTOCOL_EXAMPLES}}` with flavor's "When to Load Additional Files" examples
   - Replace `{{SESSION_EXAMPLE}}` with flavor's progressive disclosure session example
   - Replace `{{PRIMARY_DIR}}` with flavor's primary directory (experiments/spikes)

7. **Create notes/INDEX.md**
   - Use the INDEX.md template below
   - Include section headers from flavor's INDEX.md examples

8. **Create notes/quick-reference.md**
   - Use the quick-reference.md template below

9. **Create notes/README.md**
   - Quick guide to the system
   - How to use the directories

10. **Optional: Add to .gitignore**
    - Ask user if they want notes/local/ in .gitignore for personal notes

11. **Confirm completion**
    - List all created files
    - Explain next steps
    - Remind them Claude will auto-read .claude/rules/ on next session

## .claude/rules/note-taking-protocol.md Template

Use this template, replacing placeholders with flavor-specific content:

```markdown
# Note-Taking Protocol

*This file is automatically loaded by Claude Code from .claude/rules/*

## Context Efficiency Protocol (CRITICAL)

**Progressive Disclosure System**: Never load all notes at once. Follow this strict hierarchy:

1. **ALWAYS start by reading** `notes/INDEX.md` (quick reference, ~200 lines max)
2. **Then read** `notes/quick-reference.md` (current working state)
3. **Ask before loading** any other files
4. **Use view tool with line ranges** when you only need specific sections
5. **Never load** entire `notes/` directory at once

## Directory Structure

~~~
project/
├── .claude/
│   ├── templates/                    # Notes template dir
│   ├── notebook-flavor.txt           # Selected flavor
│   └── rules/
│       └── note-taking-protocol.md   # This file (auto-loaded)
├── notes/
│   ├── INDEX.md                       # Master index - READ FIRST
│   ├── quick-reference.md             # Current working state
{{DIRECTORY_STRUCTURE}}
└── configs/                           # Working configurations
~~~

## Workflow for Every Session

### Session Start

1. **Read `notes/INDEX.md`** - This is mandatory, always do this first
2. **Read `notes/quick-reference.md`** - Get current state
3. **Ask user**: "What are you working on today?"
4. **Based on their answer**, suggest relevant notes to load
5. **Load only relevant files** using the `view` tool

### When to Load Additional Files

{{PROTOCOL_EXAMPLES}}

### What NOT to Do

❌ Don't say: "Let me read all your notes"
❌ Don't load multiple files without asking
❌ Don't read entire files when you only need a section
❌ Don't skip reading INDEX.md (it's always required)

✅ Do say: "Based on INDEX.md, I found relevant info in X. Should I read it?"
✅ Do use `view` tool with line ranges for large files
✅ Do ask user to specify which time period matters
✅ Do read INDEX.md and quick-reference.md at session start

### During Work

- Keep rough notes of what you're trying
- Document failures as they happen (these are valuable)
- Track configuration changes

### Session End

**Ask user which type of note to create:**

1. **Primary note** (most common)
   - Create in `notes/{{PRIMARY_DIR}}/YYYY-MM-DD-description.md`
   - Document: goal, what tried, what worked, what failed, next steps

2. **Decision note** (for architectural/design choices)
   - Create in `notes/decisions/YYYY-MM-topic.md`
   - Document: context, options considered, decision, rationale, trade-offs

3. **Troubleshooting note** (for error patterns)
   - Create in `notes/troubleshooting/topic-errors.md`
   - Document: symptom, cause, solution, prevention

**After creating note, ask:**
- "Should I update `notes/INDEX.md`?" (if significant finding)
- "Should I update `notes/quick-reference.md`?" (if current best practice changed)

## Progressive Disclosure in Practice

{{SESSION_EXAMPLE}}

## File Naming Conventions

- **Primary notes**: `YYYY-MM-DD-brief-description.md` (chronological)
- **Decisions**: `YYYY-MM-topic-name.md` (topical)
- **Troubleshooting**: `topic-name-errors.md` or `error-type.md` (topical)
- **Research**: `topic-name.md` or `paper-name.md` (topical)

Always use lowercase with hyphens, no spaces.

## Critical Reminders

1. **INDEX.md is your map** - Always read it first
2. **Failed attempts are valuable** - Document them thoroughly
3. **Be specific** - Include exact values and error messages
4. **Ask before loading** - Don't waste context on irrelevant files
5. **Use line ranges** - `view file.md 10 50` for specific sections
6. **Date everything** - Chronology matters for understanding evolution
7. **Copy-pasteable configs** - Not vague advice
8. **Link related notes** - Build a knowledge graph
9. **Update quick-reference** - Keep current best practices fresh

---

**Remember**: This system optimizes for **findability over storage**. Every note should answer: "How will future-me find this when I need it?"
```

## INDEX.md Template

```markdown
# Project Notes Index

Last updated: [TODAY'S DATE]

## Quick Links
- [Current Best Config](../configs/)
- [Quick Reference](quick-reference.md)

## What Works (Summary)
- Add successful approaches here as you discover them

## Known Issues
- Add problems here as you encounter them

## Recent Activity (Last 7 days)
- [TODAY]: Initialized note-taking system

{{INDEX_SECTIONS}}
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

"Setup complete!

**Flavor:** {selected_flavor}

**Created:**
- `.claude/rules/note-taking-protocol.md` (auto-loaded protocol)
- `.claude/notebook-flavor.txt` (flavor configuration)
- `notes/INDEX.md` (your knowledge map)
- `notes/quick-reference.md` (current state)
- `notes/{directories from flavor}/` (organized by type)
- `configs/` (configuration file storage)

**Templates:**
Built-in templates available via `/create-note`:
- experiment/spike (depends on flavor)
- decision
- troubleshooting

**Next steps:**
1. Start your next Claude Code session
2. I'll automatically read `.claude/rules/` and follow the protocol
3. At the end of your session, I'll ask if you want to create a note

The system optimizes for findability - you'll never repeat the same mistake twice!"

## Error Handling

- If directories exist: Ask before overwriting
- If not in project root: Warn and ask for confirmation
- If flavor not found: Warn and fall back to `ml`
