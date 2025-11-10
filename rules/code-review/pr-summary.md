---
title: "PR Summary Enhancement"
description: "Enhance pull request descriptions with technical summaries based on commits and code changes"
slug: "pr_summary"
when: "PRs with empty, minimal, template-only descriptions, or when implementation significantly diverges from existing description"
actions: "Append 'Summary by Gitar' section with 2-5 technical bullet points to PR description"
integrations: "github"
---

# PR Summary Enhancement

Automatically enhance pull request descriptions with concise technical summaries of changes.

## When to Use This

### Priority 1: Exclusions (Override Everything)

**NEVER apply when:**
- Changes are purely formatting (prettier, linting, whitespace)
- Changes are only typo/spelling fixes
- Changes are trivial comment updates
- Changes are small refactors that don't change functionality

### Priority 2: Description Quality Triggers

**ALWAYS apply when:**
- PR description is empty, null, or only whitespace/HTML comments
- Description contains unfilled template placeholders (e.g., `[TODO]`, `[Add description]`, `[feature/bug/etc]`)
- Description ≤ 20 characters and adds no meaningful context beyond the title
- Description semantically matches the PR title (exact match or paraphrase without added detail)

**Apply when:**
- New pull request with minimal description (21-99 characters)
- Description ≥ 100 characters but lacks technical depth (only restates commits or is vague)
- Meaningful functionality, bug fixes, or architectural changes are introduced
- Implementation or commits diverge significantly from existing description (e.g., description says "add login" but commits show OAuth, 2FA, SAML integration)

**Do NOT apply for:**
- PRs with comprehensive descriptions (≥ 100 chars with clear what/why/how/impact)
- Cases where summary would add no unique value

## How It Works

1. **Check exclusions first** - Verify PR doesn't match Priority 1 exclusions (formatting, typos, trivial changes). If it does, STOP and skip.

2. **Detect description quality and apply appropriate mode**:

   **COMPREHENSIVE MODE** (Empty/Template - Provide 3-5 bullets):
   - Triggers: Description is null, empty, only whitespace/HTML comments, unfilled template placeholders, ≤20 chars, or semantically matches title
   - Strategy: Fill the documentation void completely
   - Cover: What changed (technical specifics), Why (motivation), How (implementation), Impact (what this enables/fixes)
   - Don't worry about repetition since there's nothing to repeat

   Examples:
   - Title: "Add authentication" / Description: "adds auth" → COMPREHENSIVE MODE
   - Title: "Fix memory leak" / Description: "" → COMPREHENSIVE MODE

   **SUPPLEMENTAL MODE** (Minimal - Provide 2-4 bullets):
   - Triggers: Description 21-99 characters OR ≥100 chars but lacks depth (restates commits, no context/motivation)
   - Strategy: Fill gaps and add missing technical details
   - Focus: Technical specifics not mentioned, context/motivation if missing, implementation approach
   - Complement thin content without full duplication

   Examples:
   - "Adds login feature with JWT" (27 chars) → SUPPLEMENTAL MODE
   - "Updates React 18.2→18.3, Next.js 14.0→14.1, TypeScript 5.3→5.4, and other deps" (150 chars but shallow) → SUPPLEMENTAL MODE

   **SELECTIVE MODE** (Substantial - Provide 0-3 bullets or skip):
   - Triggers: Description ≥ 100 characters with clear technical depth (explains what/why/how or provides architecture/impact details)
   - Strategy: Skip unless you can add genuinely unique value
   - Focus: Only add external resources (release notes/docs), non-obvious cross-cutting concerns, or security/performance implications not mentioned
   - Skip if description already covers everything well

   Examples:
   - Description with architecture, motivation, and testing info → SELECTIVE MODE (skip)
   - Security bug fix with technical details but missing severity context → SELECTIVE MODE (1-2 bullets on risk/scope)

3. **Handle mixed change types**:
   - If both significant changes AND excluded types (e.g., feature + formatting): Focus summary on significant changes only
   - If ONLY excluded types: Skip entirely per Priority 1

4. **Analyze commits and check for divergence**:
   - Identify change types: features, bug fixes, refactoring, tests, dependency updates
   - Note affected codebase areas
   - Compare commit scope/complexity against existing description - if commits show significantly more features, complexity, or different approach than described, apply SUPPLEMENTAL or COMPREHENSIVE mode
   - For dependency/platform upgrades (React, Node, Rust, Spring), search for official release notes

5. **Generate summary** in this format:
   ```markdown
   ---

   ## Summary by Gitar

   - [Concise factual point about changes]
   - [Additional technical context if relevant]
   - [Links to release notes or documentation if applicable]
   ```

   **Formatting guidelines for readability:**
   - Use **bold** for key concepts, mode names, or important terms (e.g., **COMPREHENSIVE MODE**, **Priority 1**, **breaking change**)
   - Use `code formatting` for technical terms, thresholds, or specific values (e.g., `≤20 chars`, `OAuth 2.0`, `Redis cluster`)
   - Keep each bullet to 1-2 sentences maximum
   - Front-load the most important information in each bullet

6. **Update PR description**:
   - If section exists and is accurate → skip update
   - If section exists but needs updates → replace with new content
   - If section doesn't exist → append to end
   - Preserve existing user content

## Why This Matters

- **Improved Code Review Efficiency** - Reviewers get a quick technical overview without reading every commit
- **Better Documentation** - Creates a clear record of what changed and why
- **Consistency** - All PRs follow a similar documentation pattern
- **Time Savings** - Authors don't need to manually update descriptions after each push
