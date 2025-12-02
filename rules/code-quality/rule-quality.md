---
title: "Rule Quality Standards"
description: "Ensures all rules follow established structure, conventions, and include proper YAML frontmatter"
slug: "rule_quality"
when: "Gitar rule markdown files are added or modified"
actions: "Post inline comments with structural suggestions and auto-generated frontmatter"
---

# Rule Quality Standards

Ensure all new or modified gitar rule files follow the established conventions and structure.

## When to Use This

Apply when gitar rule markdown files are added or modified.

## How It Works

1. **Validate YAML frontmatter** at top of file:
   ```yaml
   ---
   title: "Rule Title"
   description: "Brief 1-2 sentence description"
   slug: "category_rule_name"
   when: "Natural language trigger description"
   actions: "Brief summary of actions"
   integrations: "linear"  # Only external integrations, not VCS
   ---
   ```

   **Required fields**: `title`, `description`, `slug`, `when`, `actions`
   **Optional field**: `integrations` (only for external services)

   **Slug naming conventions**:
   - Use snake_case
   - Prefix with integration name for integration-specific rules (e.g., `linear_pr_linking`, `slack_notify`)
   - This ensures unique slugs across categories and enables filtering by integration

   **Integrations field conventions**:
   - **DO NOT** include VCS providers (`github`, `gitlab`, `bitbucket`) - these are implicit from the repository containing the rules
   - **DO** include external integrations that the rule depends on (e.g., `linear`, `slack`, `jira`, `notion`)
   - Multiple integrations: `integrations: "linear, slack"`

2. **Check for 3 required sections**:
   - **H1 Title** + 1-2 sentence description
   - **## When to Use This** - Bulleted list of trigger conditions
   - **## How It Works** - Step-by-step procedure (what to check, what actions to take, conditionals, references to files/docs)
   - **## Why This Matters** - Business/technical value explanation

3. **Validate content quality**:
   - "When to Use This" is clear and specific
   - "How It Works" uses numbered steps and shows the workflow/procedure
   - Code examples included where relevant
   - Actions described in active voice
   - "Why This Matters" explains value

4. **Generate frontmatter inline suggestion** if missing or incomplete:
   - Extract `title` from H1
   - Extract `description` from first paragraph
   - Extract `when` from "When to Use This" section
   - Extract `actions` from "How It Works" section (summarize key actions)
   - **Do not** suggest `integrations` field (maintainers handle this)
   - Post as an inline code suggestion at where the frontmatter should be

5. **Post inline comments** for issues:
   - Missing or incomplete frontmatter → suggest auto-generated version
   - Missing sections → provide template with examples
   - Vague content → suggest specific improvements
   - Reference CONTRIBUTING.md for detailed guidelines

## Why This Matters

- **Workflow-Oriented**: "How It Works" matches real-world usage as procedures, not just validation
- **Easier to Write**: Users describe workflows naturally, Gitar extracts frontmatter automatically
