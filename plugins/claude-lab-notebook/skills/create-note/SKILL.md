---
description: Create a new experiment, decision, troubleshooting, or meeting note using templates
argument-hint: [type]
allowed-tools: [Read, Write, Glob, Grep, Bash]
---

You are helping the user create a properly formatted note.

## Template Resolution

All templates are stored in `.claude/templates/` in the user's project.

1. **Determine template name:**
   - User says "experiment" or type == 'experiment' → look for "experiment-template.md"
   - User says "decision" or type == 'decision' → look for "decision-template.md"
   - User says "custom-type" → look for "custom-type-template.md"

2. **Load template:**
   - Read from `.claude/templates/{type}-template.md`
   - If not found, show error: "Template '{type}-template.md' not found in .claude/templates/"
   - List available templates by reading all files matching `.claude/templates/*-template.md`

3. **Built-in templates:**
   - `/setup-notes` copies built-in templates to `.claude/templates/` during initialization
   - Users can customize these templates directly
   - Users can create new templates with `/create-note-type`

## Task

Guide the user through creating a note using the appropriate template.

## Arguments

If the user provided a type argument (e.g., `/create-note experiment`), use that type directly instead of asking.

Valid types: `experiment`, `decision`, `troubleshooting`, `meeting`, `research`, or any custom template name.

## Steps

1. **Determine note type**
   - If type was provided as argument: Use it directly
   - Otherwise ask: "What type of note would you like to create?"
   - experiment: For documenting what you tried
   - decision: For recording why you made a choice
   - troubleshooting: For capturing how you fixed an error
   - meeting: For recording meeting notes and action items
   - research: For external references

2. **Get note details**
   
   **For experiment:**
   - Ask for brief description
   - Suggest filename: `notes/experiments/YYYY-MM-DD-{description}.md`
   
   **For decision:**
   - Ask for topic name
   - Suggest filename: `notes/decisions/YYYY-MM-{topic}.md`
   
   **For troubleshooting:**
   - Ask for error/issue name
   - Suggest filename: `notes/troubleshooting/{issue}-errors.md`
   
   **For meeting:**
   - Ask for meeting topic/name
   - Suggest filename: `notes/meetings/YYYY-MM-DD-{topic}.md`

   **For research:**
   - Ask for topic
   - Suggest filename: `notes/research/{topic}.md`

3. **Load appropriate template**

   **Read the template:**
   - Use Read tool to load `.claude/templates/{type}-template.md`
   - If file doesn't exist, list available templates with Glob: `.claude/templates/*-template.md`
   - Show error: "Template not found. Run `/setup-notes` to initialize templates or `/create-note-type {type}` to create a custom template."

   **Pre-fill template:**
   - Fill in today's date automatically (YYYY-MM-DD format)
   - Pre-fill what you can from conversation context
   - Keep placeholder text for sections you don't have info for

4. **Create the note**
   - Use `create_file` to write the note
   - Place in correct directory
   - Use proper filename format

5. **Gather INDEX metadata**

   Every note MUST be added to INDEX.md. Ask the user:

   a. "What tags describe this? (comma-separated, e.g., 'optimizer, adamw, training')"
   b. "One-line key finding or summary?"

   These make the INDEX searchable without opening individual notes.

6. **Update INDEX.md (always)**

   Add entry to the appropriate section with this format:

   ```markdown
   **YYYY-MM-DD: [Note Title]**
   - Tags: tag1, tag2, tag3
   - Finding: One-line summary of key insight
   - File: [relative/path/to/note.md](relative/path/to/note.md)
   ```

   **Section mapping:**
   - experiment → "## All Experiments"
   - decision → "## All Decisions"
   - troubleshooting → "## All Troubleshooting Guides"
   - meeting → "## All Meetings"
   - research → "## All Research"

   Also add to "## Recent Activity" with today's date and title.

7. **Optionally update quick-reference.md**

   Ask: "Did this change current best practices? Should I update quick-reference.md?"

   If yes:
   - Add to "What Works" if successful approach
   - Add to "Don't Do These" if failed approach
   - Update "Last Updated" section

## Template References

### Experiment Template Structure
```markdown
# YYYY-MM-DD: Title

## Goal
## What I Tried
## Key Findings
## Failed Approaches (table)
## Configuration Used
## Next Steps
## References
```

### Decision Template Structure
```markdown
# Decision: Title

Date: YYYY-MM-DD

## Context
## Options Considered
## Decision
## Rationale
## Trade-offs
## When to Revisit
## References
```

### Troubleshooting Template Structure
```markdown
# Troubleshooting: Error Name

Last updated: YYYY-MM-DD

## Symptom
## Cause
## Solution
## Prevention
## Related Issues
## Example Occurrences
```

### Meeting Template Structure
```markdown
# Meeting: Title

Date: YYYY-MM-DD
Attendees: [Names]

## Agenda
## Discussion
## Decisions Made (table)
## Action Items
## Key Takeaways
## Follow-up
## References
```

## Smart Pre-filling

Based on the conversation history:
- Extract what the user was trying to do (→ Goal)
- Extract what they tried (→ What I Tried)
- Extract any errors mentioned (→ Failed Approaches)
- Extract successful config (→ Configuration Used)
- Note any related past experiments

## Filename Formatting

Always use:
- Lowercase
- Hyphens instead of spaces
- YYYY-MM-DD format for dates
- Brief but descriptive

Good: `2025-01-13-learning-rate-sweep.md`
Bad: `Learning Rate Sweep.md`

## After Creation

Confirm to user:

"✅ Created {filename}

📝 Note type: {type}
📍 Location: notes/{subdirectory}/{filename}
🏷️ Tags: {tags}
📋 Finding: {one-line finding}

✅ Added to INDEX.md under '{section}'

Next steps:
- [ ] Fill in remaining details in the note
- [ ] Link related notes in References section

💡 This note is now searchable via INDEX.md. I'll reference it in future sessions when relevant."

## Error Handling

- If `.claude/templates/` directory doesn't exist: "Run `/setup-notes` first to initialize the system"
- If template file doesn't exist: "Template not found. Available templates: [list from .claude/templates/]. Use `/create-note-type {type}` to create a custom template."
- If note file already exists: Ask if they want to overwrite or create with different name
- If INDEX.md doesn't exist: Warn but create note anyway
