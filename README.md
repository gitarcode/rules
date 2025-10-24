# Gitar Rules

Reusable Gitar rules for automating your repository workflows - no code required.

This repository collects community-contributed `.gitar/rules/*.md` files that define actions like commenting, labeling, assigning reviewers, and more - all written in natural language.

**Use these examples as-is or modify them to match your project's needs.**

## What is Gitar?

[Gitar](https://gitar.ai) is CI for the age of AI. Create automated workflows using natural language specified in `.gitar/rules/*.md` files. When conditions are met, Gitar automatically executes actions - no code required. [Learn more](https://docs.gitar.ai/working-with-gitar#repository-rules).

## Quick Start

To use a rule from this repository:

```bash
# Create the rules directory in your repository
mkdir -p .gitar/rules

# Copy a rule you want to use
cp examples/brand.md .gitar/rules/

# Commit and push
git add .gitar/rules/
git commit -m "Add Gitar brand rule"
git push
```

Gitar will automatically detect and apply the rules on your next pull request or commit.

## When Rules Trigger

Rules are evaluated when:
- **Every push to a PR** - Any new commits pushed to a pull request
- **Metadata updates** - Changes to PR title, description, reviewers, or labels

Want additional triggers? [Open an issue](https://github.com/gitar-ai/rules/issues/new) to request new trigger events.

## Available Rules

### Code Quality & Consistency

**[Brand Consistency](examples/brand.md)** - Ensures consistent brand across web applications
- Validates meta tags, titles, and descriptions in Next.js, HTML, and manifest files
- Provides inline suggestions with corrected brand text
- Explains why consistency matters for SEO and social sharing

## How Rules Work

Gitar rules are simple markdown files that describe:

1. **When** the rule should trigger (conditions)
2. **What** Gitar should check or validate
3. **How** Gitar should respond (actions)

Example rule structure:

```markdown
# Rule Name

Brief description of what this rule does.

## When to Apply This Rule

- Condition 1
- Condition 2

## Actions

1. Check for X
2. If Y is found, comment with Z
3. Add label "needs-review"
```

### Supported Actions

Gitar can:
- **Post comments** on pull requests or as inline code reviews
- **Apply labels** based on detected conditions
- **Assign reviewers** when specific changes are detected
- **Suggest code changes** with inline suggestions
- **Make the code changes** if [auto accept changes](https://go.gitar.ai/workflow-docs) is on
- **Validate** code quality, documentation, or compliance requirements
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
- **Smart Reviewer Assignment** - Assign reviewers based on code complexity, risk level, or domain expertise
- **Security Gates** - Require additional approvals when changes introduce security risks
- **PR Size Checks** - Flag large PRs and suggest breaking them down
- **Release Preparation** - Post deployment checklists before merging to main

## Resources

- [Gitar Documentation](https://docs.gitar.ai)
- [Report Issues](https://github.com/gitar-ai/rules/issues)

---

**Ready to automate your workflows?** Copy a rule, commit it to `.gitar/rules/`, and let Gitar handle the rest.
