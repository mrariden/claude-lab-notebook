# Quick Start Guide: Note-Taking Protocol

## TL;DR

This system helps you capture and retrieve experimental knowledge efficiently using progressive disclosure - Claude only loads what it needs, when it needs it.

## 5-Minute Setup

### Option A: Using the Plugin (Recommended)

```
# In Claude Code, run:
/setup-notes
```

That's it! The command creates everything you need:
- `CLAUDE.md` - Protocol instructions
- `notes/` - Complete directory structure
- `templates/` - Note templates
- Starter files (INDEX.md, quick-reference.md)

**Have existing notes?** Run `/migrate-notes` after setup to organize them.

### Option B: Manual Setup

<details>
<summary>Click to expand manual setup steps</summary>

#### 1. Copy files to your project root
```bash
# Copy these files to your project root:
# - CLAUDE.md (the protocol)
# - templates/ directory (note templates)
```

#### 2. Create directory structure
```bash
mkdir -p notes/{experiments,decisions,troubleshooting,research}
mkdir -p configs
```

#### 3. Create starter files
```bash
# Create INDEX.md
cat > notes/INDEX.md << 'EOF'
# Project Notes Index

Last updated: $(date +%Y-%m-%d)

## Quick Links
- [Current Best Config](../configs/)

## What Works (Summary)
- Add findings here as you discover them

## Known Issues
- Add problems here as you encounter them

## Recent Activity (Last 7 days)
- $(date +%Y-%m-%d): Started note-taking system

For full details, read specific files listed above.
EOF

# Create quick-reference.md
cat > notes/quick-reference.md << 'EOF'
# Quick Reference - Current Project State

Last updated: $(date +%Y-%m-%d)

## What's Working Right Now
```yaml
# Add your current working config here
```

## Don't Do These (Failed Approaches)
- Add failures here as you learn

## Current Best Practices
- Add successful patterns here

## Open Questions
- [ ] Things to investigate
EOF
```

</details>

### Start using it
In your next Claude Code session, Claude will automatically read CLAUDE.md and follow the protocol.

## Your First Session

**What Claude will do automatically:**
1. Read `notes/INDEX.md` (quick scan)
2. Read `notes/quick-reference.md` (current state)
3. Ask what you're working on
4. Suggest relevant notes to load

**What you do:**
1. Work on your task
2. At the end, say: "Create an experiment note"
3. Claude creates the note and asks if INDEX should be updated

## Daily Workflow

**Morning:**
```
You: [Start Claude Code in project]
Claude: [Reads INDEX.md + quick-reference.md]
Claude: "What are you working on today?"
You: "Testing different learning rates"
Claude: "Should I read experiments/2025-01-10-lr-sweep.md for context?"
```

**During work:**
- Claude helps with task
- Takes mental notes of findings

**Evening:**
```
You: /create-note
Claude: "What type? (experiment/decision/troubleshooting)"
You: experiment
Claude: [Creates notes/experiments/2025-01-15-lr-test.md]
Claude: "Should I update INDEX.md?"
You: yes
Claude: [Updates INDEX.md with new finding]
```

## File Types Explained

### INDEX.md
**Purpose:** Master index of all knowledge (read first, always)
**Size:** ~200 lines max
**Content:** Summaries with links to detailed notes
**Update:** When you discover something important

### quick-reference.md  
**Purpose:** Current working state
**Size:** ~100 lines max
**Content:** What works now, what doesn't, copy-paste configs
**Update:** When best practices change

### experiments/YYYY-MM-DD-topic.md
**Purpose:** Record what you tried
**Size:** As long as needed
**Content:** Goal, attempts, results, failures, next steps
**Create:** After each work session

### decisions/YYYY-MM-topic.md
**Purpose:** Document why you made key choices
**Size:** ~100-300 lines
**Content:** Context, options, decision, rationale, trade-offs
**Create:** When making architectural/design decisions

### troubleshooting/topic-errors.md
**Purpose:** Solutions to errors you've encountered
**Size:** ~50-200 lines
**Content:** Symptom, cause, solution, prevention
**Create:** When you solve a tricky bug

## Key Concepts

### Progressive Disclosure
Claude doesn't load everything at once. It:
1. Starts with INDEX.md (small)
2. Asks before loading more
3. Uses only what's needed

This keeps context usage minimal.

### Failed Attempts Are Gold
The most valuable part of any note is the "Failed Approaches" table:
```markdown
| What I Tried | Why It Failed | Lesson Learned |
|--------------|---------------|----------------|
| LR=0.01 | NaN loss at step 50 | LR > 0.001 unstable |
```

### Specificity Matters
❌ "Use small learning rate"
✅ "LR=0.0003 works, LR=0.001 causes NaN at step ~50"

❌ "GPU ran out of memory"  
✅ "batch_size=64 causes 'CUDA OOM: 16GB required' on RTX 3090"

## Common Questions

**Q: Do I need to fill out every section of a template?**
A: No. Only fill what's relevant. Skip sections if they don't apply.

**Q: How often should I update INDEX.md?**
A: When you discover something worth remembering. Not every session needs an INDEX update.

**Q: What if I forget to create a note?**
A: Create it next session. Better late than never. Date it when the work actually happened.

**Q: Can I have sub-directories in experiments/?**
A: Yes, but keep it simple. Usually chronological order (YYYY-MM-DD) is enough.

**Q: What goes in configs/ vs embedded in notes?**
A: configs/ = reusable configuration files you actually run
Notes = snapshots and explanations of what you tried

**Q: Should I create a decision note for every choice?**
A: No. Only for significant decisions you might reconsider later or that others need to understand.

## Example Note Snippets

### Good Experiment Note
```markdown
# 2025-01-15: Learning Rate Comparison

## Goal
Find optimal LR for current 4-layer architecture

## What I Tried
1. LR=0.01
   - Training config: batch=32, warmup=100
   - Result: NaN loss at step 50
   
2. LR=0.001  
   - Training config: batch=32, warmup=100
   - Result: Loss 0.45 after 500 steps
   
3. LR=0.0003
   - Training config: batch=32, warmup=100
   - Result: Loss 0.23 after 500 steps ✓ BEST

## Failed Approaches
| What I Tried | Why It Failed | Lesson Learned |
|--------------|---------------|----------------|
| LR=0.01 | NaN loss at step 50 | This architecture can't handle LR > 0.001 |
| No warmup with LR=0.001 | Unstable first 100 steps | Always use warmup for this model |

## Configuration Used
See configs/lr-sweep.yaml
```

### Good Troubleshooting Note
```markdown
# Troubleshooting: CUDA Out of Memory

## Symptom
```
RuntimeError: CUDA out of memory. 
Tried to allocate 2.00 GiB (GPU 0; 15.78 GiB total capacity)
```

## Cause
batch_size=64 requires ~16GB VRAM, but RTX 3090 only has 15.78GB usable

## Solution
Reduce batch_size to 32:
```yaml
training:
  batch_size: 32  # Was 64
```

## Prevention
- ✓ Keep batch_size ≤ 32 for RTX 3090
- ✓ Monitor GPU memory with nvidia-smi
```

## Migrating Existing Notes

If you have existing notes, use the `/migrate-notes` command:

```
You: /migrate-notes
Claude: "Where are your existing notes located?"
You: ./docs
Claude: [Scans and analyzes files]
Claude: [Presents migration plan]
You: yes
Claude: [Moves files, updates INDEX.md]
```

For detailed manual migration steps, see MIGRATION-GUIDE.md.

## Tips for Success

1. **Write notes immediately** - Don't wait until tomorrow
2. **Be specific** - Exact errors, exact parameters
3. **Document failures** - They're more valuable than successes
4. **Link related notes** - Build a knowledge graph
5. **Keep INDEX.md current** - It's your entry point
6. **Update quick-reference** - Keep it truthful
7. **Use templates** - They ensure consistency
8. **Ask Claude to help** - It can create notes from your session

## Getting Help

Ask Claude Code:
- "Show me an example experiment note"
- "Help me decide what type of note to create"
- "Search INDEX.md for learning rate experiments"
- "What should I put in quick-reference.md?"
- "Create a troubleshooting note for this error"

## Next Steps

1. ⬜ Run `/setup-notes` to initialize
2. ⬜ Run `/migrate-notes` if you have existing notes
3. ⬜ Do your next work session
4. ⬜ Run `/create-note` to document findings
5. ⬜ Run `/update-index` to maintain organization
6. ⬜ Build the habit

---

**Remember:** This system exists to help future-you find what past-you learned. Every note is an investment in not repeating mistakes.

Start simple, build the habit, refine as you go.
