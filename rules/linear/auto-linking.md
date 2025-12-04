---
title: "Linear Auto-Linking"
description: "Find and attach relevant Linear issues to changes without linked tickets"
slug: "linear_pr_linking"
when: "PR/MR opened without any Linear issue reference"
actions: "Search author's recent Linear issues (30 days), suggest or attach best match"
integrations: "linear"
---

# Linear Auto-Linking

Automatically link relevant Linear issues when changes are created without ticket references.

## When to Use This

- Opened with no Linear reference in title, description, or branch name
- Author has Linear integration enabled

Skip if already references a Linear issue or has `no-ticket` label.

## How It Works

1. **Check for existing reference** in title, description, branch name
2. **Search author's recent issues** (30 days, active states: In Progress, Todo, Backlog)
3. **If no author match**, search team's recent unassigned issues
4. **Score relevance** by title similarity, description keywords, file paths
5. **Link or suggest**:
   - High confidence (≥80): Auto-link in description
   - Medium confidence (50-79): Post comment with suggestion
   - Low confidence (<50): List top 3 candidates for author to pick

## Why This Matters

Ensures changes are connected to tracked work for traceability, context, and metrics.
