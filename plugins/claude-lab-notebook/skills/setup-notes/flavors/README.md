# Flavors

Flavors customize the note-taking system for different domains. Each flavor defines:

- Directory structure (e.g., `experiments/` vs `spikes/`)
- Protocol examples with domain-specific terminology
- INDEX.md section templates
- Tool integration references

## Available Flavors

| Flavor | Primary Directory | Use Case |
|--------|-------------------|----------|
| `ml` | `experiments/` | Machine learning, data science, research |
| `software-engineering` | `spikes/` | Web development, APIs, system design |
| `devops-sre` | `incidents/` | Infrastructure, on-call, incident response |

## Creating a Custom Flavor

1. Create a new file in this directory: `your-flavor.md`

2. Follow this structure:

```markdown
# Your Flavor Name

Brief description of the domain.

## Configuration

\```yaml
name: your-flavor
display_name: Your Flavor Name
description: One-line description
\```

## Directory Structure

\```yaml
directories:
  - primary-dir    # Main work logs
  - decisions      # Choice documentation
  - troubleshooting # Error patterns
  - other-dir      # Domain-specific
\```

## Protocol Examples

### When to Load Additional Files

**User says:** "[domain-specific question]"
**You respond:** "[domain-appropriate response with file reference]"

[Add 2-3 examples relevant to your domain]

### Progressive Disclosure Example Session

[Show a realistic session in your domain]

## INDEX.md Examples

[Show section templates with domain-specific tags and findings]

## quick-reference.md Format

\```yaml
format: [yaml|env_vars|json|other]
example: |
  [domain-appropriate config format]
\```

## Tool Integration

- **Tool Category**: tool1, tool2
[List tools commonly used in your domain]
```

## Flavor File Reference

### Required Sections

- **Configuration**: Name and description
- **Directory Structure**: List of directories to create
- **Protocol Examples**: Domain-specific file loading examples
- **INDEX.md Examples**: Section templates with example entries

### Optional Sections

- **Progressive Disclosure Example Session**: Full session walkthrough
- **quick-reference.md Format**: Configuration format preference
- **Tool Integration**: Common tools in the domain

## Example Custom Flavors

### Game Development

```yaml
directories:
  - prototypes     # Gameplay experiments
  - decisions      # Design choices
  - troubleshooting # Performance, bugs
  - design         # GDDs, mechanics docs
```

### DevOps/SRE

```yaml
directories:
  - incidents      # Postmortems, RCAs
  - decisions      # Infrastructure choices
  - troubleshooting # Runbooks
  - architecture   # System diagrams
```

### Academic Research

```yaml
directories:
  - experiments    # Lab work, trials
  - decisions      # Methodology choices
  - literature     # Paper reviews
  - data           # Dataset documentation
```

## Using Your Custom Flavor

Once created, use it with:

```
/setup-notes your-flavor
```

The system will read your flavor file and apply its configuration.
