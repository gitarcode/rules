---
title: "Multi-Issue Relationship Linking"
description: "Link related Linear issues when multiple are attached to a single change"
slug: "linear_multi_issue_relationship"
when: "PR/MR has multiple Linear issues that aren't already related"
actions: "Create 'related' relationships between unconnected issues in Linear"
integrations: "linear"
---

# Multi-Issue Relationship Linking

Establish relationships between Linear issues when worked on together in the same change.

## When to Use This

- References 2+ distinct Linear issues
- Issues don't already have a relationship (parent/child, related, blocking, duplicate)
- Issues are in the same Linear workspace

Skip if issues already connected or from different workspaces.

## How It Works

1. **Extract all Linear issues** from title, description, branch
2. **Fetch existing relationships** for each issue pair
3. **For unconnected pairs**, create "Related" relationship in Linear
4. **Post comment** on both issues noting the connection:
   ```markdown
   🔗 Linked as Related to [ABC-456] via #123
   ```

## Why This Matters

Builds connections between related work automatically. Helps with impact analysis and discovery.
