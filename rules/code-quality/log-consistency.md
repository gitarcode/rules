---
title: "Log Statement Consistency"
description: "Enforce consistent logging style and structured format across the codebase"
slug: "log_consistency"
when: "PR/MRs that add or modify log statements"
actions: "Post inline comments comparing to existing patterns and apply 'logging-style' label"
---

# Log Statement Consistency

Ensures log messages follow consistent style for easier debugging and log analysis.

## When to Use This

Apply when adding or modifying log statements in any language.

## How It Works

1. **Check log message style**:
   - Present tense: "Processing request" not "Processed request"
   - Capital letter start, no ending period
   - Active voice: "Failed to connect" not "Connection failed"

2. **Validate structured format**:
   ```javascript
   // ✅ Good
   logger.info('Processing user authentication', { userId, email });
   logger.error('Failed to save profile: Database timeout', { userId, error });

   // ❌ Bad
   logger.info('authenticating user...');
   logger.error('error saving', err);
   ```

3. **Verify contextual data included**:
   - Relevant IDs/context as structured data
   - Error details with reason
   - Duration for operations >100ms

4. **Post inline comments** when logs don't match project style:
   ```markdown
   **Log Consistency Issue**

   Match project logging patterns:

   ```javascript
   // Current
   logger.info('saving user data');

   // Suggested (matches 23 existing logs)
   logger.info('Saving user profile', {
     userId: user.id,
     fields: updatedFields.length
   });
   ```

   - Capital letter start (project convention)
   - Present continuous tense
   - Structured context for querying
   ```

5. **Apply label**: `logging-style`

## Why This Matters

- **Debugging**: Consistent patterns easier to scan/search
- **Log tools**: Structured data enables filtering in Datadog/Splunk
- **Team efficiency**: Predictable format reduces cognitive load
- **Query-friendly**: Easier to grep/search with consistent patterns
