---
title: "Unaddressed Items Follow-up"
description: "Create Linear issues for unresolved comments and TODOs when PRs merge"
slug: "linear_unaddressed_items_followup"
when: "PR merged with unresolved review comments or new TODOs"
actions: "Create follow-up Linear issue assigned to PR author"
integrations: "linear"
---

# Unaddressed Items Follow-up

Create follow-up Linear issues when PRs merge with unresolved review comments or new TODO comments.

## When to Use This

- PR merged with unresolved review comments (not marked resolved)
- OR PR introduces new TODO/FIXME/HACK comments
- No existing follow-up issue already tracks these items

Skip for resolved comments, nit-only feedback, or pre-existing TODOs.

## How It Works

1. **Collect unresolved comments** (exclude author self-reviews, nits, approvals)
2. **Detect new TODOs** introduced in diff (TODO, FIXME, HACK, XXX)
3. **Create Linear issue** if significant items found:
   ```markdown
   ## Follow-up: PR #123 - [Title]

   ### Unresolved Comments
   - @reviewer on `auth.ts:45`: "Handle expired token case"

   ### New TODOs
   - `UserService.ts:89`: TODO: Add caching
   ```
4. **Assign to PR author**, label as `follow-up`, `tech-debt`
5. **Comment on PR** with link to tracking issue

## Why This Matters

Prevents TODOs and reviewer feedback from being forgotten. Creates accountability for technical debt.
