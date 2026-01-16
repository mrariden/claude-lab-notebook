# Experiment Notebook - Claude Code Plugin

**Category:** Productivity | Knowledge Management  
**Version:** 2.1.0  
**License:** MIT

## Overview

Progressive disclosure note-taking system designed for research, experiments, and technical decision tracking. Organizes knowledge hierarchically to minimize context usage while maintaining full access to your experimental history.

## Key Features

✅ **Progressive Disclosure** - Loads only what's needed (INDEX first, then specific notes on demand)  
✅ **Three Note Types** - Experiments (chronological), Decisions (why you chose), Troubleshooting (how you fixed)  
✅ **Template-Based** - Auto-dated, pre-structured notes with proper formatting  
✅ **Smart Indexing** - Automatic consolidation and pattern extraction from multiple experiments  
✅ **Context Efficient** - Uses ~5K tokens per session vs 50K+ loading all notes  
✅ **Failed Approaches First** - Emphasizes documenting what didn't work to avoid repeating mistakes

## Installation

```bash
/plugin marketplace add mrariden/claude-lab-notebook
/plugin install claude-lab-notebook
```
```


## Quick Start

1. **Initialize in your project:**
   ```
   /setup-notes
   ```
   Creates complete directory structure, templates, and starter files.

2. **Do your work**, then document findings:
   ```
   /create-note
   ```
   Choose experiment/decision/troubleshooting, Claude creates properly formatted note.

3. **Maintain organization:**
   ```
   /update-index
   ```
   Claude consolidates recent findings and updates your knowledge map.

## Commands

### `/setup-notes`
**What it does:** Initializes the complete note-taking system in your project

**Creates:**
- `.claude/rules/note-taking-protocol.md` - Protocol instructions for Claude
- `notes/INDEX.md` - Master knowledge index
- `notes/quick-reference.md` - Current working state  
- `notes/{experiments,decisions,troubleshooting,research}/` - Organized directories
- `templates/` - Note templates with proper structure
- `configs/` - Configuration file storage

**Usage:**
```
You: /setup-notes
Claude: ✅ Complete system installed!
        Created .claude/rules/note-taking-protocol.md, notes/, templates/
        Ready to use.
```

### `/create-note`
**What it does:** Creates properly formatted notes using templates

**Features:**
- Auto-fills current date
- Pre-fills from conversation context
- Suggests proper filename format
- Offers to update INDEX.md
- Links to related notes

**Usage:**
```
You: /create-note
Claude: What type? (experiment/decision/troubleshooting/research)
You: experiment
Claude: Brief description?
You: learning rate sweep
Claude: [Creates notes/experiments/2025-01-13-learning-rate-sweep.md]
Claude: Should I update INDEX.md?
```

### `/update-index`
**What it does:** Maintains INDEX.md with recent findings and patterns

**Modes:**
- Add single experiment/decision
- Consolidate multiple notes (scans last 7 days)
- Extract common patterns
- Clean up (remove old entries, fix broken links)

**Usage:**
```
You: /update-index
Claude: What to add?
You: consolidate recent experiments
Claude: [Scans experiments/ from last week]
Claude: Found these patterns:
        ✅ LR=0.0003 works (3 experiments)
        ❌ Batch >64 causes OOM (2 experiments)
        Should I add to INDEX.md?
```

## How It Works

### Progressive Disclosure Architecture

**Session Start (minimal context):**
```
1. Claude reads .claude/rules/note-taking-protocol.md (protocol instructions)
2. Claude reads notes/INDEX.md (~1K tokens)
3. Claude reads notes/quick-reference.md (~500 tokens)
Total: ~2K tokens before any work
```

**During Work (on-demand loading):**
```
Claude: "What are you working on?"
You: "Testing optimizers"
Claude: [Checks INDEX.md]
Claude: "Should I read experiments/2025-01-12-optimizer-test.md?"
You: "Yes"
Claude: [Uses view tool to read specific file - ~2K tokens]
Total: ~4K tokens for entire session
```

**Compare to loading all notes at start:** 50K+ tokens

### Directory Structure

```
your-project/
├── .claude/
│   └── rules/
│       └── note-taking-protocol.md    # Protocol Claude follows (auto-loaded)
├── notes/
│   ├── INDEX.md                       # Master index (read first, always)
│   ├── quick-reference.md             # Current best config
│   ├── experiments/                   # What you tried (chronological)
│   │   └── YYYY-MM-DD-description.md
│   ├── decisions/                     # Why you chose (topical)
│   │   └── YYYY-MM-topic.md
│   ├── troubleshooting/              # How you fixed (topical)
│   │   └── topic-errors.md
│   └── research/                      # External references
│       └── topic.md
├── configs/                           # Working configurations
└── templates/                         # Note templates
```

### Note Templates

**Experiment Template:**
- Goal
- What I Tried (with results)
- Key Findings
- **Failed Approaches** (table format - most valuable section)
- Configuration Used (copy-pasteable)
- Next Steps
- References

**Decision Template:**
- Context
- Options Considered (pros/cons)
- Decision + Rationale
- Trade-offs
- When to Revisit

**Troubleshooting Template:**
- Symptom (exact error)
- Cause
- Solution (step-by-step)
- Prevention
- Related Issues

## Use Cases

### ML Research
- Track hyperparameter sweeps
- Document architecture experiments
- Record failed approaches to avoid repetition
- Link decisions to supporting experiments

### Software Development
- Log debugging sessions
- Document API design decisions
- Track performance optimizations
- Maintain troubleshooting guides

### General R&D
- Scientific experiments
- Product testing
- A/B test results
- Technical investigations

## Best Practices

1. **Write notes immediately** - Don't wait until tomorrow
2. **Be brutally specific** - "LR=0.0003" not "small learning rate"
3. **Document failures first** - They're more valuable than successes
4. **Update INDEX weekly** - Keep your knowledge map current
5. **Link liberally** - Build a knowledge graph
6. **Use templates** - Consistency makes retrieval easier

## Philosophy

This system optimizes for **findability over storage**.

The question isn't "Did I write this down?"  
The question is "Can I find it when I need it?"

Every note should answer: "How will future-me find this?"

## Context Efficiency Comparison

**Without this system:**
```
[Session starts]
Claude loads all notes: 50K+ tokens
Result: Slow, expensive, hits limits quickly
```

**With this system:**
```
[Session starts]
INDEX.md: 1K tokens
quick-reference.md: 500 tokens
[User asks about topic]
Specific note: 2K tokens
Total: ~3.5K tokens, can work for hours
```

## Requirements

- Claude Code 2.0.0 or higher
- Git repository (optional but recommended)
- Markdown editor (optional, any text editor works)

## Migration from Existing Notes

See MIGRATION-GUIDE.md for detailed instructions on organizing existing notes into this structure.

**Quick migration:**
```
You: "Help me migrate my existing notes"
Claude: [Scans current notes]
Claude: [Proposes categorization]
Claude: [Creates INDEX.md from existing content]
```

## Integration with Other Tools

**Git:**
```bash
git add notes/
git commit -m "Add LR sweep experiment"
```

**W&B/TensorBoard:**
```markdown
## Results
See run: https://wandb.ai/user/project/runs/abc123
```

**Obsidian (optional):**
- Notes are valid Obsidian markdown
- Graph view shows connections
- Full-text search across experiments

## Examples

See the repository for:
- Complete example notes
- Sample INDEX.md
- Real-world experiment logs
- Migration examples

## Support & Contributing

- 🐛 **Issues:** [GitHub Issues](https://github.com/mrariden/claude-lab-notebook/issues)
- 💡 **Feature Requests:** Open an issue with enhancement tag
- 🤝 **Contribute:** PRs welcome! See CONTRIBUTING.md
- 📖 **Docs:** Full documentation in repo

## Author

Created by Michael Rariden 
Inspired by Sionic AI's skills system and scientific lab notebooks

## License

MIT - Free to use and modify

---

**Start organizing your experimental knowledge today!**

```bash
/plugin install experiment-notebook
/setup-notes
```
