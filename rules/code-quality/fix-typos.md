---
title: "Fix Typos"
description: "Suggest fixes for typos in PR titles, descriptions, and code while learning codebase-specific terminology"
slug: "fix_typos"
when: "PRs with new or modified code, comments, documentation, or PR metadata (title/description)"
actions: "Post inline suggestions for typos and update codebase dictionary"
---

# Fix Typos

Automatically detect and suggest fixes for spelling errors in PR titles, descriptions, code comments, and documentation while learning codebase-specific terminology.

## When to Use This

Apply when PRs include:
- Changes to code comments or docstrings
- Updates to README, documentation files, or markdown content
- New or modified PR titles and descriptions
- String literals in user-facing messages
- Variable/function names that include English words

## How It Works

1. **Maintain codebase dictionary**:
   - Build a list of technical terms, product names, and domain-specific vocabulary unique to this codebase
   - Track terms that appear frequently across the codebase (e.g., function names, class names, product features)
   - Examples: proprietary tool names, internal APIs, custom DSL keywords, company-specific acronyms
   - Store in `.gitar/dictionary.json` or similar persistent location

2. **Scan for typos** in multiple locations:
   ```javascript
   // PR Title: "Implemnt new cache startegy"
   // Should be: "Implement new cache strategy"

   // Code comment with typo
   /**
    * Proccess user authentcation and genrate token
    */
   // Should be: "Process user authentication and generate token"

   // User-facing string
   const message = "Sucessfully saved your prefernces";
   // Should be: "Successfully saved your preferences"
   ```

3. **Smart dictionary checking**:
   - Check against standard English dictionary
   - Check against codebase-specific dictionary
   - If a "typo" appears 3+ times across the codebase → likely valid term, add to dictionary
   - Common false positives: `kubectl`, `repo`, `async`, `webhook`, `OAuth`, custom product names

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

5. **Update PR title/description** if typos found:
   - Post a comment suggesting the corrected title/description
   - Example: "Consider updating PR title: 'Implemnt new cache startegy' → 'Implement new cache strategy'"

6. **Learn from feedback**:
   - If author marks suggestion as "not a typo" or ignores it → add to dictionary
   - Track correction acceptance rate to improve detection accuracy

## Why This Matters

- **Better Communication**: Clear, typo-free code and documentation improve searchability and reduce misunderstandings
- **Smart Learning**: Self-improving dictionary learns codebase terminology to reduce false positives over time
