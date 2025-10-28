# Rule Quality Standards

Ensure all new or modified rule files in the `examples/` directory follow the established conventions and structure outlined in CONTRIBUTING.md.

## When to Apply This Rule

Apply this rule when:
- New `.md` files are added to the `examples/` directory
- Existing rule files in `examples/` are modified with structural changes
- Any file matching `examples/*.md` pattern is created or updated

## Required Standards

All rule files must follow this exact structure as defined in CONTRIBUTING.md:

### 1. Title and Description (Required)

```markdown
# Rule Title

Brief 1-2 sentence description of what this rule does and why it's useful.
```

**Validation**:
- Must have a single H1 heading at the top
- Must have a brief description (1-2 sentences) immediately after the title
- Description should explain both "what" and "why"

### 2. When to Apply This Rule (Required)

```markdown
## When to Apply This Rule

Clear list of conditions that trigger this rule:
- Specific file patterns or paths
- Types of changes (new files, modifications, deletions)
- Content patterns to detect
```

**Validation**:
- Must have an H2 section titled "When to Apply This Rule"
- Should contain a bulleted list of specific, actionable conditions
- Should include file patterns using glob syntax (e.g., `*.tsx`, `package.json`)
- Should specify what types of changes trigger the rule

### 3. Required Standards / Expected Behavior (Required)

```markdown
## Required Standards
OR
## Expected Behavior

Detailed explanation of what Gitar should check for:
- Specific patterns to match
- Code examples showing correct vs. incorrect
- Any validation logic
```

**Validation**:
- Must have an H2 section titled "Required Standards", "Expected Behavior", or similar
- Should provide detailed, specific criteria for validation
- Should include code examples when relevant
- Should be actionable and unambiguous

### 4. Actions / Enforcement (Required)

```markdown
## Actions
OR
## Enforcement

What Gitar should do when conditions are met:
1. Check for compliance
2. Post comments with specific guidance
3. Apply labels
4. Assign reviewers
5. Suggest code changes
```

**Validation**:
- Must have an H2 section titled "Actions", "Enforcement", or similar
- Should use numbered or bulleted lists
- Should clearly describe what Gitar will do (not just what it will check)
- Should be written in active voice

### 5. Why This Matters (Required)

```markdown
## Why This Matters

Brief explanation of the value:
- What problems does this prevent?
- What benefits does it provide?
- What workflows does it improve?
```

**Validation**:
- Must have an H2 section titled "Why This Matters"
- Should explain the business or technical value
- Should help authors understand the reasoning behind the rule
- Typically 3-5 bullet points or 2-3 short paragraphs

## Actions

When a new or modified rule file is detected in `examples/`:

1. **Validate structure**: Check that the file contains all five required sections in the correct order:
   - Title (H1) with description
   - "When to Apply This Rule" (H2)
   - "Required Standards" or equivalent (H2)
   - "Actions" or equivalent (H2)
   - "Why This Matters" (H2)

2. **Check content quality**:
   - Verify the title description is concise (1-2 sentences)
   - Ensure "When to Apply" includes specific file patterns
   - Confirm "Actions" uses active voice and describes behavior
   - Validate "Why This Matters" explains value

3. **Add inline comments with suggestions** for any issues found:
   - Reference this rule file (`examples/rule-quality.md`)
   - Reference CONTRIBUTING.md for detailed guidelines
   - Provide a code suggestion showing the correct structure
   - Explain which section is missing or needs improvement
   - Include examples from existing well-structured rules (e.g., `brand.md`, `pr-summary.md`)

4. **Suggest specific improvements**:
   - If a section is missing, suggest adding it with a template
   - If a section exists but lacks detail, suggest specific content to add
   - If file patterns are vague, suggest specific glob patterns
   - If actions are passive, suggest active voice alternatives

5. **Approve if compliant**: If the rule file follows all conventions, no action is needed

## Example Issues and Suggestions

### Missing "Why This Matters" Section

```markdown
**Issue**: This rule is missing the "Why This Matters" section

**Suggestion**: Add this section to help users understand the value:

## Why This Matters

- [Explain what problems this prevents]
- [Describe benefits it provides]
- [Note which workflows it improves]

See CONTRIBUTING.md for more details on this section.
```

### Vague "When to Apply" Conditions

```markdown
**Issue**: The conditions are too vague. Use specific file patterns.

**Current**:
- When HTML files change

**Suggested**:
- New HTML files (`*.html`, `index.html`)
- Modifications to existing HTML files in `apps/*/public/`
- Changes to web manifest files (`site.webmanifest`, `manifest.json`)
```

### Passive Voice in Actions Section

```markdown
**Issue**: Actions should be written in active voice describing what Gitar will do.

**Current**:
Files should be checked for compliance

**Suggested**:
1. Check all matching files for compliance with standards
2. Post inline comments on non-compliant lines with correction suggestions
3. Apply "needs-review" label if violations are found
```

## Why This Matters

Consistent rule structure provides several benefits:

- **Easier to Use** - Users can quickly scan any rule and understand its purpose, triggers, and actions
- **Better AI Understanding** - Gitar can more effectively parse and execute rules with predictable structure
- **Maintainability** - Consistent format makes rules easier to update and debug
- **Onboarding** - New contributors can learn the pattern by example and create quality rules faster
- **Documentation** - Well-structured rules serve as both automation and documentation
- **Community Growth** - Clear standards encourage more contributions and reduce review friction

By enforcing these standards, we ensure every rule in the repository is clear, actionable, and valuable to the community.
