# Custom Templates Guide

## Quick Start

### Using Built-in Templates

After running `/setup-notes`, you'll have four built-in templates ready to use:

- **experiment** - Document what you tried and learned
- **decision** - Record why you chose a particular approach
- **troubleshooting** - Capture error solutions for future reference
- **meeting** - Record meeting discussions and action items

Just run:
```
/create-note
```

Choose your template type and Claude creates a properly formatted note.

### Creating Custom Templates

To customize existing templates or create new types:

1. Templates are already in `.claude/templates/` after running `/setup-notes`

2. Edit an existing template:
   ```bash
   vim .claude/templates/experiment-template.md
   ```

3. Or create a new template:
   ```bash
   vim .claude/templates/my-type-template.md
   ```

4. Use it:
   ```
   /create-note
   > Type: my-type
   ```

## How Template Resolution Works

When you run `/create-note`, Claude looks for templates in:

- **`.claude/templates/{type}-template.md`**

That's it! All templates (both built-in and custom) live in `.claude/templates/`.

During `/setup-notes`, the built-in templates are automatically copied to your project:
- You can customize them directly
- Changes won't be overwritten by plugin updates
- New templates you create work the same way

## Template File Structure

Templates are markdown files with placeholder sections. Claude fills in the date automatically and pre-fills from conversation context.

### Naming Convention

Templates must follow the pattern: `{type}-template.md`

| Type Name | Template Filename |
|-----------|-------------------|
| experiment | experiment-template.md |
| decision | decision-template.md |
| ablation | ablation-template.md |
| sprint | sprint-template.md |

## Customizing Built-in Templates

Built-in templates are already in `.claude/templates/` after `/setup-notes`. Just edit them directly:

### Method 1: Edit Existing Templates

```bash
# Templates are already copied to your project
vim .claude/templates/experiment-template.md
```

### Method 2: Create New Templates

Use `/create-note-type` for guided creation:

```
/create-note-type my-type
```

Or create manually:

```bash
cat > .claude/templates/my-type-template.md << 'EOF'
# YYYY-MM-DD: [Title]

## Objective
What I was trying to achieve

## Method
How I approached it

## Results
What happened

## Conclusions
What I learned
EOF
```

## Example Custom Templates

### Ablation Study Template

```markdown
# .claude/templates/ablation-template.md

# YYYY-MM-DD: Ablation Study - [Component Name]

## Hypothesis
What you expect to learn by removing this component

## Baseline Performance
| Metric | Value |
|--------|-------|
| Accuracy | |
| Loss | |

## Ablated Component
What you removed/disabled

## Results
| Metric | Baseline | Ablated | Change |
|--------|----------|---------|--------|
| Accuracy | | | |
| Loss | | | |

## Analysis
What this tells us about the component's importance

## Configuration
See configs/ablation-YYYY-MM-DD.yaml
```

### Sprint Experiment Template

```markdown
# .claude/templates/sprint-template.md

# YYYY-MM-DD: Sprint Experiment - [Feature Name]

## Sprint: [Sprint Number]
## Ticket: [TICKET-123]

## Goal
Link to product requirements

## Experiment
What you tested

## Results
Metrics vs. targets

## Production Decision
- [ ] Ship it
- [ ] Iterate
- [ ] Abandon

## Stakeholder Sign-off
- PM: [Name]
- Engineering: [Name]
```

### Paper Replication Template

```markdown
# .claude/templates/paper-template.md

# YYYY-MM-DD: Paper Replication - [Paper Title]

## Paper Reference
- **Title**:
- **Authors**:
- **Link**:
- **Code**: (if available)

## Claimed Results
What the paper reports

## Our Reproduction
What we achieved

## Differences
Where results differ and why

## Lessons Learned
Key takeaways from the replication attempt
```

## Template Variables

Claude automatically handles these placeholders:

| Placeholder | Filled With |
|-------------|-------------|
| `YYYY-MM-DD` | Today's date |
| `[Title]`, `[Name]`, etc. | User input or conversation context |

## Sharing Templates

### With Your Team

Commit `.claude/templates/` to version control:

```bash
git add .claude/templates/
git commit -m "Add team experiment templates"
git push
```

Team members pull and get your custom templates automatically.

### Across Projects

Copy templates to new projects:

```bash
# From project A to project B
cp -r project-a/.claude/templates project-b/.claude/
```

Or create a shared templates repository:

```bash
git clone git@github.com:your-org/experiment-templates.git
ln -s experiment-templates .claude/templates
```

## Best Practices

1. **Start with built-in** - Copy and modify rather than starting from scratch
2. **Be specific** - Add sections your team actually uses
3. **Remove clutter** - Delete sections you never fill in
4. **Consistent naming** - Always end with `-template.md`
5. **Include examples** - Show expected format in placeholder text
6. **Version control** - Commit templates to git
7. **Share discoveries** - If you make a useful template, share it with your team

## Troubleshooting

**Template not found:**
- Check filename ends with `-template.md`
- Verify it's in `.claude/templates/` (not `templates/`)
- Run `/setup-notes` if `.claude/templates/` doesn't exist
- List available templates: `ls .claude/templates/`

**Templates directory missing:**
- Run `/setup-notes` to initialize the system and copy built-in templates

**Custom template ignored:**
- Filename must exactly match: `{type}-template.md`
- Directory must be `.claude/templates/` in project root
- Check file permissions: `ls -la .claude/templates/`

## FAQ

**Q: Do I need to create .claude/templates/?**
A: No! `/setup-notes` creates it and copies built-in templates automatically.

**Q: Can I customize the built-in templates?**
A: Yes! After `/setup-notes`, edit the files in `.claude/templates/` directly.

**Q: Will my customizations be overwritten when the plugin updates?**
A: No - templates in `.claude/templates/` are yours to keep. Plugin updates won't touch them.

**Q: Can I create templates for new note types?**
A: Yes! Use `/create-note-type` or manually add `{name}-template.md` files to `.claude/templates/`.

**Q: What if I want to reset to default templates?**
A: Delete the templates you want to reset from `.claude/templates/`, then re-run `/setup-notes` to copy fresh versions.
