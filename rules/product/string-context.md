---
title: "String Context for Translation"
description: "Ensure ambiguous strings in localization files include translator context comments"
slug: "string_context"
when: "PR/MRs that modify Android strings.xml, iOS Localizable.strings, or i18n/localization JSON files"
actions: "Post inline suggestions with context comments and apply 'needs-translation-context' label"
---

# String Context for Translation

Ensures ambiguous strings in localization files include translator context comments.

## When to Use This

Apply when modifying:
- Android `strings.xml`, iOS `Localizable.strings`
- i18n/localization JSON files (`en.json`, `translations/*.json`)

## How It Works

1. **Identify ambiguous strings**:
   - Generic actions: "Apply", "Save", "Submit", "OK", "Cancel", "Add", "Remove", "Open", "Close"
   - Ambiguous nouns: "Post", "Order", "Match", "Block", "File", "Record", "Address"

2. **Check for context comments** in required format:
   ```xml
   <!-- Context: Button to apply for a job posting -->
   <string name="btn_apply">Apply</string>
   ```

   ```json
   {
     // Context: Save user profile changes
     "save": "Save"
   }
   ```

3. **Post inline suggestions** when context is missing:

   **If context can be inferred:**
   ```markdown
   **Add Translation Context**

   ```xml
   <!-- Context: Apply promo code button in checkout -->
   <string name="apply_code">Apply</string>
   ```
   Please confirm or edit.
   ```

   **If context cannot be inferred:**
   ```markdown
   **Translation Context Required**

   Add comment describing where/how this string is used:
   ```xml
   <!-- Context: [WHERE] - [PURPOSE] -->
   <string name="btn_submit">Submit</string>
   ```
   ```

4. **Apply label**: `needs-translation-context`

## Why This Matters

Without context, "Apply" could be translated as:
- "Postuler" (apply for job) vs "Appliquer" (apply setting) in French
- Prevents costly translation revision cycles
- Critical when using external translation vendors
