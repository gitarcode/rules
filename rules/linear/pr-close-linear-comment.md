---
title: "PR Close Linear Comment"
description: "Add implementation summary to linked Linear issues when PRs are merged"
slug: "linear_pr_close_comment"
when: "PR with linked Linear issue is merged"
actions: "Post comment to Linear issue summarizing what was implemented"
integrations: "linear"
---

# PR Close Linear Comment

Update linked Linear issues with implementation details when PRs are merged.

## When to Use This

- PR merged (not just closed)
- PR has linked Linear issue(s)
- Contains substantive code changes

Skip for abandoned PRs, pure docs changes, or automated dependency updates.

## How It Works

1. **Detect PR merge** and extract linked Linear issues
2. **Gather details**: commits, files changed, key changes
3. **Post comment** to each linked issue:
   ```markdown
   ## PR Merged: [PR Title](link)

   **Author:** @username | **Merged:** 2024-01-15

   ### Summary
   - Added UserAuthService with OAuth2
   - Updated LoginController
   - Added 12 unit tests
   ```
4. **Optionally transition** issue state to Done (if configured)

## Why This Matters

Creates permanent record linking code changes to business requirements. Stakeholders get notified of shipped work.
