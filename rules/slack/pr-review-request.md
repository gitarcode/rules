---
title: "Review Request Slack Notification"
description: "Send Slack DM to reviewers when they are assigned to a PR/MR"
slug: "pr_review_request_slack"
when: "PR/MR is opened with reviewers assigned, or reviewers are added to an existing PR/MR"
actions: "Send direct message via Slack to each assigned reviewer with PR/MR details"
integrations: "slack"
---

# Review Request Slack Notification

Automatically notify reviewers via Slack direct message when they are assigned to review a pull request or merge request.

## When to Use This

Apply when:

- PR/MR is opened with one or more reviewers already assigned
- New reviewer(s) added to an existing PR/MR (metadata update)
- Reviewer is a human user with a matching Slack account

Do NOT apply for:

- Draft pull requests or work-in-progress merge requests
- Reviewers who are bots or automated accounts
- PR/MR author assigning themselves as reviewer
- Reviewers who have already been notified for this PR/MR

## How It Works

1. **Detect review request event**:
   - **On PR/MR open**: Check if reviewers are assigned in the initial request
   - **On metadata update**: Detect newly added reviewers (compare before/after reviewer lists)

2. **Filter reviewers to notify**:
   - Exclude the PR/MR author (self-review)
   - Exclude bot accounts
   - Exclude reviewers already notified for this PR/MR (track via comments or metadata)
   - Only include newly assigned reviewers on updates

3. **Match reviewers to Slack users**:
   - Get reviewer email from platform API
   - Query Slack `users.lookupByEmail` endpoint
   - If no match found, skip notification for that reviewer (fail gracefully)

4. **Send Slack direct message to each reviewer**:

   **Message format**:

   ```md
   👀 Review requested on a pull request

   **[PR Title #123](https://github.com/owner/repo/pull/123)**

   Requested by @author-name

   > First 1-2 lines of PR description or commit message summary...
   ```

   **Include contextual details**:
   - PR/MR title with link
   - Author who requested the review
   - Brief description snippet (first ~100 chars, if available)
   - File count or change size indicator (e.g., "+150 / -30 lines")

5. **Track notification sent**:
   - Record that reviewer was notified to prevent duplicate DMs on subsequent updates
   - Can use PR/MR comment metadata or external tracking

6. **Handle errors gracefully**:
   - Slack user not found: Log and skip (don't block workflow)
   - Slack API errors: Log warning, do not fail the rule
   - Rate limits: Queue for retry

## Examples

### Example 1: PR Opened with Reviewers

```
Event: PR #456 opened
Title: "Add user authentication flow"
Author: @alice
Reviewers: @bob, @carol

Action:
- DM @bob: "👀 Review requested... [Add user authentication flow #456]..."
- DM @carol: "👀 Review requested... [Add user authentication flow #456]..."
```

### Example 2: Reviewer Added to Existing PR

```
Event: PR #456 updated (metadata change)
Before reviewers: [@bob]
After reviewers: [@bob, @dave]

Action:
- DM @dave: "👀 Review requested..."
- Skip @bob (already notified)
```

### Example 3: Self-Review Filtered Out

```
Event: PR #789 opened
Author: @eve
Reviewers: @eve, @frank

Action:
- Skip @eve (author cannot review own PR)
- DM @frank: "👀 Review requested..."
```

## Why This Matters

- **Faster Review Turnaround**: Reviewers are notified immediately in Slack where they're already working, rather than relying on email or platform notifications
- **Reduced Review Latency**: Eliminates the "I didn't see the review request" problem that delays merges
- **Better Context Delivery**: DM includes enough context for reviewer to prioritize without opening the link first
- **Respects Attention**: Direct message ensures visibility without noisy channel posts
