# Contributing to Gitar Rules

We welcome contributions from the community! Whether you're sharing a rule you've been using in production or creating a new example to help others, your contributions help make CI automation more accessible.

## How to Contribute

### 1. Share Your Rule

If you have a Gitar rule that works well for your team, consider sharing it:

1. **Fork this repository**
2. **Choose the appropriate category** in the `rules/` directory
3. **Write your rule** following the markdown structure below (don't worry about frontmatter - Gitar will suggest it!)
4. **Submit a pull request** with a clear description of what your rule does
5. **Review Gitar's suggestions** for frontmatter and structure, then update as needed

### 2. Improve Existing Rules

Found a way to make an existing rule better? We'd love to see it:

- Clarify instructions or conditions
- Add edge cases or additional validations
- Improve error messages or suggestions
- Fix bugs or inconsistencies

### 3. Report Issues

If you find a problem with a rule or have suggestions:

- [Open an issue](https://github.com/gitar-ai/rules/issues/new) describing the problem
- Include the rule name and what behavior you expected vs. what happened
- Share your repository context if relevant (language, framework, etc.)

## Rule Writing Guidelines

### Structure

Every rule must include two components: **YAML frontmatter** and **markdown content**.

#### 1. YAML Frontmatter (Auto-Suggested)

**Don't worry about writing frontmatter yourself!** When you submit a rule, Gitar will automatically analyze your content and suggest appropriate frontmatter. You can then review and adjust as needed.

However, if you want to include it, here's the format:

```yaml
---
title: "Rule Title"
description: "Brief description (1-2 sentences)"
when: "Natural language description of when this rule applies"
actions: "Brief summary of what actions this rule takes"
---
```

**Frontmatter Field Guide:**

- **title**: Clear, descriptive title (3-8 words)
- **description**: Brief summary for (1-2 sentences)
- **when**: Natural language one-liner describing when this rule applies. Don't over-specify file patterns or change types - Gitar can infer most of this. Be natural and clear.
- **actions**: Brief summary of what actions Gitar will take (1 sentence)

**Example:**

```yaml
---
title: "Log Statement Consistency"
description: "Enforce consistent logging style and structured format across the codebase"
when: "PRs that add or modify log statements"
actions: "Post inline comments comparing to existing patterns and apply 'logging-style' label"
---
```

#### 2. Markdown Content (Required)

After frontmatter, include these 3 sections:

```markdown
# Rule Title

Brief 1-2 sentence description of what this rule does.

## When to Use This

Clear list of conditions that trigger this rule:
- Specific file patterns or paths
- Types of changes (new files, modifications, deletions)
- Specific conditions that apply

## How It Works

Step-by-step procedure showing the workflow:
1. Check/validate something against criteria
2. Compare against existing patterns or reference files
3. Post inline comments with suggestions
4. Apply labels or assign reviewers
5. Update PR descriptions or other actions

Include:
- Code examples showing correct vs. incorrect patterns
- References to external files/docs if used
- Conditionals ("if X then Y")
- The complete procedure from detection to action

## Why This Matters

Brief explanation of the value:
- What problems does this prevent?
- What benefits does it provide?
- What workflows does it improve?
```

### Best Practices

**Be Specific**
- Use concrete file paths and patterns (`*.tsx`, `package.json`)
- Provide exact examples of what to check
- Include code snippets when relevant

**Be Clear**
- Write in active voice
- Use simple, direct language

**Be Actionable**
- Focus on what Gitar should *do*, not just what it should *check*
- Provide helpful error messages and suggestions
- Include links to documentation when relevant

**Be Helpful**
- Explain *why* the rule matters, not just *what* it does. This helps Gitar explain to the author why this rule applied.
- Consider edge cases and document exceptions
- Provide examples of both compliant and non-compliant code

### Testing Your Rule

Before submitting:

1. **Test in your own repository**
   ```bash
   cp your-rule.md .gitar/rules/
   git add .gitar/rules/your-rule.md
   git commit -m "Add new rule for testing"
   git push
   ```
2. **Create a test PR** that should trigger the rule
3. **Verify the behavior** matches your expectations
4. **Iterate** based on actual behavior

### Documentation

When adding a rule:

1. **Update README.md** with a link and brief description
2. **Use descriptive file names** (e.g., `enforce-branding.md`, `validate-typescript.md`)
3. **Include inline comments** if your rule has complex logic
4. **Add examples** showing both compliant and non-compliant code

## Categories and Structure

Rules are organized into categories

**Adding a New Category:**
1. Create folder: `rules/new-category/`
2. Add `_category.yml` file with metadata (see existing examples)
3. Update this guide and README.md


## Code of Conduct

This project follows the Gitar Community Code of Conduct. By participating, you agree to:

- Be respectful and inclusive
- Provide constructive feedback
- Focus on what's best for the community
- Show empathy towards others

## Questions?

- Check the [Gitar Documentation](https://docs.gitar.ai)
- Ask in the [Gitar Community](https://go.gitar.ai/community)

---

Thank you for contributing to Gitar Rules! Your efforts help teams automate CI workflows and ship code faster.
