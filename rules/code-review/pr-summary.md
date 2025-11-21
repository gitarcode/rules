---
title: "PR Summary Enhancement"
description: "Enhance pull request descriptions with technical summaries based on commits and code changes"
slug: "pr_summary"
when: "New PRs are created or PRs are updated with new commits (when descriptions are empty, minimal, template-only, or implementation diverges from description)"
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

6. **Apply stable category patterns**:

   **Category Patterns** (choose based on change type):
   - `New [artifact]` - New authentication service, New `parseUserInput` helper
   - `[Component] refactor` - Database layer refactor, API client refactor
   - `Fixed [problem]` - Fixed memory leak, Fixed race condition
   - `[Service] integration` - Redis caching integration, Stripe payments integration
   - `Updated [dependency]` - Updated Node 18 → 20, Updated React 18.2 → 18.3
   - `[System] configuration` - CI/CD pipeline configuration, Docker containerization
   - `Performance optimization` / `Security enhancement` - For perf/security improvements

   **Principles:**
   - Top-level bullets are ONLY category/pattern labels (no data)
   - ALL context and details go in sub-bullets (1-2 per category)
   - Use specific artifact names in sub-bullets (not "Updated code structure")
   - Focus on purpose, not mechanics
   - Allow flexibility for novel changes or team-specific categories

7. **Generate summary** with hierarchical structure:

   ```markdown
   ---
   ## Summary by Gitar

   - **[Stable pattern category]**
     - Key context with artifact in `code format`
     - Additional detail if needed
   - **New helper**
     - `functionName` in `path/to/file.ts` does X and solves Y
   ```

   **Format:**
   - **Top-level**: Category/pattern label ONLY ending with `:` (e.g., "**Authentication refactor:**", "**New helper:**")
   - **Sub-bullets**: 1-2 with ALL context and artifacts in `code format`
   - Use **bold** for categories, `code format` for files/functions/APIs
   - Keep sub-bullets 1-2 sentences max

   **Example:**
   ```markdown
   - **Authentication module refactor:**
     - Split `auth/AuthService.ts` (800 lines) into 6 modules under `auth/services/*`
     - Added barrel export in `auth/index.ts` for backward compatibility

   - **New helper:**
     - `validateUserPermissions` in `auth/utils/permissions.ts` centralizes RBAC logic

   - **Fixed memory leak:**
     - Corrected token refresh timing in `TokenManager.ts:145`
   ```

   **Anti-patterns:**
   ```markdown
   ❌ Data in top-level: "**New `validateUserPermissions` helper** in auth/utils/permissions.ts"
   ❌ Unstable categories: "Enhanced permission checking with new helper"
   ❌ Buried artifacts: "Auth refactor involving new validateUserPermissions helper"
   ```

7. **Update PR description**:
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
```

### Feature Addition
```markdown
- **OAuth integration:**
  - Google and GitHub providers with PKCE flow and account linking
- **New service:**
  - `OAuthManager` in `auth/services/OAuthManager.ts` handles provider registration, token exchange, and refresh
- **New API endpoints:**
  - `POST /api/auth/oauth/initiate`, `GET /api/auth/oauth/callback`, `POST /api/auth/oauth/link`
```

### Bug Fix
```markdown
- **Fixed memory leak:**
  - Added `useEffect` cleanup in `hooks/useWebSocket.ts:67` to prevent browser crashes
- **New lifecycle tracking:**
  - `activeConnections` Map in `WebSocketManager.ts` tracks open connections
```

### Dependency Upgrade
```markdown
- **Updated React:**
  - 18.2 → 18.3 ([release notes](https://react.dev/blog/2024/04/25/react-19)) with improved concurrent rendering and new `use` Hook
- **Fixed breaking changes:**
  - Replaced deprecated `ReactDOM.render` with `createRoot` in 3 test files
```

## Quick Reference

**Verify:**
- Top-level bullets are ONLY category labels ending with `:` (stable patterns)
- ALL data/context in sub-bullets (1-2 per category)
- Specific artifact names in sub-bullets with `code format`
- New artifacts as distinct categories

**Avoid:**
- Data in top-level bullets or missing `:`
- Unstable/dynamic categories
- Buried artifacts, padding sub-bullets

## Why This Matters

- **Improved Code Review Efficiency** - Reviewers get a quick technical overview without reading every commit
- **Better Documentation** - Creates a clear record of what changed and why
- **Consistency** - All PRs follow a similar documentation pattern
- **Time Savings** - Authors don't need to manually update descriptions after each push
- **Scannability** - Hierarchical structure allows quick understanding at-a-glance
