---
description: Create a new experiment, decision, or troubleshooting note using templates
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
   b. `$PLUGIN_ROOT/templates/{type}-template.md` (built-in)

3. **Use first found:**
   - If found in .claude/templates/, use that (user override)
   - Else if found in plugin templates/, use that (built-in)
   - Else error: "Template not found"

4. **Special handling:**
   - User can create ANY template name
   - Built-in templates always available as fallback
   - User templates with same name override built-in

## Task

Guide the user through creating a note using the appropriate template.

## Steps

1. **Determine note type**
   Ask: "What type of note would you like to create?"
   - experiment: For documenting what you tried
   - decision: For recording why you made a choice
   - troubleshooting: For capturing how you fixed an error
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
   
   **For research:**
   - Ask for topic
   - Suggest filename: `notes/research/{topic}.md`

3. **Load appropriate template**

   **Template resolution logic:**
```bash
   # First check user's custom templates
   USER_TEMPLATE=".claude/templates/${type}-template.md"
   PLUGIN_TEMPLATE="$CLAUDE_PLUGIN_ROOT/templates/${type}-template.md"
   
   if [ -f "$USER_TEMPLATE" ]; then
       TEMPLATE_PATH="$USER_TEMPLATE"
       TEMPLATE_SOURCE="custom"
   elif [ -f "$PLUGIN_TEMPLATE" ]; then
       TEMPLATE_PATH="$PLUGIN_TEMPLATE"
       TEMPLATE_SOURCE="built-in"
   else
       echo "Error: Template '${type}-template.md' not found"
       echo "Available templates:"
       # List both user and plugin templates
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

5. **Ask about updates**
   - "Should I update notes/INDEX.md to reference this note?"
   - "Should I update notes/quick-reference.md?" (if this changes current best practice)

6. **Update files if requested**
   
   **For INDEX.md:**
   - Add to appropriate section (Experiments/Decisions/Troubleshooting)
   - Add to "Recent Activity" with today's date
   - Add one-line summary
   
   **For quick-reference.md:**
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

Next steps:
- [ ] Fill in remaining details
- [ ] Link related notes in References section
- [ ] {Updated INDEX.md / Skipped INDEX update}

💡 This note is now part of your knowledge base. I'll reference it in future sessions when relevant."

## Error Handling

- If templates directory doesn't exist: "Run /setup-notes first to initialize the system"
- If file already exists: Ask if they want to overwrite or create with different name
- If INDEX.md doesn't exist: Warn but create note anyway
