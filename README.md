# Gitar Rules

Reusable Gitar rules for automating your repository workflows - no code required.

This repository collects community-contributed `.gitar/rules/*.md` files that define actions like commenting, labeling, assigning reviewers, and more - all written in natural language.

## What is Gitar?

[Gitar](https://gitar.ai) is CI for the age of AI. Create automated workflows using natural language specified in `.gitar/rules/*.md` files. When conditions are met, Gitar automatically executes actions - no code required. [Learn more](https://docs.gitar.ai/working-with-gitar#repository-rules).

## Quick Start

To use a rule from this repository:

```bash
# Create the rules directory in your repository
mkdir -p .gitar/rules

# Copy a rule you want to use (browse rules/ folder to find rules)
cp rules/product/brand-consistency.md .gitar/rules/

# Commit and push
git add .gitar/rules/
git commit -m "Add Gitar brand consistency rule"
git push
```

Gitar will automatically detect and apply the rules on your next pull request or commit.

**Tip**: Browse the `rules/` directory to explore all available rules organized by category.

## When Rules Trigger

Rules are evaluated when:
- **Every push to a PR** - Any new commits pushed to a pull request
- **Metadata updates** - Changes to PR title, description, reviewers, or labels

Want additional triggers? [Open an issue](https://github.com/gitar-ai/rules/issues/new) to request new trigger events.

## Available Rules

Browse the `rules/` directory to explore available rules organized by category:

### [Code Quality & Consistency](rules/code-quality/)
Enforce coding standards, style guidelines, and consistency across the codebase

### [Product Quality](rules/product/)
Ensure consistent user-facing product quality including branding, messaging, and localization

### [Reviews & Collaboration](rules/code-review/)
Enhance code review workflows and team collaboration

### [Workflows](rules/workflows/)
Automate common CI workflows and improve development processes

## How Rules Work

Gitar rules are markdown files with two key components:

### 1. YAML Frontmatter

Frontmatter provides metadata that helps Gitar determine rule applicability efficiently:

```yaml
---
title: "Log Statement Consistency"
description: "Enforce consistent logging style and structured format across the codebase"
when: "PRs that add or modify log statements"
actions: "Post inline comments comparing to existing patterns and apply 'logging-style' label"
---
```

Fields:
- **title**: Rule name
- **description**: Brief summary (1-2 sentences)
- **when**: Natural language trigger description
- **actions**: What Gitar will do
- **integrations** (optional): Required integrations (e.g., `slack`, `jira` etc.)


### 2. Markdown Content

The rule body describes:

1. **When** the rule should trigger (conditions)
2. **What** Gitar should check or validate
3. **How** Gitar should respond (actions)

Example rule structure:

```markdown
---
title: "Rule Name"
description: "Brief description of what this rule does"
when: "Natural language description of when to apply"
actions: "Brief summary of actions taken"
---

# Rule Name

Brief description of what this rule does.

## When to Use This

- Condition 1: Specific trigger
- Condition 2: Another trigger

## How It Works

1. Check for X against criteria
2. Validate Y matches expected pattern
3. If issues found → post inline comment with Z
4. Apply label "needs-review"
5. Reference external docs if needed

## Why This Matters

- Benefit 1: Specific value provided
- Benefit 2: Problem prevented
```

### Supported Actions

Gitar can:
- **Post comments** on pull requests or as inline code reviews
- **Apply labels** based on detected conditions
- **Assign reviewers** when specific changes are detected
- **Suggest code changes** with inline suggestions
- **Make the code changes** if [auto accept changes](https://go.gitar.ai/workflow-docs) is on
- **Validate** code quality, documentation, or compliance requirements
- **Integrate with other tools*** grab context from jira, send messages to slack, update notion docs.
- Much more - get creative with your rules!

## Contributing

We welcome contributions! If you've created a useful Gitar rule, please share it with the community.

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on:
- Rule writing best practices
- Testing your rules
- Submitting examples
- Documentation standards

## Example Use Cases

Beyond brand consistency, Gitar rules can automate:

- **Dependency Management** - Comment with upgrade guides when dependencies change
- **Documentation Sync** - Keep docs in sync when APIs change
- **Security Gates** - Require additional approvals when changes introduce security risks
- **PR Size Checks** - Flag large PRs and suggest breaking them down
- **Release Preparation** - Post deployment checklists before merging to main
- **Automated Changelogs** - Generate and update CHANGELOG.md with technical changes on PR merge
- **Release Notes** - Create user-facing product update announcements from merged features
- **Knowledge Base Updates** - Sync API changes to Confluence or Notion documentation automatically
- **Infrastructure Alerts** - Send Slack notifications when CI infrastructure failures are detected
- **Project Management Sync** - Summarize PR changes and post updates to Jira tickets or Notion on merge

## Resources

- [Gitar Documentation](https://docs.gitar.ai)
- [Report Issues](https://github.com/gitar-ai/rules/issues)

---

**Ready to automate your workflows?** Copy a rule, commit it to `.gitar/rules/`, and let Gitar handle the rest.
