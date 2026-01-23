---
description: Create a new experiment, decision, troubleshooting, or meeting note using templates
argument-hint: [type]
allowed-tools: [Read, Write, Glob, Grep, Bash]
---

You are helping the user create a properly formatted note.

## Template Resolution

Claude uses this logic to find templates:

1. **Determine template name:**
   - User says "experiment" or type == 'experiment'→ look for "experiment-template.md"
   - User says "decision" or type == 'decision'→ look for "decision-template.md"
   - User says "custom-type" → look for "custom-type-template.md"

2. **Search paths (in order):**
   a. `.claude/templates/{type}-template.md` (user custom)
   b. `$SKILL_DIR/{type}-template.md` (built-in, same directory as this skill)

3. **Use first found:**
   - If found in .claude/templates/, use that (user override)
   - Else if found in skill directory, use that (built-in)
   - Else error: "Template not found"

4. **Special handling:**
   - User can create ANY template name
   - Built-in templates always available as fallback
   - User templates with same name override built-in

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

   **Template resolution logic:**
```bash
   # First check user's custom templates
   USER_TEMPLATE=".claude/templates/${type}-template.md"
   SKILL_TEMPLATE="$SKILL_DIR/${type}-template.md"

   if [ -f "$USER_TEMPLATE" ]; then
       TEMPLATE_PATH="$USER_TEMPLATE"
       TEMPLATE_SOURCE="custom"
   elif [ -f "$SKILL_TEMPLATE" ]; then
       TEMPLATE_PATH="$SKILL_TEMPLATE"
       TEMPLATE_SOURCE="built-in"
   else
       echo "Error: Template '${type}-template.md' not found"
       echo "Available templates:"
       # List both user and skill templates
       exit 1
   fi
```
   
   **Read template:**
   - Use `view` tool to read from $TEMPLATE_PATH
   - Fill in today's date automatically
   - Pre-fill what you can from conversation context
   
   **Inform user (optional):**
   If using custom template, mention it:
   "Using custom template from .claude/templates/"

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

- If templates directory doesn't exist: "Run /setup-notes first to initialize the system"
- If file already exists: Ask if they want to overwrite or create with different name
- If INDEX.md doesn't exist: Warn but create note anyway
