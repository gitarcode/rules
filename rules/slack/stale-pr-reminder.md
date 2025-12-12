---
title: "Stale PR Review Reminder"
description: "Send Slack DM to reviewers when a PR/MR has been waiting for review too long"
slug: "stale_pr_reminder_slack"
when: "PR/MR has been open for a configurable number of days without review activity"
actions: "Send direct message via Slack to assigned reviewers reminding them to review"
integrations: "slack"
---

# Stale PR Review Reminder

Automatically remind reviewers via Slack direct message when a pull request or merge request has been waiting for their review beyond a reasonable timeframe.

## When to Use This

Apply when:

- PR/MR has been open for N days without review activity (default: 3 days)
- PR/MR has assigned reviewers who haven't submitted a review
- OR review requested changes but author has pushed new commits and is awaiting re-review

Do NOT apply for:

- Draft pull requests or work-in-progress merge requests
- PR/MRs with `on-hold`, `blocked`, or `do-not-merge` labels
- PR/MRs where all reviewers have already submitted reviews
- Weekends/holidays (calculate business days if possible)
- Reviewers who are currently OOO (if detectable via Slack status)

## How It Works

1. **Identify stale PR/MRs**:
   - Calculate time since PR/MR was opened or since review was re-requested
   - Check if threshold exceeded (default: 3 days)
   - Verify PR/MR is still open and not in draft state

2. **Determine reviewers to remind**:
   - Get list of assigned reviewers
   - Filter to those who have NOT submitted a review
   - OR those whose review is outdated (requested changes, but new commits pushed since)
   - Exclude reviewers who received a reminder within the last 24 hours

3. **Check for blocking conditions**:
   - Skip if PR/MR has blocking labels (`on-hold`, `blocked`, `waiting-on-author`, etc.)
   - Skip if PR/MR has merge conflicts (author needs to resolve first)
   - Skip if CI is failing (may indicate PR isn't ready)

4. **Match reviewers to Slack users**:
   - Get reviewer email from platform API
   - Query Slack `users.lookupByEmail` endpoint
   - Optionally check Slack status for OOO indicators

5. **Send Slack direct message to each pending reviewer**:

   **Message format**:

   ```md
   :wave: Friendly reminder: a PR is waiting for your review

   **[PR Title #123](https://github.com/owner/repo/pull/123)**

   Opened by @author-name • Waiting for 4 days

   +250 / -80 lines across 12 files
   ```

   **Contextual variations**:

   - **First reminder (3 days)**:
     ```
     :wave: Friendly reminder: a PR is waiting for your review
     ```

   - **Second reminder (5+ days)**:
     ```
     :hourglass: This PR has been waiting for review for 6 days
     ```

   - **After re-request (author addressed feedback)**:
     ```
     :arrows_counterclockwise: @author-name addressed your feedback and is waiting for re-review
     ```

6. **Track reminder sent**:
   - Record timestamp of reminder per reviewer per PR/MR
   - Prevent spamming same reviewer within 24-48 hours

7. **Handle errors gracefully**:
   - Slack user not found: Skip silently
   - Slack API errors: Log and continue with other reviewers

## Examples

### Example 1: Initial Stale Reminder

```
PR #456: "Add payment processing"
Opened: 4 days ago
Reviewers: @bob (no review), @carol (no review)
CI: Passing
Labels: None blocking

Action:
- DM @bob: ":wave: Friendly reminder... waiting for 4 days"
- DM @carol: ":wave: Friendly reminder... waiting for 4 days"
```

### Example 2: Re-Review Requested

```
PR #456: "Add payment processing"
Timeline:
- Day 1: PR opened, @bob assigned
- Day 2: @bob requests changes
- Day 3: @alice (author) pushes fixes
- Day 5: Still waiting

Action:
- DM @bob: ":arrows_counterclockwise: @alice addressed your feedback and is waiting for re-review"
```

### Example 3: Blocked PR Skipped

```
PR #789: "Refactor auth system"
Opened: 5 days ago
Labels: ["blocked", "waiting-on-design"]
Reviewers: @dave (no review)

Action: None (blocked label present)
```

### Example 4: Partial Review Complete

```
PR #101: "Update API endpoints"
Opened: 4 days ago
Reviewers: @eve (approved), @frank (no review)

Action:
- Skip @eve (already reviewed)
- DM @frank: ":wave: Friendly reminder..."
```

## Why This Matters

- **Reduces Review Bottlenecks**: Gentle nudges keep PRs moving without requiring authors to manually ping reviewers
- **Maintains Team Velocity**: Long-open PRs lead to merge conflicts and context loss; timely reviews prevent this
- **Respectful Reminders**: DMs are less intrusive than public channel pings while still ensuring visibility
- **Accounts for Real Workflows**: Tracks re-review requests separately so authors aren't left waiting after addressing feedback
