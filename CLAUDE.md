# Development Notes for Claude

This file contains instructions for maintaining and updating the claude-lab-notebook plugin.

## Version Bumping

When releasing a new version, update the version number in these locations:

### Required Files

1. **`.claude-plugin/marketplace.json`**
   - Line: `"version": "X.Y.Z",`
   - Location: Inside the `plugins[0]` object

2. **`MARKETPLACE.md`**
   - Line: `**Version:** X.Y.Z`
   - Location: Top of file, under the title in the metadata section

### Verification Command

After updating, verify all versions are consistent:

```bash
grep -r "2\.[0-9]\.[0-9]" --include="*.md" --include="*.json" . | grep -v ".git" | grep -v "2.0.0"
```

This should only show your intended version number.

### Version Number Guidelines

Follow semantic versioning (semver):

- **Major (X.0.0)**: Breaking changes to plugin API or structure
- **Minor (X.Y.0)**: New features, new skills, significant enhancements
- **Patch (X.Y.Z)**: Bug fixes, documentation updates, minor improvements

## Files That DON'T Need Version Updates

- `plugins/claude-lab-notebook/.claude-plugin/plugin.json` - No version field
- README.md - Version not referenced
- Individual skill SKILL.md files - No version numbers

## Commit Message Format

When bumping versions, use this format:

```
<Brief description of changes>

- Bullet point list of specific changes
- Grouped by category (features, fixes, docs)

<Optional: Longer explanation of why changes were made>

Bump version to X.Y.Z

Co-Authored-By: Claude Sonnet 4.5 <noreply@anthropic.com>
```

## Release Checklist

- [ ] Update version in `.claude-plugin/marketplace.json`
- [ ] Update version in `MARKETPLACE.md`
- [ ] Verify with grep command above
- [ ] Commit with descriptive message
- [ ] Tag commit: `git tag vX.Y.Z`
- [ ] Push with tags: `git push origin main --tags`

## Testing After Changes

Before committing significant changes to skills, test:

1. **Template resolution:**
   - Run `/setup-notes` in a test directory
   - Verify `.claude/templates/` is created with all 4 templates
   - Run `/create-note` and verify templates load correctly

2. **Flavor system:**
   - Test each flavor: `ml`, `software-engineering`, `devops-sre`
   - Verify correct directories are created
   - Check protocol file has correct examples

3. **Documentation consistency:**
   - Check that README.md examples match actual behavior
   - Verify TEMPLATES.md instructions work as described

## Common Issues

### Template Path Resolution

Templates must be copied to `.claude/templates/` during `/setup-notes`:
- Use `Glob("**/skills/create-note/*-template.md")` to find them
- Read from plugin directory, write to `.claude/templates/`
- Never reference `$SKILL_DIR` (not a real variable)

### Flavor Files

Flavor markdown files in `skills/setup-notes/flavors/` must have:
- Configuration section with name, display_name, description
- Directory structure list
- Protocol examples
- INDEX.md examples

## Plugin Structure

```
claude-lab-notebook/
├── .claude-plugin/
│   └── marketplace.json          # VERSION HERE (main marketplace entry)
├── CLAUDE.md                     # This file
├── MARKETPLACE.md                # VERSION HERE (marketplace listing)
├── README.md                     # No version
├── TEMPLATES.md                  # No version
├── QUICK-START.md                # No version
└── plugins/claude-lab-notebook/
    ├── .claude-plugin/
    │   └── plugin.json          # No version field
    └── skills/
        ├── setup-notes/
        │   ├── SKILL.md
        │   └── flavors/
        │       ├── ml.md
        │       ├── software-engineering.md
        │       └── devops-sre.md
        ├── create-note/
        │   ├── SKILL.md
        │   ├── experiment-template.md
        │   ├── decision-template.md
        │   ├── troubleshooting-template.md
        │   └── meeting-template.md
        ├── create-note-type/
        │   └── SKILL.md
        ├── update-index/
        │   └── SKILL.md
        └── migrate-notes/
            └── SKILL.md
```
