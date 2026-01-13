# Migration Guide: Transitioning to Note-Taking Protocol

## Overview

This guide helps you migrate from unstructured notes to the organized note-taking protocol. The migration preserves all existing knowledge while organizing it for efficient retrieval.

## Pre-Migration Checklist

- [ ] Backup existing notes (git commit or copy to safe location)
- [ ] Review CLAUDE.md for the new protocol structure
- [ ] Identify where your current notes are located
- [ ] Estimate number of existing note files (for planning)

## Migration Steps

### Step 1: Create Directory Structure

Create the new directory structure in your project root:

```bash
mkdir -p notes/{experiments,decisions,troubleshooting,research}
mkdir -p configs
mkdir -p templates
```

### Step 2: Create Templates

Copy these templates into your `templates/` directory:

**templates/experiment-template.md**
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
| Thing | Error/symptom | Takeaway |

## Configuration Used
```yaml
# Copy-pasteable config
```

## Next Steps
- [ ] Follow-up task

## References
- Related: experiments/YYYY-MM-DD-file.md
```

**templates/decision-template.md**
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

**templates/troubleshooting-template.md**
```markdown
# Troubleshooting: Error Name

## Symptom
Exact error message

## Cause
Why this happens

## Solution
1. Step one
2. Step two

## Prevention
- How to avoid

## Related Issues
- Similar problems
```

### Step 3: Analyze Existing Notes

For each existing note file, determine:

1. **What type is it?**
   - Time-stamped experiments/attempts → `experiments/`
   - Architectural/design choices → `decisions/`
   - Error solutions → `troubleshooting/`
   - External references → `research/`
   - Current working state → Will become `quick-reference.md`

2. **Can you add a date?**
   - Check git history: `git log --follow filename.md`
   - Use file modification time if needed
   - Estimate based on context if necessary

3. **What are the key findings?**
   - These will go in INDEX.md
   - Extract "what works" and "what fails"

### Step 4: Categorize and Rename

**Example Migrations:**

Existing: `optimizer_tests.md`
→ Category: Experiment
→ New location: `notes/experiments/2025-01-12-optimizer-comparison.md`
→ Extract decision to: `notes/decisions/2025-01-12-optimizer-choice.md`

Existing: `cuda_oom_fix.md`
→ Category: Troubleshooting
→ New location: `notes/troubleshooting/cuda-memory-errors.md`

Existing: `current_config.md`
→ Category: Current state
→ Becomes: `notes/quick-reference.md`
→ Archive specific configs to: `configs/`

Existing: `attention_paper_notes.md`
→ Category: Research
→ New location: `notes/research/attention-mechanisms.md`

### Step 5: Migration Script Template

You can ask Claude Code to help with migration using this approach:

```
For each existing note:
1. Read the note
2. Determine category (experiment/decision/troubleshooting/research)
3. Add date to filename (YYYY-MM-DD format)
4. Move to appropriate notes/ subdirectory
5. Extract key findings for INDEX.md
6. Update internal links if they reference moved files
```

### Step 6: Create INDEX.md

Scan all migrated notes and create the master index:

```markdown
# Project Notes Index

Last updated: YYYY-MM-DD

## Quick Links
- [Current Best Config](../configs/best.yaml)
- [Common Issues](troubleshooting/)

## What Works (Summary)
<!-- Extract from experiments -->
- **Learning Rate**: 0.0003 optimal (see experiments/2025-01-10-lr-sweep.md)
- **Architecture**: 4 layers works (see experiments/2025-01-11-architecture.md)
- **Optimizer**: AdamW > SGD (see experiments/2025-01-12-optimizer-comparison.md)

## Known Issues
<!-- Extract from troubleshooting -->
- **CUDA OOM**: batch_size > 64 fails (see troubleshooting/cuda-memory-errors.md)
- **NaN Loss**: LR > 0.001 unstable (see experiments/2025-01-10-lr-sweep.md)

## Recent Activity (Last 30 days)
<!-- List recent experiments chronologically -->
- 2025-01-15: Tested new architecture
- 2025-01-12: Compared optimizers
- 2025-01-11: Architecture sweep
- 2025-01-10: Learning rate sweep

## Decisions Made
- 2025-01-12: Chose AdamW optimizer (see decisions/2025-01-12-optimizer-choice.md)
- 2025-01-11: Settled on 4-layer architecture (see decisions/2025-01-11-architecture.md)

## All Experiments
<!-- Chronological list with one-line summaries -->
- 2025-01-15: New architecture test - Failed, reverted
- 2025-01-12: Optimizer comparison - AdamW won
- 2025-01-11: Architecture sweep - 4 layers optimal
- 2025-01-10: Learning rate sweep - 0.0003 best

## All Decisions
<!-- List all decision files -->
- [Optimizer Choice](decisions/2025-01-12-optimizer-choice.md)
- [Architecture Design](decisions/2025-01-11-architecture.md)

## All Troubleshooting Guides
<!-- List all error solutions -->
- [CUDA Memory Errors](troubleshooting/cuda-memory-errors.md)
- [Training Instability](troubleshooting/nan-loss-errors.md)
```

### Step 7: Create quick-reference.md

Extract current working configuration and gotchas:

```markdown
# Quick Reference - Current Project State

Last updated: YYYY-MM-DD

## What's Working Right Now

```yaml
# Current best configuration
model:
  layers: 4
  hidden_dim: 256
  
training:
  optimizer: adamw
  lr: 0.0003
  batch_size: 32
  warmup_steps: 100
```

## Don't Do These (Failed Approaches)
- ❌ LR > 0.001 → NaN loss at step ~50
- ❌ Batch size > 64 → CUDA OOM error
- ❌ SGD optimizer → 15% worse than AdamW
- ❌ 2 layers → Underfits, need minimum 4 layers
- ❌ No warmup → Unstable first 100 steps

## Current Best Practices
- ✓ Use AdamW optimizer with LR=0.0003
- ✓ Include 100 warmup steps
- ✓ Keep batch_size ≤ 32 for this GPU
- ✓ Use 4 layers (sweet spot)
- ✓ Monitor loss - should decrease steadily

## Open Questions
- [ ] Does dropout improve generalization?
- [ ] Can we speed up data loading?
- [ ] Would learning rate scheduling help?

## Known Bugs/Issues
- CUDA OOM if batch_size > 64 (see troubleshooting/cuda-memory-errors.md)
- Loss becomes NaN if LR too high (see troubleshooting/nan-loss-errors.md)

## Last Updated
- 2025-01-15: Added architecture notes
- 2025-01-12: Updated optimizer recommendation
```

### Step 8: Update Internal Links

Search for references to old filenames and update them:

```bash
# Find all markdown files that might have links
grep -r "old-filename.md" notes/

# Update links to new structure
# old-filename.md → experiments/2025-01-10-old-filename.md
```

### Step 9: Verify Migration

Test the new structure by asking Claude Code to:

1. Read INDEX.md and quick-reference.md
2. Answer a question about a past experiment
3. Find information about a known error
4. Locate a specific decision rationale

Example test queries:
- "What learning rate works best?"
- "How do I fix CUDA OOM errors?"
- "Why did we choose this architecture?"

### Step 10: Archive Old Notes (Optional)

Once migration is verified:

```bash
# Create archive directory
mkdir -p notes/archive/pre-migration

# Move original unstructured notes
mv old-notes/* notes/archive/pre-migration/

# Document what was archived
echo "# Pre-Migration Archive

Original unstructured notes before migration to protocol.
Migrated on: $(date)
See main notes/ directory for organized version.
" > notes/archive/pre-migration/README.md
```

## Common Migration Patterns

### Pattern 1: Mixed Content Note

**Original**: `training_experiments.md` contains experiments, decisions, and errors

**Migration**:
1. Split into multiple files
2. Experiments → `experiments/2025-01-XX-training-*.md` (one per experiment)
3. Decision → `decisions/2025-01-XX-training-strategy.md`
4. Errors → `troubleshooting/training-errors.md`
5. Summary → Add to INDEX.md

### Pattern 2: Undated Notes

**Original**: `architecture_ideas.md` with no clear date

**Migration**:
1. Check `git log --follow architecture_ideas.md`
2. Use earliest commit date or file creation date
3. If truly unknown, use today's date with note: "Note: Original date unknown, using migration date"

### Pattern 3: Ongoing Work

**Original**: `current_work.md` being actively updated

**Migration**:
1. Create dated experiment file for past work
2. Update quick-reference.md with current state
3. Add to INDEX.md under "Recent Activity"
4. Continue new work using experiment template

### Pattern 4: Configuration History

**Original**: Multiple config files like `config_v1.yaml`, `config_v2.yaml`

**Migration**:
1. Keep all in `configs/` directory
2. Rename with dates: `config-2025-01-10.yaml`, `config-2025-01-15.yaml`
3. Create `configs/best.yaml` symlink or copy
4. Document config evolution in INDEX.md

## Automated Migration Helper

Ask Claude Code to run this migration workflow:

```
Claude, please help migrate my notes to the new protocol:

1. Scan my existing notes in [location]
2. For each note:
   - Determine type (experiment/decision/troubleshooting/research)
   - Extract or estimate date
   - Propose new filename and location
3. Show me the migration plan before executing
4. After I approve, perform the migration
5. Create INDEX.md from all findings
6. Create quick-reference.md from current state
7. Verify all internal links work
```

## Post-Migration Workflow

After migration, use this workflow:

**Before each session:**
1. Claude reads INDEX.md and quick-reference.md
2. Claude asks what you're working on
3. Claude suggests relevant notes to load

**During session:**
- Make notes of findings
- Document failures immediately

**After session:**
- Create appropriate note file
- Update INDEX.md if significant
- Update quick-reference.md if best practice changed

## Troubleshooting Migration Issues

**Issue**: Can't determine note type
**Solution**: Default to `experiments/`, can always move later

**Issue**: Multiple dates for same content
**Solution**: Use earliest date, note evolution in file

**Issue**: Broken links after migration
**Solution**: Use search/replace: `grep -r "old-path" notes/` then update

**Issue**: INDEX.md too long
**Solution**: Keep summaries brief (one line per item), link to full notes

**Issue**: Lost context during migration
**Solution**: Keep archive/ directory with originals until confident

## Migration Checklist

- [ ] Created directory structure
- [ ] Created templates
- [ ] Categorized all existing notes
- [ ] Added dates to filenames
- [ ] Moved files to correct directories
- [ ] Created INDEX.md
- [ ] Created quick-reference.md
- [ ] Updated internal links
- [ ] Tested retrieval with sample queries
- [ ] Archived original notes
- [ ] Committed changes to git
- [ ] Tested with Claude Code session

## Success Criteria

Migration is successful when:
1. Claude can answer "What works?" by reading INDEX.md
2. Claude can find specific experiments by date/topic
3. Claude can locate error solutions quickly
4. All internal links work
5. quick-reference.md reflects current truth
6. You can find any piece of knowledge in <30 seconds

---

**Remember**: Migration is iterative. Start with most important/recent notes. You can migrate older notes over time as needed.
