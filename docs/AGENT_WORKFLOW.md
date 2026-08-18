# 🤖 Agent Workflow — How Bug-Fix Agent Works

## Overview

This document describes the automated pipeline that runs every night at 8 PM IST.

## Pipeline Steps

### 1. Scan Submitted Repos

- Read all open issues with label `repo-submitted` in `bug-fix-agent` repo
- Extract repo URL, language, fix types, and focus areas from the issue
- Filter repos that are public and accessible
- Prioritize Python repos

### 2. Fork & Clone

- Fork the target repo to `as7111771-create/<repo-name>`
- Get the file tree recursively
- Identify Python files (`.py`), config files, and test files

### 3. Code Analysis

For each Python file, the agent checks:

| Check | Description |
|-------|-------------|
| Syntax Errors | Parse errors, indentation issues |
| Import Issues | Missing imports, circular imports, broken modules |
| Type Errors | Missing type hints, wrong return types, mypy issues |
| Runtime Bugs | KeyError, IndexError, TypeError, AttributeError patterns |
| Security | Hardcoded secrets, SQL injection, unsafe eval/exec |
| Code Quality | PEP 8 violations, dead code, unused variables/imports |
| Error Handling | Missing try/except, bare except, unhandled edge cases |
| Test Coverage | Functions without tests, untested edge cases |

### 4. Fix Generation

- For each bug found, generate a minimal, safe fix
- Do NOT change behavior or add features
- Follow the project's existing code style
- Add comments where the fix is non-obvious
- Group related fixes into a single commit

### 5. PR Submission

- Create a branch: `bugfix/auto-fix-<date>`
- Commit fixes
- Open a PR with:
  - Clear title and description
  - List of bugs found and fixed
  - Files changed
  - Test recommendations
- Link back to the original submission issue

### 6. Comment on Issue

- Comment on the submission issue with:
  - PR link
  - Summary of bugs found
  - Number of files changed
  - Recommendations for the repo owner

### 7. Summary Delivery

- Post a summary in the agent's conversation:
  - Repos processed
  - Bugs found and fixed
  - PRs opened
  - Stats (files scanned, lines analyzed)

## Safety Rules

- ❌ NEVER force-push to any repo
- ❌ NEVER make destructive changes
- ❌ NEVER modify tests to make them pass artificially
- ❌ NEVER add new features — only fix bugs
- ✅ ALWAYS work on a branch
- ✅ ALWAYS submit fixes as PRs
- ✅ ALWAYS follow project conventions
- ✅ ALWAYS leave the repo better than found

## Language Support

| Language | Status |
|----------|--------|
| Python | ✅ Full support |
| JavaScript | 🟡 Partial support |
| TypeScript | 🟡 Partial support |
| Go | 🔜 Coming soon |
| Rust | 🔜 Coming soon |
