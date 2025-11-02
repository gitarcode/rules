---
title: "Rule Quality Standards"
description: "Ensures all rules follow established structure, conventions, and include proper YAML frontmatter"
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
   when: "Natural language trigger description"
   actions: "Brief summary of actions"
   # integrations: "github" - Optional, managed by maintainers
   ---
   ```

   **Required fields**: `title`, `description`, `when`, `actions`
   **Optional field**: `integrations` (maintainers add this, contributors should omit)

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

4. **Generate frontmatter suggestion** if missing or incomplete:
   - Extract `title` from H1
   - Extract `description` from first paragraph
   - Extract `when` from "When to Use This" section
   - Extract `actions` from "How It Works" section (summarize key actions)
   - **Do not** suggest `integrations` field (maintainers handle this)

5. **Post inline comments** for issues:
   - Missing or incomplete frontmatter → suggest auto-generated version
   - Missing sections → provide template with examples
   - Vague content → suggest specific improvements
   - Reference CONTRIBUTING.md for detailed guidelines

## Why This Matters

- **Workflow-Oriented**: "How It Works" matches real-world usage as procedures, not just validation
- **Easier to Write**: Users describe workflows naturally, Gitar extracts frontmatter automatically
