---
name: git-workflow
description: Common Git workflows and best practices. Use this skill when creating branches, writing commit messages, opening pull requests, resolving merge conflicts, or asking for help with any Git operation.
license: MIT
metadata:
  author: olamide226
  version: "1.0.0"
---

# Git Workflow Skill

Guides AI agents through common Git workflows following best practices for branching, committing, pull requests, and conflict resolution.

## When to Use

- Creating a new branch or starting a feature
- Writing a commit message
- Opening or reviewing a pull request
- Resolving merge conflicts
- Setting up a new repository
- Undoing or amending commits

## Branching Strategy

Use short-lived feature branches off `main` (or `develop` if the project uses Gitflow).

**Branch naming convention:**

| Type | Pattern | Example |
|------|---------|---------|
| Feature | `feat/{short-description}` | `feat/add-login-page` |
| Bug fix | `fix/{short-description}` | `fix/null-pointer-on-signup` |
| Chore / tooling | `chore/{short-description}` | `chore/update-dependencies` |
| Docs | `docs/{short-description}` | `docs/update-readme` |
| Release | `release/{version}` | `release/1.2.0` |

Always branch from an up-to-date `main`:

```bash
git checkout main
git pull origin main
git checkout -b feat/my-feature
```

## Commit Messages

Follow the [Conventional Commits](https://www.conventionalcommits.org/) specification:

```
<type>(<optional scope>): <short summary>

<optional body — explain WHY, not WHAT>

<optional footer: BREAKING CHANGE or issue refs>
```

**Types:**
- `feat` — new feature
- `fix` — bug fix
- `docs` — documentation only
- `style` — formatting, missing semicolons, etc. (no logic change)
- `refactor` — code change that neither fixes a bug nor adds a feature
- `test` — adding or updating tests
- `chore` — build process, tooling, dependency updates

**Rules:**
- Summary line ≤ 50 characters, imperative mood ("add feature" not "added feature"); body lines ≤ 72 characters
- Reference issues in the footer: `Closes #42` or `Refs #42`
- Mark breaking changes: `BREAKING CHANGE: <description>` in the footer

**Examples:**

```
feat(auth): add OAuth2 login via GitHub

Allows users to sign in with their GitHub account.
Stores the access token in the session, not localStorage.

Closes #18
```

```
fix(api): handle null response from payment service

The payment provider occasionally returns null on timeout.
Previously this caused an unhandled exception; now it
returns a 503 with a user-friendly message.
```

## Pull Requests

1. **Keep PRs small and focused** — one logical change per PR
2. **Write a clear description** that answers:
   - What does this change?
   - Why is this change needed?
   - How was it tested?
3. **Link related issues** using `Closes #<number>` or `Refs #<number>`
4. **Request specific reviewers** who know the affected code
5. **Respond to review comments promptly** and resolve threads after addressing them
6. **Squash or rebase** before merging to keep history clean

**PR description template:**

```markdown
## Summary
<!-- What does this PR do? -->

## Motivation
<!-- Why is this change needed? -->

## Changes
- 
- 

## Testing
<!-- How was this tested? -->

## Related Issues
Closes #
```

## Merge Conflict Resolution

When a merge conflict occurs:

1. Identify conflicting files: `git status`
2. Open each file and resolve sections marked with `<<<<<<<`, `=======`, `>>>>>>>`
3. After resolving, stage the file: `git add <file>`
4. Complete the merge: `git commit`
5. Never blindly accept one side — understand both changes before resolving

## Undoing Changes

| Situation | Command |
|-----------|---------|
| Undo last commit (keep changes staged) | `git reset --soft HEAD~1` |
| Undo last commit (unstage changes) | `git reset HEAD~1` |
| Discard all uncommitted changes | `git checkout -- .` |
| Amend the last commit message | `git commit --amend` |
| Revert a pushed commit safely | `git revert <sha>` |

**Never force-push to shared branches** (`main`, `develop`). Force-push is only acceptable on your own feature branch before it has been reviewed.

## Keeping a Branch Up to Date

Prefer rebase over merge to maintain a linear history on feature branches:

```bash
git fetch origin
git rebase origin/main
```

If conflicts arise during rebase, resolve them file by file, then `git rebase --continue`.
