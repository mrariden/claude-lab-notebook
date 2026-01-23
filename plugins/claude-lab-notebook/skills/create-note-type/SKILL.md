---
description: Create a new custom note type with its own template
argument-hint: [type-name]
allowed-tools: [Read, Write, Glob, Grep, Bash]
---

You are helping the user create a new custom note type for their project.

## Task

Guide the user through creating a new note type template that can be used with `/create-note`.

## How Custom Templates Work

Custom templates are stored in `.claude/templates/` in the user's project. When `/create-note` runs, it looks for templates in this order:

1. `.claude/templates/{type}-template.md` (user custom - checked first)
2. `$SKILL_DIR/{type}-template.md` (built-in - fallback)

By creating a template in `.claude/templates/`, users can:
- Create entirely new note types
- Override built-in templates with custom versions

## Arguments

If the user provided a type name (e.g., `/create-note-type standup`), use that name directly.

## Steps

1. **Get type name**
   - If provided as argument: Use it directly
   - Otherwise ask: "What would you like to call this note type?"
   - Examples: `standup`, `retrospective`, `bug-report`, `feature-spec`, `interview`

   **Naming rules:**
   - Lowercase only
   - Use hyphens for multi-word names (e.g., `bug-report`)
   - No spaces or special characters
   - Keep it short and descriptive

2. **Ensure templates directory exists**
   ```bash
   mkdir -p .claude/templates
   ```

3. **Check for existing template**
   - Look for `.claude/templates/{type}-template.md`
   - If exists, ask: "A template for '{type}' already exists. Do you want to overwrite it?"

4. **Gather template sections**

   Ask: "What sections should this note type have? I'll suggest some based on common patterns."

   **Suggest sections based on type name:**

   For `standup` or `daily`:
   - Yesterday's Progress
   - Today's Plan
   - Blockers

   For `retrospective` or `retro`:
   - What Went Well
   - What Could Be Improved
   - Action Items

   For `bug-report` or `bug`:
   - Summary
   - Steps to Reproduce
   - Expected Behavior
   - Actual Behavior
   - Environment
   - Screenshots/Logs

   For `feature-spec` or `spec`:
   - Overview
   - Requirements
   - User Stories
   - Technical Approach
   - Open Questions
   - Success Criteria

   For `interview`:
   - Candidate
   - Position
   - Questions Asked
   - Responses/Notes
   - Assessment
   - Recommendation

   For `review` or `code-review`:
   - PR/Code Reference
   - Summary of Changes
   - Feedback
   - Questions
   - Decision

   For other types:
   - Ask user to list their desired sections
   - Suggest common sections: Summary, Details, Action Items, References

5. **Determine filename pattern**

   Ask: "How should files of this type be named?"

   Suggest based on type:
   - Date-based: `YYYY-MM-DD-{description}.md` (for recurring notes like standups)
   - Topic-based: `{topic}.md` (for one-off reference notes)
   - Combined: `YYYY-MM-DD-{topic}.md` (for dated topical notes)

   Also ask: "What directory should these notes go in?"
   - Suggest: `notes/{type}s/` (e.g., `notes/standups/`, `notes/bug-reports/`)

6. **Create the template**

   Build template with:
   - Title format: `# {Type}: {Title}` or `# YYYY-MM-DD: {Title}`
   - Date field if applicable
   - Each section as `## Section Name` with placeholder content
   - References section at the end

   Write to: `.claude/templates/{type}-template.md`

7. **Create the notes directory**
   ```bash
   mkdir -p notes/{type}s
   ```

8. **Update INDEX.md (optional)**

   Ask: "Should I add a section for '{type}' notes to INDEX.md?"

   If yes, add:
   ```markdown
   ## All {Type}s
   <!-- Example:
   **YYYY-MM-DD: Title**
   - Tags: tag1, tag2
   - Finding: One-line summary
   - File: [{type}s/filename.md]({type}s/filename.md)
   -->
   ```

9. **Confirm completion**

## Template Format

Use this structure for the generated template:

```markdown
# {Type}: Descriptive Title

Date: YYYY-MM-DD

## Section 1
Placeholder text explaining what goes here

## Section 2
Placeholder text explaining what goes here

## Section 3
Placeholder text explaining what goes here

## References
- Related notes: path/to/note.md
- External links: [Link text](URL)
```

## Example Templates

### Standup Template
```markdown
# YYYY-MM-DD: Daily Standup

## Yesterday
- What I completed

## Today
- What I plan to work on

## Blockers
- Any impediments (or "None")

## Notes
- Additional context
```

### Bug Report Template
```markdown
# Bug: Descriptive Title

Reported: YYYY-MM-DD
Status: Open | In Progress | Resolved

## Summary
Brief description of the bug

## Steps to Reproduce
1. Step one
2. Step two
3. Step three

## Expected Behavior
What should happen

## Actual Behavior
What actually happens

## Environment
- OS:
- Browser/Runtime:
- Version:

## Screenshots/Logs
```
[Paste screenshots or error logs]
```

## Investigation Notes
- Findings during debugging

## Solution
How it was fixed (fill in when resolved)

## References
- Related issues
- Related code
```

## After Creation

Tell the user:

"✅ Created new note type: {type}

📄 Template: `.claude/templates/{type}-template.md`
📁 Directory: `notes/{type}s/`
📋 INDEX.md: {Updated | Not updated}

🚀 To use it:
```
/create-note {type}
```

Or when prompted for note type, enter: `{type}`

💡 To customize further:
Edit `.claude/templates/{type}-template.md` directly.

This template will be used automatically whenever you create a '{type}' note!"

## Error Handling

- If `.claude/templates/` doesn't exist: Create it
- If template already exists: Ask before overwriting
- If INDEX.md doesn't exist: Skip INDEX update, warn user
- If notes/ doesn't exist: Run `/setup-notes` first or create minimal structure
