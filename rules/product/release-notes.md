---
title: "Release Notes"
description: "Automatically suggest and update release notes when a release is created or updated"
slug: "release_notes"
when: "PR/MRs targeting the default branch with 'release' keyword in title or changes to release notes files"
actions: "Analyze changes since last release and suggest user-facing updates to release notes"
---

# Release Notes

Automatically analyze changes and suggest release notes content for releases, focusing on user-facing improvements and major changes.

## When to Use This

Apply when:
- Title contains keywords: `release`, `version`, `v1.2.3`, `changelog`
- Modifies release notes files: `CHANGELOG.md`, `CHANGELOG.mdx`, `RELEASE_NOTES.md`, `docs/releases/*.md`, `docs/releases/*.mdx`
- Targeting the default branch or `release/*` branches
- Description indicates this is a release preparation

Do NOT apply for:
- Regular feature changes not related to releases
- Hotfix or patch releases (unless requested)
- Drafts still in preparation
- Internal-only changes or infrastructure updates

## How It Works

1. **Identify release context**:
   - Check title and description for release version (e.g., `v2.4.0`, `Release 1.5.1`)
   - Look for existing release notes file in the repository
   - Common locations:
     ```
     CHANGELOG.md / CHANGELOG.mdx
     RELEASE_NOTES.md / RELEASE_NOTES.mdx
     docs/releases/v2.4.0.md
     docs/releases/index.mdx
     docs/changelog.md
     releases/2024-01.md
     ```
   - Parse the last release date/version from the file

2. **Analyze changes since last release**:
   - Review commits, merged changes, and code changes since last release tag or date
   - Extract screenshots from descriptions and comments for visual features
   - Categorize changes into user-relevant groups:
     - **New Features**: User-visible functionality additions
     - **Improvements**: Enhancements to existing features
     - **Bug Fixes**: Resolved issues that affected users
     - **Performance**: Speed/efficiency improvements users will notice
     - **Security**: Security patches or improvements
     - **Breaking Changes**: Changes requiring user action or migration

3. **Determine user-facing impact**:
   - **High Priority**: New features, breaking changes, major UX improvements
   - **Medium Priority**: Bug fixes, performance improvements, notable enhancements
   - **Low Priority**: Minor tweaks, internal refactoring with indirect benefits
   - **Exclude**: Internal refactoring, test changes, CI/CD updates (unless they affect users)

4. **Generate release notes content**:
   - Follow the existing format and tone in the release notes file
   - Keep entries concise: one clear sentence per item
   - Use action-oriented language emphasizing user benefits
   - Group related changes together
   - Include links to relevant documentation or migration guides
   - Include screenshots for visual features (or suggest where to add them)

   Example format:
   ```markdown
   ## Version 2.4.0 - 2024-11-06

   ### New Features
   - **Dark Mode Support**: Toggle between light and dark themes in user settings for improved readability
   - **Bulk Export**: Export multiple projects at once to JSON, CSV, or PDF formats

   ### Improvements
   - Search results now load 3x faster for large datasets
   - Improved mobile responsiveness for dashboard and settings pages

   ### Bug Fixes
   - Fixed issue where notifications weren't displaying for @mentions in comments
   - Resolved crash when uploading files larger than 10MB

   ### Breaking Changes
   - API endpoints now require authentication token in header (previously accepted query params)
     - [Migration guide →](docs/migration/v2.4.0.md)
   ```

5. **Suggest updates to release notes**:
   - If release notes file exists and has content → suggest additions/modifications
   - If release notes file is empty or doesn't exist → suggest full content
   - Post as a comment with suggested content:
   ```markdown
   ## 📝 Suggested Release Notes

   Based on changes since `v2.3.0`, here are suggested additions to your release notes:

   ```markdown
   ### New Features
   - **Dark Mode Support**: Toggle between light and dark themes in user settings for improved readability
   - **Bulk Export**: Export multiple projects at once to JSON, CSV, or PDF formats

   ### Bug Fixes
   - Fixed issue where notifications weren't displaying for @mentions in comments
   ```

   **Rationale**:
   - Dark mode implementation (#1234) adds significant UX improvement
   - Bulk export feature (#1245) addresses top user request
   - Notification bug (#1256) affected 15% of active users

   **Excluded from notes**:
   - Refactored internal caching layer (no user-visible change)
   - Updated test suite (internal improvement)

   ---

   Feel free to edit, reorder, or modify these suggestions to match your voice and priorities.
   ```

6. **Provide context and reasoning**:
   - Explain why certain changes were included
   - Note which changes were excluded and why
   - Reference change numbers for traceability
   - Suggest categories if the project uses them

7. **Handle different release note formats**:
   - **Keep a Changelog format**: Follow semantic versioning categories
   - **Date-based releases**: Group by release date
   - **Narrative style**: Focus on story and user journey
   - **Technical changelogs**: Include API changes and deprecations

8. **Search for related documentation**:
   - For dependency upgrades, find official release notes
   - For breaking changes, suggest creating migration guides
   - For new features, note if documentation updates are needed

## Why This Matters

- **Better User Experience**: Clear, comprehensive release notes improve feature adoption and reduce support questions
- **Time Savings**: Automatically surfaces user-facing changes and preserves context without manual git log review
