# Note-Taking Protocol Plugin for Claude Code

A Claude Code plugin that automates setup and management of a progressive disclosure note-taking system for capturing experimental knowledge.

## What This Plugin Does

Adds four slash commands to Claude Code:

1. **`/setup-notes`** - Initialize the note-taking system in your project
2. **`/create-note`** - Create properly formatted experiment/decision/troubleshooting notes
3. **`/update-index`** - Maintain your knowledge base index
4. **`/migrate-notes`** - Migrate existing notes into the organized structure

## Installation

### Option 1: Copy to Project (Recommended for Single Project)

```bash
# In your project root
mkdir -p .claude/commands
cd .claude/commands

# Copy the command files
curl -O https://[your-repo]/setup-notes.md
curl -O https://[your-repo]/create-note.md
curl -O https://[your-repo]/update-index.md
```

### Option 2: Install Globally (For All Projects)

```bash
# In your home directory
mkdir -p ~/.claude/commands
cd ~/.claude/commands

# Copy the command files
curl -O https://[your-repo]/setup-notes.md
curl -O https://[your-repo]/create-note.md
curl -O https://[your-repo]/update-index.md
```

### Option 3: Clone This Repository

```bash
# Clone the plugin
git clone https://[your-repo]/note-taking-plugin.git

# Install to global commands
cp note-taking-plugin/.claude/commands/* ~/.claude/commands/

# Or install to project
cd your-project
mkdir -p .claude/commands
cp path/to/note-taking-plugin/.claude/commands/* .claude/commands/
```

### Option 4: Use the Setup Script

```bash
# Download and run the automated setup script
curl -O https://[your-repo]/setup-notes.sh
chmod +x setup-notes.sh
./setup-notes.sh /path/to/your/project
```

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
   - `CLAUDE.md` - Protocol instructions
   - `notes/` - Directory structure
   - `templates/` - Note templates
   - Starter files (INDEX.md, quick-reference.md)

4. **Start using it**
   - Do your work
   - Run `/create-note` to document findings
   - Run `/update-index` to maintain organization

## Commands

### /setup-notes

Initializes the complete note-taking system in your current project.

**What it creates:**
```
your-project/
├── CLAUDE.md                          # Protocol Claude follows
├── notes/
│   ├── INDEX.md                       # Master knowledge index
│   ├── quick-reference.md             # Current working state
│   ├── experiments/                   # Chronological experiments
│   ├── decisions/                     # Design decisions
│   ├── troubleshooting/              # Error solutions
│   └── research/                      # External references
├── configs/                           # Configuration files
└── templates/                         # Note templates
    ├── experiment-template.md
    ├── decision-template.md
    └── troubleshooting-template.md
```

**Usage:**
```
You: /setup-notes
Claude: [Creates complete structure]
Claude: ✅ Setup complete! Ready to use.
```

**Safety:**
- Backs up existing CLAUDE.md before overwriting
- Asks before overwriting existing directories
- Safe to run multiple times

### /create-note

Creates a new note using the appropriate template.

**Usage:**
```
You: /create-note
Claude: "What type of note? (experiment/decision/troubleshooting/research)"
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
   - Claude reads `CLAUDE.md` (the protocol)
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

Claude: [Reads CLAUDE.md, INDEX.md, quick-reference.md]
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

You can modify templates in `templates/` to match your workflow:

```bash
# Edit experiment template
vim templates/experiment-template.md

# Add custom sections
# Remove sections you don't use
# Change formatting
```

Changes apply to all new notes created with `/create-note`.

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

Edit `CLAUDE.md` to change behavior:

```markdown
# Your CLAUDE.md

## Custom Instructions
- Always check tests/ directory before experiments
- Include performance benchmarks in all experiment notes
- Tag notes with project phase (prototype/production)
```

### Add Custom Commands

Create your own slash commands in `.claude/commands/`:

```bash
# .claude/commands/weekly-review.md
---
name: weekly-review
description: Review and consolidate this week's notes
---

[Your custom instructions]
```

### Extend Templates

Add sections to templates:

```markdown
# templates/experiment-template.md

## [Your Custom Section]
- Custom field 1
- Custom field 2

## Performance Metrics
- Latency:
- Throughput:
- Resource usage:
```

## Troubleshooting

**Command not found:**
- Verify files are in `.claude/commands/` or `~/.claude/commands/`
- Restart Claude Code
- Check file permissions: `chmod +r *.md`

**Setup fails:**
- Check if you're in the right directory
- Verify write permissions
- Look for existing CLAUDE.md conflict

**Notes not being referenced:**
- Verify INDEX.md exists and has content
- Check that CLAUDE.md is in project root
- Ensure proper file naming (lowercase, hyphens, dates)

**Context still too large:**
- Check that INDEX.md is concise (<200 lines)
- Verify Claude is asking before loading notes
- Review CLAUDE.md has progressive disclosure rules

## FAQ

**Q: Does this work with existing notes?**
A: Yes! Run `/setup-notes` first, then use `/migrate-notes` to automatically analyze, categorize, and move your existing notes into the proper structure.

**Q: Can I use this without Claude Code?**
A: The note structure works standalone. You'll just create notes manually instead of using `/create-note`.

**Q: What if I want different note types?**
A: Add custom templates to `templates/` and modify the `/create-note` command to recognize them.

**Q: How do I share with my team?**
A: Commit `CLAUDE.md`, `notes/`, and `templates/` to git. Team members clone and install the plugin commands.

**Q: Can I migrate from Obsidian/Notion?**
A: Yes. Export to markdown, then run `/migrate-notes` to automatically categorize and organize them into the proper structure.

## Files Included

```
claude-lab-notebook/
├── .claude/
│   └── commands/
│       ├── setup-notes.md      # /setup-notes command
│       ├── create-note.md      # /create-note command
│       ├── update-index.md     # /update-index command
│       └── migrate-notes.md    # /migrate-notes command
├── templates/                  # Note templates
├── QUICK-START.md              # Quick start guide
├── MIGRATION-GUIDE.md          # Detailed migration instructions
└── README.md                   # This file
```

## Contributing

Improvements welcome! Consider:
- Additional note types
- Better template defaults
- Migration scripts for other tools
- Integration examples

## License

Free to use and modify for your projects.

---

**Remember:** The best documentation system is the one you actually use.

Start simple. Build the habit. Refine as you go.
