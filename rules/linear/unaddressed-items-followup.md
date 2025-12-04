---
title: "Unaddressed Items Follow-up"
description: "Create Linear issues for unresolved comments and TODOs when changes merge"
slug: "linear_unaddressed_items_followup"
when: "PR/MR merged with unresolved review comments or new TODOs"
actions: "Create follow-up Linear issue assigned to author"
integrations: "linear"
---

# Unaddressed Items Follow-up

Create follow-up Linear issues when changes merge with unresolved review comments or new TODO comments.

## When to Use This

- Merged with unresolved review comments (not marked resolved)
- OR introduces new TODO/FIXME/HACK comments
- No existing follow-up issue already tracks these items

Skip for resolved comments, nit-only feedback, or pre-existing TODOs.

## How It Works

1. **Collect unresolved comments** (exclude author self-reviews, nits, approvals)
2. **Detect new TODOs** introduced in diff (TODO, FIXME, HACK, XXX)
3. **Create Linear issue** if significant items found:
   ```markdown
   ## Follow-up: #123 - [Title]

   ### Unresolved Comments
   - @reviewer on `auth.ts:45`: "Handle expired token case"

   ### New TODOs
   - `UserService.ts:89`: TODO: Add caching
   ```
4. **Assign to author**, label as `follow-up`, `tech-debt`
5. **Comment** with link to tracking issue

## Why This Matters

Prevents TODOs and reviewer feedback from being forgotten. Creates accountability for technical debt.
