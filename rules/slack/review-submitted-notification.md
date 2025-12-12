---
title: "Review Submitted Slack Notification"
description: "Send Slack DM to PR/MR authors when a reviewer submits their review"
slug: "review_submitted_slack"
when: "A reviewer submits a review (approved, changes requested, or commented)"
actions: "Send direct message via Slack to PR/MR author with review outcome and link"
integrations: "slack"
---

# Review Submitted Slack Notification

Automatically notify pull request and merge request authors via Slack direct message when a reviewer completes and submits their review.

## When to Use This

Apply when:

- A reviewer submits a review with a verdict (approved, changes requested, or comment-only)
- PR/MR author is a human user with a matching Slack account
- Review contains substantive feedback (not just an empty approval click)

Do NOT apply for:

- Draft pull requests or work-in-progress merge requests
- Reviews submitted by bots or automated accounts
- Author reviewing their own PR/MR (self-reviews)
- Inline comments without a formal review submission
- Duplicate notifications for the same review

## How It Works

1. **Detect review submission event**:
   - **GitHub**: `pull_request_review` event with `action: submitted`
   - **GitLab**: MR approval event or note event with review verdict

2. **Extract review details**:
   - **Review verdict**:
     - GitHub: `APPROVED`, `CHANGES_REQUESTED`, or `COMMENTED`
     - GitLab: Approval, unapproval, or comment-based review
   - **Reviewer identity**: Username and email
   - **Review body**: Summary comment if provided

3. **Verify exclusion conditions**:
   - Skip if reviewer is a bot
   - Skip if reviewer is the PR/MR author (self-review)
   - Skip if PR/MR is in draft state
   - Skip if this exact review was already notified

4. **Match PR/MR author to Slack user**:
   - Get author email from platform API
   - Query Slack `users.lookupByEmail` endpoint
   - If no match found, skip notification (fail gracefully)

5. **Send Slack direct message to author**:

   **Message format by verdict**:

   **Approved**:
   ```md
   :white_check_mark: @reviewer-name approved your pull request

   **[PR Title #123](https://github.com/owner/repo/pull/123)**

   > "Looks great! Nice work on the error handling."
   ```

   **Changes Requested**:
   ```md
   :arrows_counterclockwise: @reviewer-name requested changes on your pull request

   **[PR Title #123](https://github.com/owner/repo/pull/123)**

   > "A few suggestions on the API design, see inline comments."
   ```

   **Commented** (no verdict):
   ```md
   :speech_balloon: @reviewer-name left feedback on your pull request

   **[PR Title #123](https://github.com/owner/repo/pull/123)**

   > "Had some questions about the caching strategy."
   ```

   **Include review snippet**:
   - Show first ~150 characters of review body if present
   - Truncate with "..." if longer
   - Omit quote block if review has no body text

6. **Handle errors gracefully**:
   - Slack user not found: Log and skip silently
   - Slack API errors: Log warning, do not fail the rule
   - Empty review body: Send notification without quote block

## Examples

### Example 1: Approval with Comment

```
Event: Review submitted on PR #456
Reviewer: @bob
Verdict: APPROVED
Body: "LGTM! The refactoring really cleaned this up."
Author: @alice

Action:
- DM @alice:
  ":white_check_mark: @bob approved your pull request
  [Add user authentication #456](link)
  > 'LGTM! The refactoring really cleaned this up.'"
```

### Example 2: Changes Requested

```
Event: Review submitted on PR #456
Reviewer: @carol
Verdict: CHANGES_REQUESTED
Body: "Please add error handling for the edge case on line 45."
Author: @alice

Action:
- DM @alice:
  ":arrows_counterclockwise: @carol requested changes on your pull request
  [Add user authentication #456](link)
  > 'Please add error handling for the edge case on line 45.'"
```

### Example 3: Comment-Only Review

```
Event: Review submitted on PR #789
Reviewer: @dave
Verdict: COMMENTED
Body: "Curious about the performance implications here - have we benchmarked this?"
Author: @eve

Action:
- DM @eve:
  ":speech_balloon: @dave left feedback on your pull request
  [Optimize database queries #789](link)
  > 'Curious about the performance implications here - have we benchmarked this?'"
```

### Example 4: Self-Review Skipped

```
Event: Review submitted on PR #101
Reviewer: @alice
Author: @alice

Action: None (self-review)
```

### Example 5: Approval Without Body

```
Event: Review submitted on PR #202
Reviewer: @frank
Verdict: APPROVED
Body: (empty)
Author: @grace

Action:
- DM @grace:
  ":white_check_mark: @frank approved your pull request
  [Fix login bug #202](link)"
```

## Why This Matters

- **Immediate Feedback Loop**: Authors know the moment feedback arrives, allowing them to address it while context is fresh
- **Faster Iteration Cycles**: No more checking GitHub/GitLab repeatedly to see if reviews came in
- **Clear Action Items**: The verdict emoji immediately signals whether author can merge or needs to make changes
- **Context Preservation**: Seeing the review summary in Slack helps authors prioritize which PRs need attention
- **Reduced Notification Fatigue**: Targets only the author rather than broadcasting to channels
