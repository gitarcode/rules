---
title: "Multi-Issue Relationship Linking"
description: "Link related Jira issues when multiple are attached to a single change"
slug: "jira_multi_issue_relationship"
when: "PR/MR has multiple Jira issues that aren't already related"
actions: "Create issue links between unconnected issues in Jira"
integrations: "jira"
---

# Multi-Issue Relationship Linking

Establish relationships between Jira issues when worked on together in the same change.

## When to Use This

- References 2+ distinct Jira issues
- Issues don't already have a link (relates to, blocks, duplicates, etc.)
- Issues are in the same Jira instance

Skip if issues already connected or from different Jira instances.

## How It Works

1. **Extract all Jira issues** from title, description, branch
2. **Fetch existing issue links** for each issue pair
3. **For unconnected pairs**, create "relates to" link in Jira
4. **Post comment** on both issues noting the connection:
   ```markdown
   🔗 Linked as "relates to" [PROJ-456] via #123
   ```

## Why This Matters

Builds connections between related work automatically. Helps with impact analysis and discovery.
