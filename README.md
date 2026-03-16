# agent-skills

A collection of skills for AI coding agents following the [Agent Skills](https://skills.sh/docs) format. Skills are packaged instructions and scripts that extend agent capabilities across Claude, Copilot, Cursor, and other AI assistants.

## Available Skills

### git-workflow

Common Git workflows and best practices for AI agents. Covers branching strategies, commit conventions, pull request workflows, and conflict resolution.

**Use when:**
- Creating branches or commits
- Writing commit messages
- Opening or reviewing pull requests
- Resolving merge conflicts
- Setting up a Git repository

### code-review

Structured code review guidelines to help AI agents provide thorough, actionable feedback. Covers correctness, performance, security, maintainability, and style.

**Use when:**
- "Review this code"
- "Check my PR"
- "Look for bugs"
- "Audit this file for issues"
- "Give me feedback on this implementation"

## Installation

```bash
npx skills add olamide226/agent-skills
```

## Usage

Skills are automatically available once installed. The agent will use them when relevant tasks are detected.

**Examples:**
```
Review this pull request for issues
```
```
Help me write a good commit message
```
```
What branching strategy should I use?
```

## Skill Structure

Each skill contains:
- `SKILL.md` — Instructions and metadata for the agent
- `scripts/` — Helper scripts for automation (optional)
- `references/` — Supporting documentation (optional)

## Contributing

To add a new skill:

1. Create a new directory under `skills/` using `kebab-case`
2. Add a `SKILL.md` file with YAML frontmatter (`name`, `description`) and instructions
3. Optionally add `scripts/` and `references/` directories
4. Package the skill: `cd skills && zip -r {skill-name}.zip {skill-name}/`

See `AGENTS.md` for full contribution guidelines.

## License

MIT