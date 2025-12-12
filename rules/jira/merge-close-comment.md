---
title: "Merge Close Jira Comment"
description: "Add implementation summary to linked Jira issues when changes are merged"
slug: "jira_pr_close_comment"
when: "PR/MR with linked Jira issue is merged"
actions: "Post comment to Jira issue summarizing what was implemented"
integrations: "jira"
---

# Merge Close Jira Comment

Update linked Jira issues with implementation details when changes are merged.

## When to Use This

- Merged (not just closed)
- Has linked Jira issue(s)
- Contains substantive code changes

Skip for abandoned changes, pure docs changes, or automated dependency updates.

## How It Works

1. **Detect merge** and extract linked Jira issues
2. **Gather details**: commits, files changed, key changes
3. **Post comment** to each linked issue:
   ```markdown
   ## Merged: [Title](link)

   **Author:** @username | **Merged:** 2024-01-15

   ### Summary
   - Added UserAuthService with OAuth2
   - Updated LoginController
   - Added 12 unit tests
   ```
4. **Optionally transition** issue status to Done/Resolved (if configured)

## Why This Matters

Creates permanent record linking code changes to business requirements. Stakeholders get notified of shipped work.
