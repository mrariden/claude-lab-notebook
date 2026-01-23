---
description: Initialize the note-taking protocol in the current project
allowed-tools: [Read, Write, Glob, Grep, Bash]
---

You are helping the user set up the note-taking protocol for their project.

## Task

Create the complete directory structure and files for the progressive disclosure note-taking system.

## Steps

1. **Check current directory**
   - Verify we're in a project root (look for .git or ask user)

2. **Create directory structure**
   ```bash
   mkdir -p .claude/rules
   mkdir -p notes/{experiments,decisions,troubleshooting,research}
   mkdir -p configs
   ```

3. **Create .claude/rules/note-taking-protocol.md**
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

6. **Create notes/README.md**
   - Quick guide to the system
   - How to use the directories

7. **Optional: Add to .gitignore**
   - Ask user if they want notes/local/ in .gitignore for personal notes

8. **Confirm completion**
   - List all created files
   - Explain next steps
   - Remind them Claude will auto-read .claude/rules/ on next session

## .claude/rules/note-taking-protocol.md Content

Use this EXACT content for .claude/rules/note-taking-protocol.md:

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
│   └── rules/
│       └── note-taking-protocol.md   # This file (auto-loaded)
├── notes/
│   ├── INDEX.md                       # Master index - READ FIRST
│   ├── quick-reference.md             # Current working state
│   ├── experiments/                   # Chronological experiment logs
│   │   └── YYYY-MM-DD-description.md
│   ├── decisions/                     # Why key choices were made
│   │   └── YYYY-MM-topic.md
│   ├── troubleshooting/              # Error patterns & solutions
│   │   └── topic-errors.md
│   └── research/                      # External references
│       └── topic.md
└── configs/                           # Working configurations
~~~

## Workflow for Every Session

### Session Start

1. **Read `notes/INDEX.md`** - This is mandatory, always do this first
2. **Read `notes/quick-reference.md`** - Get current state
3. **Ask user**: "What are you working on today?"
4. **Based on their answer**, suggest relevant notes to load:
   - "Should I read `experiments/2025-01-10-lr-sweep.md` for context on learning rates?"
   - "Should I check `troubleshooting/cuda-errors.md` for GPU issues?"
5. **Load only relevant files** using the `view` tool

### During Work

- Keep rough notes of what you're trying
- Document failures as they happen (these are valuable)
- Track configuration changes

### Session End

**Ask user which type of note to create:**

1. **Experiment note** (most common)
   - Use `templates/experiment-template.md`
   - Create in `notes/experiments/YYYY-MM-DD-description.md`
   - Document: goal, what tried, what worked, what failed, next steps

2. **Decision note** (for architectural/design choices)
   - Use `templates/decision-template.md`
   - Create in `notes/decisions/YYYY-MM-topic.md`
   - Document: context, options considered, decision, rationale, trade-offs

3. **Troubleshooting note** (for error patterns)
   - Use `templates/troubleshooting-template.md`
   - Create in `notes/troubleshooting/topic-errors.md`
   - Document: symptom, cause, solution, prevention

**After creating note, ask:**
- "Should I update `notes/INDEX.md`?" (if significant finding)
- "Should I update `notes/quick-reference.md`?" (if current best practice changed)

## Context Management Rules

### When to Load Additional Files

**User says:** "I'm working on learning rates"
**You respond:** "I see from INDEX.md we did LR experiments on 2025-01-10. Should I read `experiments/2025-01-10-lr-sweep.md` for details?"

**User says:** "I'm getting a CUDA error"
**You respond:** "Should I check `troubleshooting/cuda-errors.md` for known solutions?"

**User says:** "Why did we choose this architecture?"
**You respond:** "Should I read `decisions/2025-01-12-architecture.md` for the rationale?"

### What NOT to Do

❌ Don't say: "Let me read all your experiment files"
❌ Don't load multiple files without asking
❌ Don't read entire files when you only need a section
❌ Don't skip reading INDEX.md (it's always required)

✅ Do say: "Based on INDEX.md, I found relevant info in X. Should I read it?"
✅ Do use `view` tool with line ranges for large files
✅ Do ask user to specify which time period of experiments matters
✅ Do read INDEX.md and quick-reference.md at session start

## File Naming Conventions

- **Experiments**: `YYYY-MM-DD-brief-description.md` (chronological)
- **Decisions**: `YYYY-MM-topic-name.md` (topical)
- **Troubleshooting**: `topic-name-errors.md` or `error-type.md` (topical)
- **Research**: `topic-name.md` or `paper-name.md` (topical)

Always use lowercase with hyphens, no spaces.

## File Format Standards

### Experiment Notes

- Goal: What you were trying to achieve
- What I Tried: Each approach with results
- Key Findings: What worked, what surprised you
- **Failed Approaches**: Table format (most valuable section!)
- Configuration Used: Copy-pasteable config
- Next Steps: Follow-up tasks
- References: Links to related notes

### Decision Notes

- Context: Why decision was needed
- Options Considered: Pros/cons for each
- Decision: What was chosen
- Rationale: Why it was chosen
- Trade-offs: What you're giving up
- When to Revisit: Conditions for reconsidering

### Troubleshooting Notes

- Symptom: Exact error message
- Cause: Why it happens
- Solution: Step-by-step fix
- Prevention: How to avoid
- Related Issues: Cross-references

## Progressive Disclosure in Practice

**Example efficient session:**

~~~
[Session starts]
Claude: [Reads notes/INDEX.md - 1K tokens]
Claude: [Reads notes/quick-reference.md - 500 tokens]
Claude: "I've loaded your project notes index. What are you working on?"

User: "I want to improve training speed"

Claude: "From INDEX.md, I see we explored batch size on 2025-01-09
and data loading on 2025-01-11. Should I read those experiments?"

User: "Yes, read the batch size one"

Claude: [Uses view on notes/experiments/2025-01-09-batch-size.md - 2K tokens]
Claude: "Based on that experiment, batch_size=32 works best.
Going higher causes OOM. Want to try a different approach?"

[Works on task...]

[Session ends]
Claude: "Should I create an experiment note for today's work?"

User: "Yes"

Claude: [Creates notes/experiments/2025-01-15-dataloader-optimization.md]
Claude: "The experiment note has been created. Should I update INDEX.md
since we found DataLoader num_workers=4 is optimal?"

Total context used: ~5K tokens instead of 50K+ if all files loaded
~~~

## Critical Reminders

1. **INDEX.md is your map** - Always read it first
2. **Failed attempts are valuable** - Document them thoroughly
3. **Be specific** - "LR=0.001 causes NaN" not "LR too high"
4. **Include exact errors** - Full stack traces, not summaries
5. **Ask before loading** - Don't waste context on irrelevant files
6. **Use line ranges** - `view file.md 10 50` for specific sections
7. **Date everything** - Chronology matters for understanding evolution
8. **Copy-pasteable configs** - Not vague advice like "use small learning rate"
9. **Link related notes** - Build a knowledge graph
10. **Update quick-reference** - Keep current best practices fresh

## Integration with Other Tools

- **Configs**: Reference actual config files, don't duplicate
- **Code**: Link to specific files/functions when relevant
- **External tools**: Note tool versions (W&B, TensorBoard, etc.)
- **Hardware**: Document GPU models, RAM, when relevant

## When User Has Existing Notes

If user has existing markdown notes not yet in this structure:

1. **Ask to see** their current notes directory
2. **Propose migration plan**:
   - Create notes/ directory structure
   - Sort existing notes by type (experiment/decision/troubleshooting)
   - Add dates to filenames where possible
   - Create INDEX.md by scanning existing notes
   - Create quick-reference.md from current state
3. **Execute migration** with user approval
4. **Verify** INDEX.md links work
5. **Test** by asking user a question and using new structure

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

## All Experiments
<!-- Each entry has: Title, Tags, Finding, File link -->
<!-- Example:
**2025-01-15: Optimizer comparison**
- Tags: optimizer, adamw, sgd, training
- Finding: AdamW 15% better than SGD for this architecture
- File: [experiments/2025-01-15-optimizer-comparison.md](experiments/2025-01-15-optimizer-comparison.md)
-->

## All Decisions
<!-- Example:
**2025-01-14: Choose AdamW optimizer**
- Tags: optimizer, architecture, performance
- Finding: AdamW chosen for stability and performance
- File: [decisions/2025-01-14-optimizer-choice.md](decisions/2025-01-14-optimizer-choice.md)
-->

## All Troubleshooting Guides
<!-- Example:
**CUDA out of memory errors**
- Tags: cuda, memory, gpu, oom
- Finding: Reduce batch_size to 32 or lower for RTX 3090
- File: [troubleshooting/cuda-memory-errors.md](troubleshooting/cuda-memory-errors.md)
-->

## All Research
<!-- Example:
**Attention mechanisms**
- Tags: attention, transformer, papers
- Finding: Multi-head attention key for performance
- File: [research/attention-mechanisms.md](research/attention-mechanisms.md)
-->
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
- .claude/rules/note-taking-protocol.md (auto-loaded protocol)
- notes/INDEX.md (your knowledge map)
- notes/quick-reference.md (current state)
- notes/{experiments,decisions,troubleshooting,research}/ (organized by type)
- configs/ (configuration file storage)

📝 Templates:
Built-in templates available:
- experiment-template
- decision-template
- troubleshooting-template

💡 To customize or create new note types:
- Run `/create-note-type` to create a new custom note type
- Or manually create templates in `.claude/templates/`:
  mkdir -p .claude/templates
  vim .claude/templates/my-type-template.md

Custom templates in .claude/templates/ override built-in templates with matching names.

🚀 Next steps:
1. Start your next Claude Code session
2. I'll automatically read .claude/rules/ and follow the protocol
3. At the end of your session, I'll ask if you want to create a note

💡 How it works:
- I'll read INDEX.md first every session (small, fast)
- I'll ask before loading other notes (progressive disclosure)
- Create notes using the templates
- Update INDEX.md when you discover something important

The system optimizes for findability - you'll never repeat the same mistake twice!"

## Error Handling

- If directories exist: Ask before overwriting
- If not in project root: Warn and ask for confirmation
