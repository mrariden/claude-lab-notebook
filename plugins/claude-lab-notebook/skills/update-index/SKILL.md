---
description: Update INDEX.md with recent findings and organize notes
argument-hint: [action]
allowed-tools: [Read, Write, Glob, Grep, Bash]
---

You are helping the user maintain their knowledge base index.

## Task

Update notes/INDEX.md to reflect recent work and maintain organization.

## Arguments

If the user provided an action argument (e.g., `/update-index consolidate`), perform that action directly.

Valid actions:
- `experiment` - Add a recent experiment
- `decision` - Add a recent decision
- `troubleshooting` - Add a troubleshooting guide
- `consolidate` - Review recent notes and extract patterns
- `cleanup` - Remove old entries, fix broken links

## Steps

1. **Read current INDEX.md**
   - Check what's already documented
   - Note the last update date

2. **Ask what to add**
   Options:
   - "I just finished an experiment" → Add to Recent Activity + All Experiments
   - "I made a decision" → Add to Decisions list
   - "I solved an error" → Add to Known Issues or Troubleshooting
   - "Review recent notes" → Scan notes/ directories and suggest updates
   - "Consolidate findings" → Review multiple experiments and extract patterns

3. **For new experiment:**
   - Add one-line summary to "Recent Activity"
   - Add to "All Experiments" chronologically
   - If significant: Add to "What Works" or "Known Issues"
   - **CRITICAL:** Include clickable link to actual note file using `[Note title](relative/path.md)` syntax

4. **For new decision:**
   - Add to "All Decisions" list
   - Add one-line summary with clickable link: `[Decision title](decisions/filename.md)`
   - If it affects current practice: Note in "What Works"

5. **For troubleshooting:**
   - Add to "Known Issues" if ongoing problem
   - Add to "All Troubleshooting Guides" list
   - Include symptom + clickable link: `[Error title](troubleshooting/filename.md)`

6. **For consolidation:**
   - Scan experiments/ for recent notes
   - Extract common patterns
   - Group related findings
   - Update "What Works" summary section
   - Update "Known Issues" summary section

7. **Update timestamp**
   - Change "Last updated:" to today's date

8. **Show changes**
   - Display the diff or summarize what was added
   - Ask for confirmation before saving

## INDEX.md Structure

**IMPORTANT:** All file references in INDEX.md MUST be clickable markdown links using `[Display text](relative/path.md)` syntax.

```markdown
# Project Notes Index

Last updated: YYYY-MM-DD

## Quick Links
- [Current Best Config](../configs/)
- [Quick Reference](quick-reference.md)

## What Works (Summary)
- **Topic**: One-line summary (see [Experiment title](experiments/YYYY-MM-DD-file.md))

## Known Issues
- **Issue**: One-line description (see [Troubleshooting title](troubleshooting/file.md))

## Recent Activity (Last 7 days)
- YYYY-MM-DD: [What happened](experiments/YYYY-MM-DD-file.md) - Brief summary

## All Experiments
- YYYY-MM-DD: [Experiment title](experiments/YYYY-MM-DD-file.md) - Brief result

## All Decisions
- [Decision title](decisions/file.md) - One-line summary

## All Troubleshooting Guides
- [Error Name](troubleshooting/file.md) - Symptom

## All Meetings
- [Meeting title](meetings/YYYY-MM-DD-topic.md) - Key takeaways
```

## Smart Consolidation

When user asks to "consolidate" or "review":

1. **Scan directories**
   ```bash
   ls -lt notes/experiments/*.md | head -10
   ls -lt notes/decisions/*.md
   ls -lt notes/troubleshooting/*.md
   ```

2. **Read recent files**
   - Last 7 days of experiments
   - Any new decisions
   - New troubleshooting guides

3. **Extract patterns**
   - What worked across multiple experiments?
   - What failed multiple times?
   - Are there contradictions to resolve?

4. **Propose updates**
   Show user proposed additions with clickable links:
   "I found these patterns from your recent work:

   ✅ What Works:
   - Learning rate 0.0003 consistently good ([exp1](experiments/2025-01-10-lr.md), [exp2](experiments/2025-01-12-lr.md), [exp3](experiments/2025-01-13-lr.md))
   - 4-layer architecture optimal for this task ([architecture comparison](experiments/2025-01-11-arch.md))

   ❌ Known Issues:
   - Batch size >64 causes OOM on RTX 3090 ([troubleshooting guide](troubleshooting/oom-errors.md))

   Should I add these to INDEX.md?"

## One-Line Summary Guidelines

Good summaries are:
- ✅ Specific: "LR=0.0003 optimal for 4-layer model"
- ✅ Actionable: "Use batch_size ≤32 to avoid OOM"
- ✅ Concise: One line with key metric or finding

Bad summaries:
- ❌ Vague: "Tested learning rates"
- ❌ Too long: Full paragraph
- ❌ No context: "It worked"

## Recent Activity Format

Keep last 7-14 days visible with clickable links:

```markdown
## Recent Activity (Last 7 days)
- 2025-01-13: [Learning rate sweep](experiments/2025-01-13-lr-sweep.md) - 0.0003 optimal
- 2025-01-12: [Architecture comparison](experiments/2025-01-12-arch.md) - 4 layers best
- 2025-01-11: [Optimizer test](experiments/2025-01-11-optimizer.md) - AdamW > SGD
```

**Format:** `YYYY-MM-DD: [Note title](relative/path.md) - Brief result`

Auto-archive older entries to keep this section focused.

## After Update

Tell user:

"✅ Updated notes/INDEX.md

📝 Changes:
- Added to Recent Activity: {summary}
- Updated {section}: {what changed}
- Last updated: {today's date}

💡 Your knowledge map is current. I'll reference these findings in future sessions."

## Maintenance Mode

If user says "clean up INDEX" or "reorganize":

1. Remove entries older than 30 days from "Recent Activity"
2. Check for dead links (files that don't exist)
3. Alphabetize sections if requested
4. Consolidate duplicate entries
5. Verify all links work

## Error Handling

- If INDEX.md doesn't exist: "Run /setup-notes first"
- If can't parse structure: Offer to rebuild from scratch
- If referenced files missing: Note broken links and ask to fix
