---
title: "Fix Typos"
description: "Suggest fixes for typos in titles, descriptions, and code while learning codebase-specific terminology"
slug: "fix_typos"
when: "PR/MRs with new or modified code, comments, documentation, or metadata (title/description)"
actions: "Post inline suggestions for typos and update codebase dictionary"
---

# Fix Typos

Automatically detect and suggest fixes for spelling errors in titles, descriptions, code comments, and documentation while learning codebase-specific terminology.

## When to Use This

Apply when changes include:
- Changes to code comments or docstrings
- Updates to README, documentation files, or markdown content
- New or modified titles and descriptions
- String literals in user-facing messages
- Variable/function names that include English words

## How It Works

1. **Run `typos` on changed files**:
   - `typos` CLI is pre-installed and available in the environment
   - Run on only the files changed in the PR/MR:
     ```bash
     typos --format json <changed-files...>
     ```
   - The `--format json` flag produces structured output with file paths, line numbers, original typos, and suggested corrections
   - If a `_typos.toml` or `typos.toml` config exists in the repo, `typos` will respect it automatically (custom dictionaries, ignored paths, etc.)

2. **Also scan PR/MR title and description**:
   - `typos` only checks files — separately scan the PR/MR title and description for typos

3. **Filter results to only new/modified lines**:
   - Cross-reference `typos` output with the PR diff
   - Only flag typos that appear on lines added or modified in this PR — do not flag pre-existing typos in unchanged code

4. **Post inline suggestions** for detected typos:
   ```markdown
   **Typo Detected**

   ```suggestion
   // Process user authentication and generate token
   ```

   Suggested corrections:
   - "Proccess" → "Process"
   - "authentcation" → "authentication"
   - "genrate" → "generate"
   ```

5. **Update title/description** if typos found:
   - Post a comment suggesting the corrected title/description
   - Example: "Consider updating title: 'Implemnt new cache startegy' → 'Implement new cache strategy'"

6. **Customize via repo config**:
   - Teams can add a `_typos.toml` to their repo to:
     - Define codebase-specific valid words (e.g., product names, internal terms)
     - Exclude paths (e.g., `codegen/*`, `*.generated.*`, vendored files)
     - Add custom corrections

## Why This Matters

- **Better Communication**: Clear, typo-free code and documentation improve searchability and reduce misunderstandings
- **Smart Learning**: Self-improving dictionary learns codebase terminology to reduce false positives over time
