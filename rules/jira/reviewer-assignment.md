---
title: "Reviewer Assignment from Issue"
description: "Auto-assign reviewers based on Jira issue watchers and stakeholders"
slug: "jira_pr_assignee_from_issue"
when: "PR/MR opened with linked Jira issue"
actions: "Suggest or assign reviewers from Jira issue watchers/stakeholders"
integrations: "jira"
---

# Reviewer Assignment from Issue

Auto-suggest reviewers based on who's involved in the linked Jira issue.

## When to Use This

- Opened with a linked Jira issue
- Has no reviewers assigned yet
- Jira issue has watchers, reporter, or commenters

Skip if already has reviewers or no Jira issue is linked.

## How It Works

1. **Extract linked Jira issue** from title, description, or branch
2. **Fetch issue stakeholders** from Jira:
   - Issue assignee (if different from author)
   - Issue reporter (if different from author)
   - Watchers
   - Recent commenters
3. **Map Jira users** via email or linked accounts
4. **Suggest reviewers** in a comment or auto-assign if configured:
   ```markdown
   👥 **Suggested Reviewers** (from Jira issue PROJ-123)

   - @jane-doe (issue assignee)
   - @john-smith (watcher)

   Add them as reviewers or reply `/assign @username`
   ```

## Why This Matters

Ensures the right people review changes. Stakeholders tracking work in Jira automatically get visibility into implementation.
