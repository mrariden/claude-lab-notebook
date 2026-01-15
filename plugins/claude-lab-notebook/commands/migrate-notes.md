---
description: Migrate existing notes into the organized lab notebook structure
argument-hint: <required-arg> [optional-arg]
allowed-tools: [Read, Write, Glob, Grep, Bash]
---

# Migrate Existing Notes

Help the user migrate their existing notes to the lab notebook structure.

## Your Task

Guide the user through migrating their existing notes into the organized note-taking protocol.

## Step 1: Discover Existing Notes

First, ask the user where their existing notes are located. Common locations:
- `./notes/` or `./docs/`
- `./README.md` with embedded notes
- Scattered markdown files in project root
- A different notes directory

If they're unsure, search for markdown files:
```bash
find . -name "*.md" -type f | grep -v node_modules | grep -v .git
```

## Step 2: Ensure Structure Exists

Check if the lab notebook structure exists. If not, suggest running `/setup-notes` first:
- `notes/INDEX.md`
- `notes/experiments/`
- `notes/decisions/`
- `notes/troubleshooting/`
- `notes/research/`

## Step 3: Analyze Each Note

For each existing note file:

1. **Read the content** to understand what it contains
2. **Determine the type**:
   - Time-stamped experiments/attempts → `experiments/`
   - Architectural/design choices → `decisions/`
   - Error solutions/fixes → `troubleshooting/`
   - External references/papers → `research/`
   - Current working state → `quick-reference.md`
   - Mixed content → Split into multiple files

3. **Extract or estimate a date**:
   - Check git history: `git log --follow --format="%ai" -- "filename" | tail -1`
   - Use file modification time if no git history
   - Use today's date if truly unknown (note this in the file)

4. **Propose new filename**: `YYYY-MM-DD-descriptive-name.md`

## Step 4: Present Migration Plan

Before making changes, show the user a migration plan:

```
## Migration Plan

| Original File | Type | New Location |
|--------------|------|--------------|
| old-notes.md | experiment | notes/experiments/2025-01-10-old-notes.md |
| config-ideas.md | decision | notes/decisions/2025-01-08-config-architecture.md |
| error-fixes.md | troubleshooting | notes/troubleshooting/common-errors.md |

### Actions:
1. Move X files to experiments/
2. Move Y files to decisions/
3. Move Z files to troubleshooting/
4. Split W mixed-content files
5. Update INDEX.md with findings
6. Archive originals to notes/archive/

Proceed with migration? (yes/no)
```

## Step 5: Execute Migration

After user approval:

1. **Create archive directory** for originals:
   ```bash
   mkdir -p notes/archive/pre-migration
   ```

2. **For each file**:
   - Copy original to archive
   - Create new file in correct location with proper template structure
   - Preserve original content within the template
   - Add date header if missing

3. **Extract key findings** from each file for INDEX.md

4. **Update INDEX.md** with:
   - Summary of migrated experiments
   - Key decisions documented
   - Known issues/solutions

5. **Create/update quick-reference.md** with current working state if found

## Step 6: Verify Migration

After migration:
1. List all new files created
2. Show updated INDEX.md
3. Confirm all original files are archived
4. Test that notes can be found by topic

## Migration Templates

When restructuring content, use these formats:

**For experiments:**
```markdown
# YYYY-MM-DD: [Title from original]

## Goal
[Extract from original or ask user]

## What I Tried
[Extract from original]

## Key Findings
[Extract from original]

## Failed Approaches
| What I Tried | Why It Failed | Lesson Learned |
|--------------|---------------|----------------|
[Extract from original]

## Original Content
> [Preserve any content that doesn't fit above]
```

**For decisions:**
```markdown
# Decision: [Title]

Date: YYYY-MM-DD

## Context
[Extract from original]

## Options Considered
[Extract from original]

## Decision
[Extract from original]

## Rationale
[Extract from original]
```

**For troubleshooting:**
```markdown
# Troubleshooting: [Error/Issue Name]

## Symptom
[Extract error messages from original]

## Cause
[Extract from original]

## Solution
[Extract steps from original]

## Prevention
[Extract or suggest based on content]
```

## Important Notes

- Always preserve original content - never delete without archiving
- Ask before proceeding with each major step
- If a note doesn't fit cleanly into one category, ask the user
- Mixed-content files should be split into separate notes
- Update all internal links after moving files
