---
title: "Flaky Test Retry"
description: "Automatically retry CI jobs when flaky test failures are detected"
slug: "flaky_test_retry"
when: "CI jobs fail with test failures that exhibit flakiness patterns"
actions: "Detect flaky test patterns and re-trigger failed CI jobs"
---

# Flaky Test Auto-Retry

Automatically detect flaky test failures and retry CI jobs to prevent false-negative build failures from blocking development.

## When to Use This

Apply when:
- CI/CD pipeline fails due to test failures OR infrastructure/dependency issues
- Failures show characteristics of flakiness (timing, network, race conditions)
- Job has not been retried more than the configured maximum (default: 2 retries)

Do NOT apply for:
- Code compilation errors (syntax errors, type errors, missing symbols)
- Linting or static analysis failures
- Tests that fail consistently across runs
- Manual test runs or local development

## How It Works

1. **Detect flaky patterns** in CI failure logs:
   - **Timeout failures**: Tests that exceed time limits inconsistently
     ```
     Error: Test exceeded 5000ms timeout
     TimeoutError: waiting for selector ".loaded"
     ```
   - **Network/external service failures**:
     ```
     Error: connect ECONNREFUSED 127.0.0.1:3000
     Error: Failed to fetch from api.example.com
     socket hang up
     ```
   - **Build-time dependency/infrastructure failures**:
     ```
     Failed to connect to registry.npmjs.org
     Failed to connect to port 443: No route to host
     Error downloading from maven central
     Temporary failure resolving 'pypi.org'
     curl: (7) Failed to connect to remote host
     ```
   - **Race conditions and timing issues**:
     ```
     Error: Element not found (may not have rendered yet)
     AssertionError: expected 5 to equal 6 (async state not settled)
     ```
   - **Intermittent database/cache issues**:
     ```
     Error: Connection pool exhausted
     Error: Redis timeout
     ```
   - **Resource cleanup failures**:
     ```
     Error: Port 8080 already in use
     Error: Unable to delete temp directory
     ```

2. **Check retry history**:
   - Look for previous retry comments on the PR/MR
   - Count how many times this job has been retried
   - If retry count >= max retries (default: 2) → do not retry again

3. **Analyze failure consistency**:
   - If the same test failed in the last run but a different test failed this time → likely flakiness
   - If multiple tests fail randomly across runs → systemic flakiness
   - If all tests pass locally but fail in CI → environment-specific flakiness

4. **Re-trigger the CI job**:
   - Re-run the failed workflow/job
   - Track retry metadata to prevent infinite retry loops

## Why This Matters

- **Faster Development**: Eliminates manual retries and prevents false-negative failures from blocking progress
- **Better CI Experience**: Reduces frustration with flaky tests while maintaining confidence in real failures
