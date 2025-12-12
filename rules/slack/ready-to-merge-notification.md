---
title: "Ready to Merge Slack Notification"
description: "Send Slack DM to PR/MR authors when all checks pass and their contribution is ready to merge"
slug: "ready_to_merge_slack"
when: "PR/MR becomes ready to merge (all checks pass, approvals met, no conflicts)"
actions: "Send direct message via Slack to PR/MR author with merge-ready status"
integrations: "slack"
---

# Ready to Merge Slack Notification

Automatically notify pull request and merge request authors via Slack direct message when their contribution passes all required checks and is ready to merge.

## When to Use This

Apply when:

- PR/MR status indicates ready to merge (all checks passed, approvals obtained, no conflicts)
- PR/MR is not in draft or work-in-progress state
- PR/MR author is a human user, not a bot

Do NOT apply for:

- Draft pull requests or work-in-progress merge requests
- PR/MRs created by bots (Dependabot, Renovate, automated CI accounts, etc.)
- Checks still pending or failing
- Merge conflicts present
- Required approvals not yet obtained

## How It Works

1. **Check platform-native merge readiness**:
   - **GitHub**: Verify `mergeable_state == "clean"` via PR API
     - Confirms all required checks passed, approvals obtained, branch up-to-date, no conflicts
     - Automatically respects repository branch protection rules
   - **GitLab**: Verify `merge_status == "can_be_merged"` via MR API
     - Confirms pipeline passed, approvals met, no conflicts
     - Automatically respects project merge request settings

   Using the platform's native merge status is more reliable than manually checking individual CI jobs and approval counts.

2. **Verify exclusion conditions**:
   - **Check if draft**:
     - GitHub: `draft == true`
     - GitLab: `draft == true` OR `work_in_progress == true`
   - **Check if bot-created**:
     - GitHub: `author.type == "Bot"` OR username matches `*[bot]` pattern
     - GitLab: `author.bot == true` OR username matches bot patterns
     - Common bot patterns: `dependabot`, `renovate`, `github-actions`, `gitlab-bot`, `snyk-bot`

   If any exclusion condition matches, skip this rule.

3. **Check if author was already notified**:
   - Track notifications sent per PR/MR to prevent duplicate messages
   - Only notify once when transitioning to merge-ready state

4. **Match PR/MR author to Slack user**:
   - Get author email from platform API
   - Query Slack `users.lookupByEmail` endpoint
   - If no match found, skip notification (fail gracefully)

5. **Send Slack direct message to author**:

   **Message format**:

   ```md
   :white_check_mark: Your pull request is ready to merge!

   **[PR Title #123](https://github.com/owner/repo/pull/123)**

   :white_check_mark: All checks passed
   :white_check_mark: Approved by 2 reviewers
   ```

   **Dynamic status details**:
   - Show approval count if approvals were required
   - Show check summary (e.g., "12 checks passed")
   - Omit sections that aren't applicable

6. **Handle errors gracefully**:
   - Slack API unavailable: Log error, do not block PR/MR workflow
   - Rate limits hit: Queue notification for retry
   - User not found: Log and skip silently

## Examples

### Example 1: Standard Merge Ready

```
PR #456: "Add user authentication"
Author: @alice
Status: All 8 checks passed, 2 approvals, no conflicts

Action:
- DM @alice:
  ":white_check_mark: Your pull request is ready to merge!
  [Add user authentication #456](link)
  :white_check_mark: All checks passed
  :white_check_mark: Approved by 2 reviewers"
```

### Example 2: Bot PR Skipped

```
PR #789: "Bump lodash from 4.17.20 to 4.17.21"
Author: dependabot[bot]
Status: Ready to merge

Action: None (bot-authored)
```

### Example 3: Draft PR Skipped

```
PR #101: "WIP: Refactor database layer"
Author: @bob
Status: Draft, all checks passing

Action: None (draft PR)
```

### Example 4: Still Has Failing Checks

```
PR #202: "Update payment flow"
Author: @carol
Status: 7/8 checks passed, 1 failing

Action: None (not merge-ready)
```

## Why This Matters

- **Faster Merge Cycles**: Authors are immediately notified when their work is ready, eliminating manual status checking
- **Reduced Context Switching**: Notification comes directly to Slack where developers work, rather than requiring platform tab monitoring
- **Improved Team Velocity**: Reduces the time between "ready to merge" and actual merge, keeping development momentum high
- **Better Developer Experience**: Clear, proactive communication reduces uncertainty about PR/MR status
