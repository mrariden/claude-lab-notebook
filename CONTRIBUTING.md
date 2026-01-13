# Contributing to Experiment Notebook

Thank you for your interest in contributing!

## How to Contribute

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Make your changes
4. Test your changes with Claude Code
5. Commit (`git commit -m 'Add amazing feature'`)
6. Push to your fork (`git push origin feature/amazing-feature`)
7. Open a Pull Request

## What We're Looking For

- Bug fixes
- Improved templates
- Better documentation
- Additional slash commands
- Integration examples

## Testing Changes
```bash
# Copy your changes to Claude commands directory
cp .claude/commands/*.md ~/.claude/commands/

# Test in Claude Code
cd test-project
claude
> /setup-notes
> /create-note
> /update-index
```

## Code Style

- Use markdown for command files
- Keep instructions clear and specific
- Include examples in command descriptions
- Test all file paths

## Questions?

Open an issue or discussion!
