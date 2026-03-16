---
name: code-review
description: Structured code review guidelines for AI agents. Use this skill when asked to review code, check a pull request, audit a file for bugs or security issues, or give feedback on an implementation.
license: MIT
metadata:
  author: olamide226
  version: "1.0.0"
---

# Code Review Skill

Provides a structured approach for AI agents to deliver thorough, actionable code reviews. Covers correctness, security, performance, maintainability, and style.

## When to Use

- "Review this code"
- "Check my PR"
- "Look for bugs"
- "Audit this file for issues"
- "Give me feedback on this implementation"
- "Is this code secure?"
- "How can I improve this?"

## Review Checklist

Work through each category below. Flag every issue you find and provide a concrete suggestion for how to fix it.

### 1. Correctness

- [ ] Does the code do what it is supposed to do?
- [ ] Are edge cases handled? (empty input, null/undefined, zero, max values)
- [ ] Are error conditions caught and handled gracefully?
- [ ] Are all code paths reachable and correct?
- [ ] Are there off-by-one errors in loops or array accesses?
- [ ] Are asynchronous operations awaited correctly?
- [ ] Are race conditions possible?

### 2. Security

- [ ] Is user input validated and sanitized before use?
- [ ] Are SQL queries parameterized (no string interpolation)?
- [ ] Are secrets, API keys, or credentials absent from the code?
- [ ] Is authentication checked before accessing protected resources?
- [ ] Are file paths sanitized to prevent path traversal?
- [ ] Is output HTML-escaped to prevent XSS?
- [ ] Are dependencies up to date and free of known vulnerabilities?

### 3. Performance

- [ ] Are there unnecessary database or API calls inside loops?
- [ ] Is pagination or streaming used for large datasets?
- [ ] Are expensive computations cached where appropriate?
- [ ] Are N+1 query problems avoided?
- [ ] Are resources (file handles, connections) closed after use?

### 4. Maintainability

- [ ] Are functions and classes small and focused (single responsibility)?
- [ ] Are variables and functions named clearly and consistently?
- [ ] Is complex logic explained with a comment?
- [ ] Is duplicated logic extracted into a shared function?
- [ ] Are magic numbers replaced with named constants?
- [ ] Is dead code (unreachable or unused) removed?

### 5. Test Coverage

- [ ] Are new functions covered by unit tests?
- [ ] Are edge cases and error paths tested?
- [ ] Do tests assert meaningful behavior, not just that code runs without error?
- [ ] Are tests independent (no shared mutable state between tests)?

### 6. Style & Conventions

- [ ] Does the code follow the project's existing style (indentation, naming)?
- [ ] Are imports organized consistently?
- [ ] Are there no unnecessary console.log / print / debug statements left in?

## How to Present Findings

Group findings by severity:

**🔴 Critical** — Must fix before merging (security vulnerabilities, data loss risk, broken functionality)

**🟠 Major** — Should fix before merging (logic errors, missing error handling, significant performance issues)

**🟡 Minor** — Good to fix (code clarity, style, minor inefficiencies)

**🟢 Suggestion** — Optional improvement (refactoring ideas, alternative approaches)

For each finding, provide:
1. **Location** — file name and line number(s)
2. **Issue** — what is wrong and why it matters
3. **Suggestion** — concrete code snippet or description of the fix

## Example Output Format

```
## Code Review: `src/auth/login.js`

### 🔴 Critical

**Line 42 — SQL Injection Risk**
User input is interpolated directly into a SQL query string:
```js
// ❌ Vulnerable
db.query(`SELECT * FROM users WHERE email = '${email}'`);
```
Use a parameterized query instead:
```js
// ✅ Safe
db.query('SELECT * FROM users WHERE email = ?', [email]);
```

---

### 🟠 Major

**Lines 67–72 — Missing error handling on async call**
`fetchUserProfile()` is awaited but no try/catch wraps the call. A network error will crash the request handler.

---

### 🟡 Minor

**Line 15 — Magic number**
`if (attempts > 5)` — extract `5` into a named constant `MAX_LOGIN_ATTEMPTS` for clarity.

---

### 🟢 Suggestion

Consider extracting the token-generation logic (lines 80–95) into a dedicated `generateToken()` helper to improve testability.
```

## Tips for Good Reviews

- Be specific — point to exact lines, not just files
- Explain *why* something is a problem, not just *what* it is
- Offer a fix for every issue you raise
- Acknowledge what is done well — positive feedback is also useful
- Prioritize: address critical issues thoroughly before style notes
