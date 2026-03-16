# AGENTS.md

This file provides guidance to AI coding agents (Claude Code, Cursor, Copilot, etc.) when working with code in this repository.

## Repository Overview

A collection of skills for AI coding agents following the [Agent Skills](https://skills.sh/docs) format. Skills are packaged instructions and scripts that extend agent capabilities.

## Creating a New Skill

### Directory Structure

```
skills/
  {skill-name}/           # kebab-case directory name
    SKILL.md              # Required: skill definition and metadata
    scripts/              # Optional: executable scripts
      {script-name}.sh    # Bash scripts (preferred)
    references/           # Optional: supporting documentation
  {skill-name}.zip        # Required: packaged for distribution
```

### Naming Conventions

- **Skill directory**: `kebab-case` (e.g., `git-workflow`, `code-review`)
- **SKILL.md**: Always uppercase, always this exact filename
- **Scripts**: `kebab-case.sh` (e.g., `run.sh`, `fetch-data.sh`)
- **Zip file**: Must match directory name exactly: `{skill-name}.zip`

### SKILL.md Format

```markdown
---
name: {skill-name}
description: {One sentence describing what the skill does and when to use it. Include trigger phrases like "Review my code", "Help with git", etc.}
license: MIT
metadata:
  author: {author}
  version: "1.0.0"
---

# {Skill Title}

{Brief description of what the skill does.}

## When to Use

{Bulleted list of situations where this skill applies}

## Instructions

{Detailed instructions the agent should follow}

## Examples

{2-3 concrete usage examples}
```

### SKILL.md Frontmatter Fields

| Field | Required | Description |
|-------|----------|-------------|
| `name` | ✅ | Unique identifier — lowercase, hyphens only, must match directory name |
| `description` | ✅ | What it does and when to use it; include trigger phrases |
| `license` | ❌ | License identifier (e.g., `MIT`, `Apache-2.0`) |
| `metadata.author` | ❌ | Author name or organization |
| `metadata.version` | ❌ | Semantic version string |

### Best Practices

- **Keep SKILL.md under 500 lines** — move comprehensive examples, extended documentation, or language-specific guides into `references/` files so the main skill definition stays focused and scannable
- **Write specific descriptions** — helps the agent know exactly when to activate the skill
- **Use progressive disclosure** — reference supporting files that get read only when needed
- **Prefer scripts over inline code** — script execution doesn't consume context (only output does)

### Script Requirements

When adding scripts under `scripts/`:

- Use `#!/bin/bash` shebang
- Use `set -e` for fail-fast behavior
- Write status messages to stderr: `echo "Message" >&2`
- Write machine-readable output (JSON preferred) to stdout
- Include a cleanup trap for temporary files

### Creating the Zip Package

After creating or updating a skill, regenerate its zip:

```bash
cd skills
zip -r {skill-name}.zip {skill-name}/
```

### Updating an Existing Skill

1. Edit the relevant files inside `skills/{skill-name}/`
2. Regenerate the zip: `cd skills && zip -r {skill-name}.zip {skill-name}/`
3. Commit both the source directory changes and the updated zip

## Repository-Level Files

| File | Purpose |
|------|---------|
| `README.md` | Human-facing documentation listing all available skills |
| `AGENTS.md` | This file — guidance for AI agents working in this repo |
| `skills/` | All skill directories and distribution zips |

When adding a new skill, update `README.md` to include the skill name, description, and usage triggers.
