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

6. **Generate summary** using hierarchical format:

   ```markdown
   ---

   ## Summary by Gitar

   - **[Main change or category]**: Description with `artifact-name` in code format
     - Technical detail about implementation approach
     - Impact, motivation, or why this matters
     - Related changes or files affected
   - **[New utility/function]**: `createHelperFunction` in `path/to/file.ts`
     - What it does and problem it solves
     - Where it's used or who benefits
   - **[Another top-level change]**
     - Sub-detail 1
     - Sub-detail 2

   ```

   **Hierarchical formatting rules:**

   - **Top-level bullets**: Scannable headlines about the main changes
     - Start with **bold category or main change description**
     - Include artifact names in `code format` (files, functions, classes)
     - Keep to one sentence summarizing the "what"

   - **Sub-bullets (indented 2 spaces)**: Rich technical context
     - Implementation details and approach
     - Motivation, impact, or benefits
     - Related changes or affected areas
     - Keep each sub-bullet to 1-2 sentences max

   - **Formatting conventions:**
     - Use **bold** for: change categories, key concepts, important terms
     - Use `code formatting` for: artifact names, file paths, technical values, API names
     - Front-load important information in each bullet
     - Nest 2-3 sub-bullets per top-level item (adjust based on complexity)

   **Example of good hierarchical structure:**
   ```markdown
   - **Refactored authentication module** into focused services
     - Split monolithic `auth/AuthService.ts` (800 lines) into 6 modules under `auth/services/*`
     - Separated concerns: token management, session handling, OAuth flows
     - Added barrel export in `auth/index.ts` for backward compatibility

   - **Added `validateUserPermissions` helper** in `auth/utils/permissions.ts`
     - Centralized role-based access control (RBAC) logic
     - Replaces duplicated permission checks across 12 components
     - Supports hierarchical role inheritance

   - **Fixed session timeout bug** causing premature logouts
     - Corrected token refresh timing in `TokenManager.ts:145`
     - Added retry logic for failed refresh attempts
     - Resolved race condition between refresh and API calls
   ```

   **Anti-patterns to avoid:**
   ```markdown
   ❌ Flat structure without hierarchy:
   - Refactored auth module by splitting AuthService.ts
   - Added validateUserPermissions helper
   - Fixed session timeout bug
   - Corrected token refresh timing
   - Added retry logic

   ❌ Highlighting byproducts as main points:
   - Resolved circular dependency between AuthService and TokenManager
   - Refactored authentication module into focused services

   ❌ Burying new artifacts in narrative:
   - Refactored authentication module, which involved creating a new validateUserPermissions helper to centralize RBAC logic
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

### Large Refactoring PR

**Scenario**: Splitting a 1200-line utility file into 47 modular files

**Good summary structure:**
```markdown
## Summary by Gitar

- **Refactored monolithic `utils/dataProcessing.ts`** into 47 modular files
  - Organized utilities into focused modules under `utils/dataProcessing/*` (constants, parsing, validation, types)
  - Each module has single responsibility and clear boundaries
  - Added barrel export in `utils/dataProcessing.ts` to maintain backward compatibility with existing imports

- **Introduced `buildQueryStringFromFilters` helper** in `utils/dataProcessing/queryString.ts`
  - Constructs backend query strings from filter objects
  - Simplifies API integration in `DataAPI.tsx` and `FilterPanel.tsx`
  - Eliminates manual query parameter construction

- **Resolved circular dependencies** between processing modules
  - Consolidated related functions in `FilterLogic.ts`, `ParamBuilder.ts`, and `FilterFactory.ts`
  - Updated import paths in 15 test files to use new module structure
```

**Why this works:**
- Top-level bullets are scannable: "refactoring", "new helper", "fixed dependencies"
- New utility `buildQueryStringFromFilters` is highlighted as distinct item, not buried
- Circular dependency fix is acknowledged but nested as implementation detail
- Each bullet explains what, why, and impact

### Feature Addition PR

**Scenario**: Adding OAuth authentication

**Good summary structure:**
```markdown
## Summary by Gitar

- **Added OAuth 2.0 authentication** for Google and GitHub providers
  - Users can now sign in using external accounts instead of email/password
  - Implemented PKCE flow for enhanced security
  - Added account linking for users with existing credentials

- **Created `OAuthManager` service** in `auth/services/OAuthManager.ts`
  - Handles provider registration, token exchange, and refresh logic
  - Supports extensible provider configuration via `auth/config/providers.ts`
  - Manages state/nonce generation and validation

- **Added new OAuth routes** to API
  - `POST /api/auth/oauth/initiate` - Starts OAuth flow
  - `GET /api/auth/oauth/callback` - Handles provider redirects
  - `POST /api/auth/oauth/link` - Links OAuth account to existing user

- **Updated UI with social login buttons** in login and signup flows
  - Added `SocialLoginButton` component with provider branding
  - Responsive design for mobile and desktop
  - Loading states and error handling for failed auth attempts
```

**Why this works:**
- Leads with user-facing feature (OAuth authentication)
- New service and routes are distinct bullets (artifacts)
- UI changes are separate from backend implementation
- Clear what was added and what value it provides

### Bug Fix PR

**Scenario**: Fixing memory leak in WebSocket connection

**Good summary structure:**
```markdown
## Summary by Gitar

- **Fixed memory leak in WebSocket connection handling**
  - Resolved issue where connections weren't properly cleaned up on unmount
  - Added `useEffect` cleanup in `hooks/useWebSocket.ts:67` to close connections
  - Prevents browser tab crashes after ~50 page navigations

- **Added connection lifecycle tracking** in `WebSocketManager.ts`
  - New `activeConnections` Map to track all open connections
  - Implements `cleanupStaleConnections()` method called on visibility change
  - Logs connection count for monitoring and debugging

- **Improved error handling** for connection failures
  - Added exponential backoff for reconnection attempts (max 5 retries)
  - Surfaces connection errors to UI via `ConnectionStatus` component
  - Graceful degradation when WebSocket unavailable
```

**Why this works:**
- Leads with the bug and fix (what was broken, what's fixed)
- New tracking mechanism is separate bullet (new artifact)
- Implementation details are nested under main points
- Shows impact (prevents crashes) and where fix was applied

### Dependency Upgrade PR

**Scenario**: Upgrading React and related libraries

**Good summary structure:**
```markdown
## Summary by Gitar

- **Upgraded React from 18.2 to 18.3** ([release notes](https://react.dev/blog/2024/04/25/react-19))
  - Improved concurrent rendering performance
  - New `use` Hook for reading resources in components
  - Better TypeScript support for refs and context

- **Updated related dependencies** for compatibility
  - `react-dom`: 18.2 → 18.3
  - `@types/react`: 18.2.45 → 18.3.1
  - `@testing-library/react`: 14.0 → 14.3

- **Fixed breaking changes** in component tests
  - Updated `waitFor` assertions to use new async rendering behavior
  - Replaced deprecated `ReactDOM.render` with `createRoot` in 3 test files
  - Fixed type errors in `useContext` calls with stricter type checking
```

**Why this works:**
- Main upgrade is top-level with link to release notes
- Related dependencies are grouped together
- Breaking changes and fixes are clearly called out
- Shows what needed adjustment and why

## Quick Reference: Summary Checklist

When generating a PR summary, verify:

**✅ Structure**
- [ ] Top-level bullets are scannable headlines (bold category + artifact name in code format)
- [ ] Sub-bullets provide rich technical context (2-3 per main item)
- [ ] Information is hierarchical, not flat
- [ ] Front-loaded: Most important info comes first in each bullet

**✅ Content Prioritization**
- [ ] New artifacts (files, functions, APIs) are highlighted as distinct top-level bullets
- [ ] Implementation details (circular deps, test org) are nested or omitted
- [ ] Changes are grouped by impact: Features → Utilities → Architecture → Fixes → Infrastructure
- [ ] Byproducts and refactoring mechanics are de-emphasized

**✅ Clarity**
- [ ] Each bullet is 1-2 sentences max
- [ ] Technical terms use `code formatting`
- [ ] Key concepts use **bold formatting**
- [ ] File paths and artifact names are precise (`path/to/file.ts:123`)

**✅ Value**
- [ ] Summary adds information not obvious from PR title
- [ ] Explains "what" and "why", not just "how"
- [ ] Links to relevant docs/release notes where applicable
- [ ] Reviewers can understand changes without reading all commits

**❌ Anti-patterns to Avoid**
- [ ] Flat bullet lists without hierarchy
- [ ] Highlighting byproducts (circular dependencies) as main points
- [ ] Burying new utilities in refactoring narratives
- [ ] Generic statements without specific artifact names
- [ ] Over-explaining trivial changes

## Why This Matters

- **Improved Code Review Efficiency** - Reviewers get a quick technical overview without reading every commit
- **Better Documentation** - Creates a clear record of what changed and why
- **Consistency** - All PRs follow a similar documentation pattern
- **Time Savings** - Authors don't need to manually update descriptions after each push
- **Scannability** - Hierarchical structure allows quick understanding at-a-glance
