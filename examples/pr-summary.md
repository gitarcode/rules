# PR Summary Enhancement

Automatically enhance pull request descriptions with a concise AI-generated summary of changes when new commits are pushed.

## When to Apply This Rule

Apply this rule when:
- New commits are pushed to an existing pull request
- Changes significantly alter the PR's scope or purpose
- Meaningful functionality, bug fixes, or architectural changes are introduced

Do NOT apply this rule for:
- Minor formatting changes or typo fixes
- Small comment updates
- Trivial refactors that don't change functionality

## Required Standards

### Analysis Approach

When analyzing PR changes, follow this systematic approach:

1. **Review Git Context**:
   - Examine commit history since PR creation or last significant update
   - Identify change types: new features, bug fixes, refactoring, documentation, tests, dependency updates
   - Note which codebase areas are affected
   - For dependency/platform upgrades (React, Node, Rust, Spring, etc.), search for official release notes and reference them

2. **Assess Significance**:
   - Only generate summary for meaningful changes
   - Skip: formatting, typos, minor comments, small refactors
   - Include: new functionality, bug fixes, architectural changes, significant refactors, dependency upgrades

3. **Generate Summary Content**:
   - Keep concise: 2-4 bullet points maximum
   - Focus on facts and technical details not in the user's description
   - Avoid repeating information from the main PR description
   - Include relevant links to documentation or release notes
   - Use bullet points for clarity

### Output Format

The summary section should use this exact format:

```markdown
---

## Summary by Gitar

- [Concise factual point about changes]
- [Additional technical context if relevant]
- [Links to release notes or documentation if applicable]
```

## Actions

When this rule triggers:

1. **Check for existing summary**: Look for a "Summary by Gitar" section in the PR description
2. **Analyze changes**: Review commits and determine if an update is warranted
3. **Update or create section**:
   - If section exists and is accurate, skip update
   - If section exists but needs updates, replace it with new content
   - If section doesn't exist, append it to the end of PR description
4. **Preserve user content**: Never modify the user's existing PR description, only append or update the "Summary by Gitar" section

## Why This Matters

Automated PR summaries provide several benefits:

- **Improved Code Review Efficiency** - Reviewers get a quick technical overview without reading every commit
- **Better Documentation** - Creates a clear record of what changed and why
- **Reduced Context Switching** - Technical details and release notes are surfaced automatically
- **Consistency** - All PRs follow a similar documentation pattern
- **Time Savings** - Authors don't need to manually update descriptions after each push

The summary complements the author's description by focusing on technical implementation details, affected areas, and relevant external documentation - providing reviewers with comprehensive context for their review.
