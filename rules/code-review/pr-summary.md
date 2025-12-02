---
title: "PR Summary Enhancement"
description: "Enhance pull request descriptions with technical summaries based on commits and code changes"
slug: "pr_summary"
when: "New PRs are created or PRs are updated with new commits (when descriptions are empty, minimal, template-only, or implementation diverges from description)"
actions: "Append 'Summary by Gitar' section with 2-5 technical bullet points to PR description"
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

   **Techniques for artifact extraction from diffs:**

   - **Scan for new files**: Look for `+++ b/path/to/file` in diffs or `git diff --name-status` showing `A` (added)
     - Prioritize new source files over test files
     - Group related files (e.g., component + styles + types)

   - **Identify new exports**: Search diffs for patterns like:
     - `export function functionName` / `export const variableName`
     - `export class ClassName` / `export interface InterfaceName`
     - `export default` for default exports
     - Note: Only highlight genuinely reusable/public exports, not internal helpers

   - **Find new API endpoints**: Look for:
     - Route definitions: `app.get('/api/path')`, `@GetMapping`, `Route::get()`
     - Controller methods with route decorators
     - OpenAPI/Swagger annotations

   - **Detect configuration changes**: Search for:
     - New environment variables in `.env.example` or config files
     - New feature flags or toggle definitions
     - Database migrations creating new tables/columns
     - New dependencies in `package.json`, `Cargo.toml`, `pom.xml`, etc.

   - **Spot breaking changes**: Watch for:
     - Removed or renamed public functions/classes
     - Changed function signatures (parameters added/removed/reordered)
     - Deleted files that might be imported elsewhere
     - Modified API contracts or response shapes

5. **Extract and prioritize new artifacts**:

   Before writing the summary, explicitly identify:

   **High-priority artifacts (deserve top-level bullets):**
   - New files created (utilities, services, components, configs)
   - New public functions/classes/hooks (especially reusable helpers)
   - New API endpoints or integrations
   - New environment variables, feature flags, or configuration options
   - New user-facing features or capabilities

   **Implementation details (nest as sub-bullets):**
   - Circular dependency resolutions
   - Internal refactoring mechanics
   - Test file reorganization (unless testing strategy changed)
   - Code style or formatting changes
   - File moves without functional changes

   **Categorization priority (organize bullets by impact):**
   1. **New features** - What users can now do
   2. **New utilities/APIs** - Reusable developer-facing additions
   3. **Architecture changes** - Refactoring, modularization, patterns
   4. **Bug fixes** - What was broken and now works
   5. **Performance/security** - Optimizations, vulnerability fixes
   6. **Infrastructure** - Build, deploy, monitoring, dependencies

6. **Select changes using priority scoring**:

   **CRITICAL: Score and filter ruthlessly**

   Before writing, score every change (1-10):
   - **9-10**: New user-facing features, critical bug fixes, breaking changes
   - **8-9**: New public APIs/helpers, significant refactors, integrations
   - **7-8**: Minor features, config changes, infrastructure improvements
   - **6 and below**: Implementation details, test reorgs, file moves - **SKIP THESE**

   **Only include changes scoring 7 or higher**

7. **Apply adaptive category limits based on PR size**:

   **Flexible Category Count** (adapts to PR complexity):

   | PR Size | Files Changed | Category Count | Word Target |
   |---------|--------------|----------------|-------------|
   | Small | <5 files | 2-3 categories | 30-50 words |
   | Medium | 5-15 files | 3-4 categories | 50-80 words |
   | Large | 15+ files | 4-5 categories | 80-150 words |

   **Hard constraints:**
   - **Minimum**: 2 categories (only if PR truly has <3 important changes)
   - **Maximum**: 5 categories (hard stop, even if more high-priority items exist)
   - **Word cap**: 150 words maximum regardless of PR size

   **Selection process:**
   1. Count files changed in PR
   2. Determine PR size category (small/medium/large)
   3. Filter to changes scoring 7+
   4. Select top N changes that fit in the category range for that PR size
   5. If filtered changes exceed max categories, take highest-scoring ones only

8. **Use stable category patterns**:

   **Category Patterns** (choose based on change type):
   - `New [artifact]:` - New authentication service, New `parseUserInput` helper
   - `[Component] refactor:` - Database layer refactor, API client refactor
   - `Fixed [problem]:` - Fixed memory leak, Fixed race condition
   - `[Service] integration:` - Redis caching integration, Stripe payments integration
   - `Updated [dependency]:` - Updated Node 18 → 20, Updated React 18.2 → 18.3
   - `[System] configuration:` - CI/CD pipeline configuration, Docker containerization
   - `Performance optimization:` / `Security enhancement:` - For perf/security improvements

   **Format rules:**
   - **Top-level bullets**: Category label ONLY ending with `:` (no data, no details)
   - **Sub-bullets**: ALL context and artifacts in `code format` (1-2 per category max)
   - Use specific artifact names (not "Updated code structure")
   - Focus on purpose, not mechanics
   - Each sub-bullet one concise sentence

9. **Generate summary** with hierarchical structure:

   ```markdown
   ---
   ## Summary by Gitar

   - **[Stable pattern category]:**
     - Context with artifact in `code format` (one concise sentence)
     - Additional detail if needed (one concise sentence)
   - **[Another category]:**
     - Context with artifact in `code format`

   <sub>This will update automatically on new commits.</sub>
   ```

   **Format requirements:**
   - **Top-level**: Category label ONLY ending with `:` (e.g., `**Authentication refactor:**`)
   - **Sub-bullets**: 1-2 per category with ALL context and artifacts in `code format`
   - **Bold** for categories, `code format` for files/functions/APIs
   - Each sub-bullet: 10-20 words, one sentence
   - Total word count: Stay within target range for PR size (max 150 words)
   - **Footer**: Always end with `<sub>This will update automatically on new commits.</sub>`

   **Example (Medium PR - 3 categories, 45 words):**
   ```markdown
   - **Authentication refactor:**
     - Split `auth/AuthService.ts` (800 lines) into 6 modules under `auth/services/*`
   - **New helper:**
     - `validateUserPermissions` in `auth/utils/permissions.ts` centralizes RBAC logic
   - **Fixed memory leak:**
     - Corrected token refresh timing in `TokenManager.ts:145`

   <sub>This will update automatically on new commits.</sub>
   ```

   **Anti-patterns:**
   ```markdown
   ❌ Data in top-level: "**New `validateUserPermissions` helper** in auth/utils/permissions.ts"
   ❌ Unstable/dynamic categories: "Enhanced permission checking with new helper"
   ❌ Too many categories: 6+ categories on a small PR
   ❌ Exceeding word limit: 200+ words
   ```

10. **Update PR description**:
   - If section exists and is accurate → skip update
   - If section exists but needs updates → replace with new content
   - If section doesn't exist → append to end
   - Preserve existing user content

   **Formatting requirements when appending:**
   - Add a blank line before the opening `---` separator
   - Add a blank line after the closing bullet list, before the closing `---` separator
   - This ensures proper markdown rendering and clean separation from other content

   **Example of correct spacing:**
   ```markdown
   [Existing PR description content]

   ---

   ## Summary by Gitar

   - **First bullet point**
     - Sub-detail
   - **Second bullet point**
     - Sub-detail

   <sub>This will update automatically on new commits.</sub>

   ---
   [Any additional content like preview links, Jira links, etc.]
   ```

## Examples by PR Type

### Refactoring
```markdown
- **Data utilities refactor:**
  - Split `utils/dataProcessing.ts` (1200 lines) into 47 focused modules
  - Added barrel export for backward compatibility
- **New helper:**
  - `buildQueryStringFromFilters` in `utils/dataProcessing/queryString.ts` constructs backend query strings from filter objects

<sub>This will update automatically on new commits.</sub>
```

### Feature Addition
```markdown
- **OAuth integration:**
  - Google and GitHub providers with PKCE flow and account linking
- **New service:**
  - `OAuthManager` in `auth/services/OAuthManager.ts` handles provider registration, token exchange, and refresh
- **New API endpoints:**
  - `POST /api/auth/oauth/initiate`, `GET /api/auth/oauth/callback`, `POST /api/auth/oauth/link`

<sub>This will update automatically on new commits.</sub>
```

### Bug Fix
```markdown
- **Fixed memory leak:**
  - Added `useEffect` cleanup in `hooks/useWebSocket.ts:67` to prevent browser crashes
- **New lifecycle tracking:**
  - `activeConnections` Map in `WebSocketManager.ts` tracks open connections

<sub>This will update automatically on new commits.</sub>
```

### Dependency Upgrade
```markdown
- **Updated React:**
  - 18.2 → 18.3 ([release notes](https://react.dev/blog/2024/04/25/react-19)) with improved concurrent rendering and new `use` Hook
- **Fixed breaking changes:**
  - Replaced deprecated `ReactDOM.render` with `createRoot` in 3 test files

<sub>This will update automatically on new commits.</sub>
```

## Quick Reference

**Before writing, verify:**
1. **Scored changes** - Only include items scoring 7+
2. **Category count** - Matches PR size (Small: 2-3, Medium: 3-4, Large: 4-5)
3. **Word count** - Within target range, max 150 words
4. **Format** - Top-level = category with `:`, sub-bullets = all data

**Format checklist:**
- [ ] Top-level bullets are ONLY stable category labels ending with `:`
- [ ] ALL context/data in sub-bullets (1-2 per category)
- [ ] Specific artifacts in `code format`
- [ ] Each sub-bullet 10-20 words, one sentence
- [ ] Total ≤150 words
- [ ] Ends with `<sub>This will update automatically on new commits.</sub>`

**Avoid:**
- Data in top-level bullets or missing `:`
- Unstable/dynamic categories
- Exceeding category limits (>5 categories)
- Exceeding word limit (>150 words)

## Why This Matters

- **Improved Code Review Efficiency** - Reviewers get a quick technical overview without reading every commit
- **Better Documentation** - Creates a clear record of what changed and why
- **Consistency** - All PRs follow a similar documentation pattern
- **Time Savings** - Authors don't need to manually update descriptions after each push
- **Scannability** - Hierarchical structure allows quick understanding at-a-glance
