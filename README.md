# Note-Taking Protocol Plugin for Claude Code

A Claude Code plugin that automates setup and management of a progressive disclosure note-taking system for capturing experimental knowledge.

## What This Plugin Does

Adds four skills to Claude Code:

1. **`/setup-notes`** - Initialize the note-taking system in your project
2. **`/create-note`** - Create properly formatted experiment/decision/troubleshooting notes
3. **`/update-index`** - Maintain your knowledge base index
4. **`/migrate-notes`** - Migrate existing notes into the organized structure

## Installation

### Option 1: Claude Code Marketplace (Recommended)

The easiest way to install is via the Claude Code plugin marketplace:

```bash
# Add the marketplace (one-time setup)
/plugin marketplace add mrariden/claude-lab-notebook

# Install the plugin
/plugin install claude-lab-notebook@claude-lab-notebook
```

That's it! The commands are now available in all your projects.

To update the plugin later:
```bash
/plugin marketplace update claude-lab-notebook
```

For more information on Claude Code marketplaces, see the [official documentation](https://code.claude.com/docs/en/plugin-marketplaces).

### Option 2: Manual Installation

<details>
<summary>Click to expand manual installation options</summary>

#### Clone and Install as Local Plugin

```bash
# Clone the repository
git clone https://github.com/mrariden/claude-lab-notebook.git

# The plugin is ready to use from: claude-lab-notebook/plugins/claude-lab-notebook/
# Point your Claude Code settings to this location
```

#### Install to Global Plugins Directory

```bash
# Clone the plugin
git clone https://github.com/mrariden/claude-lab-notebook.git

# Copy to global plugins directory
mkdir -p ~/.claude/plugins
cp -r claude-lab-notebook/plugins/claude-lab-notebook ~/.claude/plugins/
```

</details>

## Quick Start

1. **Install the plugin** (choose one method above)

2. **Open Claude Code** in your project directory
   ```bash
   cd your-project
   claude
   ```

3. **Run setup command**
   ```
   /setup-notes
   ```
   
   This creates:
   - `.claude/rules/note-taking-protocol.md` - Protocol instructions
   - `notes/` - Directory structure with starter files (INDEX.md, quick-reference.md)
   - `configs/` - Configuration file storage

4. **Start using it**
   - Do your work
   - Run `/create-note` to document findings
   - Run `/update-index` to maintain organization

## Skills

### /setup-notes

Initializes the complete note-taking system in your current project.

**What it creates:**
```
your-project/
├── .claude/
│   ├── rules/
│   │   └── note-taking-protocol.md    # Protocol Claude follows (auto-loaded)
│   └── templates/                     # Optional: custom templates (override built-in)
├── notes/
│   ├── INDEX.md                       # Master knowledge index
│   ├── quick-reference.md             # Current working state
│   ├── experiments/                   # Chronological experiments
│   ├── decisions/                     # Design decisions
│   ├── troubleshooting/              # Error solutions
│   ├── meetings/                      # Meeting notes
│   └── research/                      # External references
└── configs/                           # Configuration files
```

**Usage:**
```
You: /setup-notes
Claude: [Creates complete structure]
Claude: ✅ Setup complete! Ready to use.
```

**Safety:**
- Never touches user's CLAUDE.md
- Asks before overwriting existing directories
- Safe to run multiple times

### /create-note

Creates a new note using the appropriate template.

**Usage:**
```
You: /create-note
Claude: "What type of note? (experiment/decision/troubleshooting/meeting/research)"
You: experiment
Claude: "What's the brief description?"
You: learning rate sweep
Claude: [Creates notes/experiments/2025-01-13-learning-rate-sweep.md]
Claude: "Should I update INDEX.md?"
```

**Features:**
- Auto-fills current date
- Pre-fills from conversation context
- Suggests proper filename
- Offers to update INDEX.md and quick-reference.md

**Note types:**
- **experiment** - For documenting what you tried
- **decision** - For recording why you chose something
- **troubleshooting** - For capturing how you fixed errors
- **meeting** - For recording meeting notes and action items
- **research** - For external references/papers

### /update-index

Maintains your notes/INDEX.md with recent findings.

**Usage:**
```
You: /update-index
Claude: "What would you like to add?"
You: I just finished an experiment on optimizers
Claude: [Reads the experiment note]
Claude: [Proposes updates to INDEX.md]
Claude: "Should I save these changes?"
```

**Modes:**
- **Add experiment** - Add recent experiment to index
- **Add decision** - Add decision to index
- **Consolidate** - Review recent notes and extract patterns
- **Clean up** - Remove old entries, fix broken links

**Smart consolidation:**
- Scans last 7 days of notes
- Extracts common patterns
- Groups related findings
- Proposes "What Works" and "Known Issues" updates

### /migrate-notes

Helps migrate existing notes into the organized lab notebook structure.

**Usage:**
```
You: /migrate-notes
Claude: "Where are your existing notes located?"
You: ./docs or scattered in project root
Claude: [Scans for markdown files]
Claude: [Analyzes each file to determine type]
Claude: [Presents migration plan]
Claude: "Proceed with migration?"
You: yes
Claude: [Archives originals, moves to proper locations]
Claude: [Updates INDEX.md with findings]
```

**What it does:**
1. Discovers existing markdown notes in your project
2. Analyzes each note to determine type (experiment/decision/troubleshooting/research)
3. Extracts or estimates dates from git history
4. Presents a migration plan for your approval
5. Archives originals to `notes/archive/pre-migration/`
6. Restructures content using proper templates
7. Updates INDEX.md with key findings

**When to use:**
- You have existing notes before installing this plugin
- You're adopting the system in an established project
- You want to reorganize scattered documentation

## How It Works

### Progressive Disclosure

The system uses a hierarchical approach to minimize context usage:

1. **Session Start:**
   - Claude reads `.claude/rules/note-taking-protocol.md` (the protocol)
   - Claude reads `notes/INDEX.md` (~1K tokens)
   - Claude reads `notes/quick-reference.md` (~500 tokens)
   - **Total: ~2K tokens** before doing any work

2. **During Work:**
   - Claude asks: "What are you working on?"
   - Claude suggests relevant notes from INDEX
   - Loads only what's needed using `view` tool
   - **Incremental: ~2-5K additional tokens**

3. **Session End:**
   - `/create-note` creates properly formatted note
   - `/update-index` maintains organization
   - **Result: Total ~5-10K tokens for entire session**

Compare to loading all notes at start: **50K+ tokens** before doing anything.

### File Organization

**Chronological (experiments/):**
- `2025-01-13-learning-rate-sweep.md`
- `2025-01-12-architecture-comparison.md`
- Sorted by date, easy to find recent work

**Topical (decisions/, troubleshooting/):**
- `decisions/2025-01-optimizer-choice.md`
- `troubleshooting/cuda-memory-errors.md`
- Organized by subject for quick lookup

**INDEX.md as Map:**
- Summaries with links
- "What Works" section
- "Known Issues" section
- Always read first

## Examples

### Example Session

```
[Start Claude Code in project]

Claude: [Reads .claude/rules/, INDEX.md, quick-reference.md]
Claude: "I've loaded your project notes. What are you working on?"

You: "I want to test different batch sizes"

Claude: "From INDEX.md, I see we tested this on 2025-01-09 
        and found batch_size >64 causes OOM. Should I read 
        that experiment for details?"

You: "Yes"

Claude: [Uses view on notes/experiments/2025-01-09-batch-size.md]
Claude: "Based on that experiment, batch_size=32 works best.
        The RTX 3090 runs out of memory above 64. Want to 
        try a different approach or reproduce those results?"

[Work on task...]

You: "/create-note"

Claude: "What type of note? (experiment/decision/troubleshooting)"

You: "experiment"

Claude: "What's the brief description?"

You: "tested batch size 16 vs 32"

Claude: [Creates notes/experiments/2025-01-13-batch-size-comparison.md]
Claude: [Pre-fills based on conversation]
Claude: "Should I update INDEX.md? We confirmed batch_size=32 
        is optimal."

You: "Yes"

Claude: [Adds to INDEX.md under "What Works"]
Claude: ✅ Note created and index updated!
```

### Example Note Creation

```
You: "/create-note"
Claude: "What type?"
You: "troubleshooting"
Claude: "What's the error/issue name?"
You: "CUDA out of memory"
Claude: [Creates notes/troubleshooting/cuda-memory-errors.md]
Claude: [Pre-fills from conversation context]

Content created:
# Troubleshooting: CUDA Out of Memory

Last updated: 2025-01-13

## Symptom
```
RuntimeError: CUDA out of memory. 
Tried to allocate 2.00 GiB (GPU 0; 15.78 GiB total)
```

## Cause
batch_size=64 requires ~16GB VRAM, RTX 3090 has 15.78GB

## Solution
1. Reduce batch_size to 32 or lower
2. Verify with nvidia-smi

## Prevention
- Keep batch_size ≤32 for RTX 3090
- Monitor GPU memory usage
```

## Advanced Usage

### Custom Templates

The plugin provides built-in templates that work out of the box. To customize:

```bash
# Create custom templates directory
mkdir -p .claude/templates

# Copy a built-in template to customize
# Or create your own from scratch
vim .claude/templates/experiment-template.md
```

Custom templates in `.claude/templates/` override built-in templates with matching names.

See [TEMPLATES.md](TEMPLATES.md) for detailed customization options.

### Integration with Other Tools

**Git hooks:**
```bash
# .git/hooks/post-commit
# Remind to update notes
echo "💡 Don't forget to document this in your notes!"
```

**W&B/TensorBoard:**
```markdown
# In experiment note
## Results
See W&B run: https://wandb.ai/user/project/runs/xyz123
```

**CI/CD:**
```yaml
# Check that INDEX.md is current
- name: Verify notes updated
  run: |
    if git diff --name-only | grep -q "notes/"; then
      git diff --name-only | grep -q "notes/INDEX.md" || \
        echo "⚠️ Notes changed but INDEX.md not updated"
    fi
```

### Team Usage

**Shared knowledge base:**
```bash
# Team members pull latest notes
git pull origin main

# Add their findings
/create-note
/update-index

# Push back to team
git push origin main
```

**Code review:**
- Reviewers can check notes/ for context
- Ask "Is this documented in notes/decisions/?"
- Verify troubleshooting guides exist for known issues

## Customization

### Modify the Protocol

Edit `.claude/rules/note-taking-protocol.md` to change behavior:

```markdown
# Your custom additions

## Custom Instructions
- Always check tests/ directory before experiments
- Include performance benchmarks in all experiment notes
- Tag notes with project phase (prototype/production)
```

### Add Custom Skills

Create your own skills as a local plugin in `.claude/plugins/`:

```bash
# .claude/plugins/my-skills/skills/weekly-review/SKILL.md
---
description: Review and consolidate this week's notes
---

[Your custom instructions]
```

### Extend Templates

Create custom templates in `.claude/templates/`:

```markdown
# .claude/templates/experiment-template.md

## [Your Custom Section]
- Custom field 1
- Custom field 2

## Performance Metrics
- Latency:
- Throughput:
- Resource usage:
```

## Troubleshooting

**Skill not found:**
- Verify the plugin is installed correctly
- Check that `plugins/claude-lab-notebook/.claude-plugin/plugin.json` exists
- Restart Claude Code

**Setup fails:**
- Check if you're in the right directory
- Verify write permissions

**Notes not being referenced:**
- Verify INDEX.md exists and has content
- Check that `.claude/rules/note-taking-protocol.md` exists
- Ensure proper file naming (lowercase, hyphens, dates)

**Context still too large:**
- Check that INDEX.md is concise (<200 lines)
- Verify Claude is asking before loading notes
- Review `.claude/rules/note-taking-protocol.md` has progressive disclosure rules

## FAQ

**Q: Does this work with existing notes?**
A: Yes! Run `/setup-notes` first, then use `/migrate-notes` to automatically analyze, categorize, and move your existing notes into the proper structure.

**Q: Can I use this without Claude Code?**
A: The note structure works standalone. You'll just create notes manually instead of using `/create-note`.

**Q: What if I want different note types?**
A: Create custom templates in `.claude/templates/`. Any `{name}-template.md` file becomes a usable template type.

**Q: How do I share with my team?**
A: Commit `.claude/rules/`, `notes/`, and `.claude/templates/` (if customized) to git. Team members clone and install the plugin.

**Q: Can I migrate from Obsidian/Notion?**
A: Yes. Export to markdown, then run `/migrate-notes` to automatically categorize and organize them into the proper structure.

## Files Included

```
claude-lab-notebook/
├── plugins/claude-lab-notebook/
│   ├── .claude-plugin/
│   │   └── plugin.json              # Plugin configuration
│   └── skills/
│       ├── setup-notes/
│       │   └── SKILL.md             # /setup-notes skill
│       ├── create-note/
│       │   ├── SKILL.md             # /create-note skill
│       │   ├── experiment-template.md
│       │   ├── decision-template.md
│       │   ├── troubleshooting-template.md
│       │   └── meeting-template.md
│       ├── update-index/
│       │   └── SKILL.md             # /update-index skill
│       └── migrate-notes/
│           └── SKILL.md             # /migrate-notes skill
├── QUICK-START.md                   # Quick start guide
├── TEMPLATES.md                     # Template customization guide
├── MIGRATION-GUIDE.md               # Detailed migration instructions
└── README.md                        # This file
```

## Contributing

Improvements welcome! Consider:
- Additional note types
- Better template defaults
- Migration scripts for other tools
- Integration examples

## License

MIT License - see [LICENSE](LICENSE) for details.

---

**Remember:** The best documentation system is the one you actually use.

Start simple. Build the habit. Refine as you go.
