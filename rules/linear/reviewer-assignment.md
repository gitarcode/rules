---
title: "Reviewer Assignment from Issue"
description: "Auto-assign reviewers based on Linear issue watchers and stakeholders"
slug: "linear_pr_assignee_from_issue"
when: "PR/MR opened with linked Linear issue"
actions: "Suggest or assign reviewers from Linear issue subscribers/watchers"
integrations: "linear"
---

# Reviewer Assignment from Issue

Auto-suggest reviewers based on who's involved in the linked Linear issue.

## When to Use This

- Opened with a linked Linear issue
- Has no reviewers assigned yet
- Linear issue has subscribers, watchers, or commenters

Skip if already has reviewers or no Linear issue is linked.

## How It Works

1. **Extract linked Linear issue** from title, description, or branch
2. **Fetch issue stakeholders** from Linear:
   - Issue assignee (if different from author)
   - Issue creator (if different from author)
   - Subscribers/watchers
   - Recent commenters
3. **Map Linear users** via email or linked accounts
4. **Suggest reviewers** in a comment or auto-assign if configured:
   ```markdown
   👥 **Suggested Reviewers** (from Linear issue ABC-123)

   - @jane-doe (issue assignee)
   - @john-smith (subscriber)

   Add them as reviewers or reply `/assign @username`
   ```

## Why This Matters

Ensures the right people review changes. Stakeholders tracking work in Linear automatically get visibility into implementation.
