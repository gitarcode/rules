---
title: "PR Conventions Auto-Fix"
description: "Automatically fix PR title, grammar, and formatting issues when CI validation fails"
slug: "pr_conventions_fix"
when: "CI job fails due to PR title/description validation errors"
actions: "Parse validation error, auto-fix title/description grammar and formatting, re-trigger CI"
---

# PR Conventions Auto-Fix

Automatically fix PR title and description issues (grammar, formatting, layout, issue references) when CI validation fails.

## When to Use This

- CI job fails with PR title/description validation errors
- Error is a fixable grammar, formatting, or layout issue
- PR is not a draft

Skip if failure requires human judgment (ambiguous meaning, content decisions).

## How It Works

1. **Identify candidate CI failures** by job name and logs:

   **Job names that suggest PR validation:**
   - `lint`, `pr-lint`, `title-check`, `pr-check`
   - `semantic-pr`, `commitlint`, `danger`
   - `validate-pr`, `pr-validation`, `check-pr-title`

   **Error patterns in logs:**
   ```
   PR title must match pattern...
   Title should be capitalized
   Title should not end with period
   PR description is empty
   Missing required section
   Missing Jira ticket in title
   No Linear issue found
   Ticket reference required: [PROJ-123]
   Issue key not found in PR title or branch
   ```

2. **Parse violation and apply fix**:

   | Violation | Auto-Fix |
   |-----------|----------|
   | **Grammar** | |
   | Title not capitalized | Capitalize first letter |
   | Title ends with period | Remove trailing punctuation |
   | Inconsistent casing | Apply title case or sentence case per repo style |
   | **Formatting** | |
   | Missing type prefix | Infer from branch/files: `feat:`, `fix:`, `chore:` |
   | Wrong prefix case | Lowercase the prefix |
   | Title too long | Truncate, move detail to description |
   | **Layout** | |
   | Empty description | Generate summary from commits |
   | Missing template sections | Fill with inferred content |
   | Malformed markdown | Fix heading levels, list formatting |
   | **Issue References** | |
   | Missing Jira ticket | Extract `PROJ-123` from branch name, commits, or linked issues |
   | Missing Linear issue | Extract `ABC-123` from branch name or find linked Linear issue |
   | Wrong ticket format | Reformat to match required pattern: `[PROJ-123]`, `PROJ-123:`, etc. |
   | Ticket in wrong location | Move from description to title (or vice versa) per repo convention |

3. **Apply fix and notify**:
   ```markdown
   🔧 **Auto-fixed PR structure**

   CI failed due to: `Title should not end with period`

   **Before:** `Add user authentication.`
   **After:** `Add user authentication`

   Re-running CI...
   ```

4. **Re-trigger the failed CI job**

5. **Prevent loops**: Max 1 auto-fix attempt per error type per PR

## Why This Matters

- **Reduced friction**: Developers don't wait for trivial formatting fixes
- **Faster merges**: Eliminates back-and-forth on style issues
- **Consistent history**: All PRs follow conventions automatically
