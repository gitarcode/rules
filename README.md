# Gitar Rules

Reusable Gitar rules for automating repository workflows — no code required.

This repository collects community-contributed `.gitar/rules/*.md` files that automate side effects like linking issues, posting updates, syncing project management tools, and more — all written in natural language.

## What is Gitar?

[Gitar](https://gitar.ai) is CI for the age of AI. Create automated workflows using natural language specified in `.gitar/rules/*.md` files. When conditions are met, Gitar automatically executes actions — no code required. [Learn more](https://docs.gitar.ai/working-with-gitar#repository-rules).

## Quick Start

To use a rule from this repository:

```bash
# Create the rules directory in your repository
mkdir -p .gitar/rules

# Copy a rule you want to use (browse rules/ folder to find rules)
cp rules/<category>/<rule>.md .gitar/rules/

# Commit and push
git add .gitar/rules/
git commit -m "Add Gitar rule"
git push
```

Gitar will automatically detect and apply the rules on your next pull request or commit.

**Tip**: Browse the `rules/` directory to explore all available rules organized by category.

## When Rules Trigger

Rules are evaluated when:
- **PR/MR is opened** — First-time open events
- **Every push to a PR/MR** — Any new commits pushed
- **Metadata updates** — Changes to title, description, reviewers, or labels

Want additional triggers? [Open an issue](https://github.com/gitar-ai/rules/issues/new) to request new trigger events.

## Available Rules

Browse the `rules/` directory to explore available rules organized by category:

### [Jira Issue Management](rules/jira/)
Automate Jira issue tracking and PR/MR-to-issue synchronization workflows

### [Linear Issue Management](rules/linear/)
Automate Linear issue tracking and PR/MR-to-issue synchronization workflows

## How Rules Work

Gitar rules are markdown files with two key components:

### 1. YAML Frontmatter

Frontmatter provides metadata that helps Gitar determine rule applicability efficiently:

```yaml
---
title: "Jira Auto-Linking"
description: "Find and attach relevant Jira issues to PRs without linked tickets"
when: "PR/MR is first opened without any Jira issue reference"
actions: "Search author's recent Jira issues and link or suggest candidates"
integrations: "jira"
---
```

Fields:
- **title**: Rule name
- **description**: Brief summary (1-2 sentences)
- **when**: Natural language trigger description
- **actions**: What Gitar will do
- **integrations** (optional): Required integrations (e.g., `slack`, `jira`, `linear`)


### 2. Markdown Content

The rule body describes:

1. **When** the rule should trigger (conditions)
2. **What** Gitar should do (the automation)
3. **How** Gitar should respond (side effects and actions)

Example rule structure:

```markdown
---
title: "Rule Name"
description: "Brief description of what this rule automates"
when: "Natural language description of when to trigger"
actions: "Brief summary of actions taken"
integrations: "jira"
---

# Rule Name

Brief description of what this rule automates.

## When to Use This

- Condition 1: Specific trigger
- Condition 2: Another trigger

## How It Works

1. Detect relevant context from the PR/MR
2. Query external system for matching data
3. Perform action (link issue, post comment, update tracker)

## Why This Matters

- Benefit 1: Specific value provided
- Benefit 2: Time saved
```

### What Rules Can Automate

Gitar rules focus on side effects — actions triggered by repository events:
- **Link issues** — Automatically connect PRs to Jira or Linear issues
- **Post updates** — Comment on PRs or push status updates to project trackers
- **Sync tools** — Keep Jira, Linear, Slack, Notion, and other tools in sync with code changes
- **Apply labels** — Tag PRs based on detected patterns
- **Send notifications** — Alert teams via Slack or other channels
- **Update docs** — Sync documentation when APIs or code change

## Example Use Cases

- **Issue Auto-Linking** — Automatically find and attach Jira/Linear issues to PRs missing ticket references
- **Merge Updates** — Post summaries to linked issues when PRs are merged
- **Follow-up Tracking** — Create follow-up issues for unresolved review comments
- **Knowledge Base Sync** — Push API changes to Confluence or Notion automatically
- **Infrastructure Alerts** — Send Slack notifications when CI failures are detected
- **Project Management Sync** — Summarize PR changes and post updates to Jira or Linear on merge

## Contributing

We welcome contributions! If you've created a useful Gitar rule, please share it with the community.

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on:
- Rule writing best practices
- Testing your rules
- Submitting examples
- Documentation standards

## Resources

- [Gitar Documentation](https://docs.gitar.ai)
- [Report Issues](https://github.com/gitar-ai/rules/issues)

---

**Ready to automate your workflows?** Copy a rule, commit it to `.gitar/rules/`, and let Gitar handle the rest.
