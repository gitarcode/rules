# Contributing to Gitar Rules

We welcome contributions from the community! Whether you're sharing a rule you've been using in production or creating a new example to help others, your contributions help make CI automation more accessible.

## How to Contribute

### 1. Share Your Rule

If you have a Gitar rule that works well for your team, consider sharing it:

1. **Fork this repository**
2. **Add your rule** to the `examples/` directory
3. **Update the README** to list your rule under the appropriate category
4. **Submit a pull request** with a clear description of what your rule does

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

Every rule should follow this general structure:

```markdown
# Rule Title

Brief 1-2 sentence description of what this rule does and why it's useful.

## When to Apply This Rule

Clear list of conditions that trigger this rule:
- Specific file patterns or paths
- Types of changes (new files, modifications, deletions)
- Content patterns to detect

## Required Standards / Expected Behavior

Detailed explanation of what Gitar should check for:
- Specific patterns to match
- Code examples showing correct vs. incorrect
- Any validation logic

## Actions / Enforcement

What Gitar should do when conditions are met:
1. Check for compliance
2. Post comments with specific guidance
3. Apply labels
4. Assign reviewers
5. Suggest code changes

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

## Categories

When adding your rule to the README, place it in one of these categories:

- **Code Quality & Consistency** - Linting, formatting, standards enforcement
- **Documentation** - README updates, API doc sync, changelog requirements
- **Dependencies** - Package upgrades, vulnerability checks, license compliance
- **Reviews & Collaboration** - Reviewer assignment, PR size checks, approval requirements
- **Automation & Workflows** - CI checks, deployment preparation, release management
- **Security & Compliance** - Secret detection, access control, audit logging

Don't see a category that fits? Suggest a new one in your PR.

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
