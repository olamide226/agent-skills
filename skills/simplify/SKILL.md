---
name: simplify
description: >
  Review recently changed code for reuse opportunities, quality issues, and unnecessary complexity,
  then fix every issue found in place. Use this skill after completing any feature, task, or
  implementation pass — especially before raising a PR, after a fast "make it work" phase, or
  whenever code feels heavier than the problem warrants. Trigger on phrases like "clean up",
  "simplify", "refactor", "code review", "pre-PR tidy", or "too complex".
license: MIT
compatibility: Claude Code, Cursor, VS Code, Windsurf, Codex
metadata:
  category: code-quality
  complexity: intermediate
  version: "2.0.0"
---

# Simplify

Review recently changed code for reuse opportunities, quality issues, and unnecessary complexity — then fix every issue found in place.

## When to Use

- After completing a feature task or spec phase before raising a PR
- When code feels "heavier" than the problem warrants
- After a fast implementation pass where quality was deferred
- As a pre-PR hygiene step to catch over-engineering early

## Execution Steps

### Step 1 — Identify Changed Files

Get the working set:

```bash
git diff --name-only          # unstaged changes
git diff --cached --name-only # staged changes
git diff HEAD~1 --name-only   # last commit
```

Read every changed file in full before touching anything.

### Step 2 — Review for Reuse

For each changed file, ask:

- **Duplication**: Does this logic already exist elsewhere in the codebase? Search for similar function names, patterns, or data transformations.
- **Reinvented utilities**: Is a custom loop, mapper, or converter doing what a shared utility, standard library function, or well-known package already does?
- **Abstraction leakage**: Is this module doing work that belongs in an existing service, adapter, or shared interface?
- **Copy-paste blocks**: Are any blocks of code structurally identical to another block in the same or nearby file?

Fix: Call the existing function / extract to a shared utility / remove the duplicate.

### Step 3 — Review for Quality

Check each changed file against common standards:

- **Naming**: Do functions, classes, and variables communicate intent without needing comments?
- **Function length**: Functions over ~20 lines that do more than one thing should be split.
- **Single responsibility**: Is each function/class doing exactly one thing?
- **Dead code**: Any unused imports, variables, functions, or commented-out blocks?
- **Magic values**: Any hardcoded strings, numbers, or identifiers that should be named constants?
- **Error messages**: Are error and exception messages descriptive and actionable?
- **Unnecessary complexity**: Abstract classes, generics, factory patterns, or design patterns applied to a problem that a plain function would solve.

Fix: Rename, split, delete dead code, replace magic values, simplify structure.

### Step 4 — Review for Efficiency

- **Redundant I/O**: Is the same resource (file, DB row, API response) fetched more than once in a single operation when it could be fetched once and passed?
- **Loop inefficiency**: Is there a loop that performs an expensive operation (network call, query, file read) per iteration when a batch operation would do?
- **Unnecessary intermediaries**: Are objects, collections, or buffers constructed only to be immediately transformed into something else?
- **Over-fetching**: Is a full record or payload loaded when only one or two fields are needed?

Fix: Consolidate reads, replace per-iteration calls with batch operations, remove unnecessary intermediary objects.

### Step 5 — Apply All Fixes

Edit the files directly. Do not just report issues — fix them.

After all edits:

1. Confirm the changes do not alter observable behaviour (same inputs → same outputs).
2. If tests exist for the simplified code, verify they still pass conceptually.
3. Report a concise summary:

```
Simplified: [filename]
  - Removed duplicate parseDate(), reused existing utils/date.ts
  - Extracted buildQueryString() (was 38 lines, now 9)
  - Replaced hardcoded "application/json" with CONTENT_TYPE_JSON constant
  - Removed unused import and dead variable `tempResult`
```

## What NOT to Change

- Do not refactor code **outside the changed file set** — stay scoped to what was modified.
- Do not change **behaviour**, only structure.
- Do not add new tests (use a test-generation skill for that).
- Do not introduce abstractions for **hypothetical future requirements**.
- Do not modify **append-only or generated artefacts** — schema migrations, compiled outputs, lock files, auto-generated bindings, and similar files should never be touched.