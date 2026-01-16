# Custom Templates Guide

## Quick Start

### Using Built-in Templates

No setup needed! The plugin provides three built-in templates:

- **experiment** - Document what you tried and learned
- **decision** - Record why you chose a particular approach
- **troubleshooting** - Capture error solutions for future reference

Just run:
```
/create-note
```

Choose your template type and Claude creates a properly formatted note.

### Creating Custom Templates

To customize templates or create new types:

1. Create the custom templates directory:
   ```bash
   mkdir -p .claude/templates
   ```

2. Add your template file:
   ```bash
   vim .claude/templates/my-type-template.md
   ```

3. Use it:
   ```
   /create-note
   > Type: my-type
   ```

## How Template Resolution Works

When you run `/create-note`, Claude looks for templates in this order:

1. **User custom**: `.claude/templates/{type}-template.md`
2. **Built-in**: Plugin's `templates/{type}-template.md`

The first match wins. This means:
- Custom templates override built-in templates with the same name
- You can create entirely new template types
- Built-in templates always work as fallback

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

## Overriding Built-in Templates

### Method 1: Copy and Modify

```bash
# Create custom templates directory
mkdir -p .claude/templates

# Copy a built-in template (adjust path to your plugin installation)
cp ~/.claude/plugins/claude-lab-notebook/templates/experiment-template.md \
   .claude/templates/

# Customize it
vim .claude/templates/experiment-template.md
```

### Method 2: Create From Scratch

```bash
mkdir -p .claude/templates

cat > .claude/templates/experiment-template.md << 'EOF'
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
- Run `/create-note` and check available types

**Built-in template not working:**
- Plugin may need reinstalling
- Check plugin status with `/plugin list`

**Custom template ignored:**
- Filename must exactly match: `{type}-template.md`
- Directory must be `.claude/templates/` in project root

## FAQ

**Q: Do I need to create .claude/templates/?**
A: No! Only create it if you want custom templates. Built-in templates work without any setup.

**Q: Can I have both custom and built-in templates?**
A: Yes! You can override some templates while using built-in for others.

**Q: Will my custom templates update when the plugin updates?**
A: No - your customizations persist. That's the point of having them separate.

**Q: Can I create templates for new note types?**
A: Yes! Any `{name}-template.md` file becomes a usable template type.

**Q: What if I want to see what built-in templates look like?**
A: Check the plugin's `templates/` directory or ask Claude to show you a template.
