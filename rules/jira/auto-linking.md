---
title: "Jira Auto-Linking"
description: "Find and attach relevant Jira issues to changes without linked tickets"
slug: "jira_pr_linking"
when: "PR/MR opened without any Jira issue reference"
actions: "Search author's recent Jira issues, link only with >95% confidence, otherwise suggest candidates"
integrations: "jira"
---

# Jira Auto-Linking

Automatically link relevant Jira issues when changes are created without ticket references, using multi-signal validation to ensure high confidence matches.

## When to Use This

- Opened with no Jira reference in title, description, or branch name
- Author has Jira integration enabled

Skip if already references a Jira issue or has `no-ticket` label.

## How It Works

### 1. Check for Existing References

Scan for issue keys in:
- Branch name (e.g., `PROJ-123-fix-login-bug`)
- Title with brackets (e.g., `[PROJ-123] Fix login`)
- Description with magic words (e.g., `Fixes PROJ-123`)

If found → link directly (explicit reference = 100% confidence).

### 2. Gather Candidate Issues

Search Jira for potential matches:
- Author's assigned issues (In Progress, To Do, Open statuses)
- Author's recently created issues (14 days)
- Issues author recently commented on (7 days)

**Exclude from candidates:**
- Resolved/Closed/Done issues
- Issues already linked to another open PR/MR
- Issues assigned to others who have their own active branch

### 3. Score Each Candidate Using Multi-Signal Analysis

#### Tier 1: Strong Positive Signals (15-25 points each)

| Signal | Points | Example |
|--------|--------|---------|
| **Author is assignee** | 25 | Issue assigned to author |
| **Issue status is "In Progress"** | 20 | Author actively working on it |
| **File path overlap** | 20 | Issue mentions `auth/` and diff modifies `src/auth/*` |
| **Branch name partial match** | 15 | Branch `fix-login-timeout` matches issue "Login timeout error" |
| **Semantic title similarity >0.8** | 15 | Title closely matches issue summary |

#### Tier 2: Supporting Signals (5-10 points each)

| Signal | Points | Example |
|--------|--------|---------|
| **Author recently updated issue** | 10 | Commented or edited within 48 hours |
| **Issue labels/components match file types** | 10 | `frontend` component + `.tsx` files changed |
| **Commit message keywords overlap** | 10 | Commit mentions "timeout" and issue summary has "timeout" |
| **Same project** | 5 | Issue is in the same Jira project |
| **Issue created by author (reporter)** | 5 | Author opened the issue |
| **Recent issue activity** | 5 | Issue updated within 7 days |

#### Tier 3: Negative Signals (subtract points)

| Signal | Points | Reason |
|--------|--------|--------|
| **Issue has linked PR/MR from another author** | -30 | Someone else is working on it |
| **Issue in "Done", "Resolved", or "Closed" status** | -50 | Already completed |
| **Generic issue summary** | -15 | Summaries like "Bug fix", "Update", "Changes" |
| **File domain mismatch** | -20 | Issue tagged `backend` component but only frontend files changed |
| **Issue >30 days without updates** | -10 | Likely stale |
| **Author is only a watcher** | -5 | Weak connection |

### 4. Calculate Confidence and Decide

```
confidence = min(100, max(0, total_score))
```

**Decision matrix:**

| Confidence | Action | Rationale |
|------------|--------|-----------|
| **≥95** | Auto-link silently | High certainty - explicit signals or strong corroboration |
| **80-94** | Auto-link with comment | Confident but inform author for verification |
| **60-79** | Suggest top match | Ask author to confirm before linking |
| **40-59** | List top 3 candidates | Multiple plausible options |
| **<40** | No action | Insufficient evidence |

### 5. Validation Before Auto-Linking (≥80 confidence)

Even with high scores, verify these safeguards:

**Required for auto-link:**
- [ ] Issue is not already linked to another open PR/MR
- [ ] Issue status allows work (not Done/Resolved/Closed/Won't Do)
- [ ] At least ONE Tier 1 signal is present
- [ ] No critical negative signals (linked PR/MR from other author)

**If any safeguard fails:** Downgrade to suggestion mode.

## Examples

### Example 1: High Confidence (Auto-link)

```
Title: "Fix authentication timeout on slow networks"
Branch: fix-auth-timeout
Author: @alice
Files: src/auth/session.ts, src/auth/refresh.ts

Candidate Issue: PROJ-456 "Auth session times out on slow connections"
- Author is assignee: +25
- Issue status "In Progress": +20
- File path overlap (auth/): +20
- Semantic similarity 0.87: +15
- Author updated issue 2 days ago: +10

Total: 90 → Auto-link with comment
```

### Example 2: Medium Confidence (Suggest)

```
Title: "Update user profile validation"
Branch: feature/profile-updates
Author: @bob

Candidate Issue: PROJ-789 "Profile page improvements"
- Author is assignee: +25
- Same project: +5
- Generic summary ("improvements"): -15
- No file path mentioned in issue: +0

Total: 15 → Too low, but...

Candidate Issue: PROJ-790 "Add email validation to profile"
- Author is reporter: +5
- Commit mentions "validation": +10
- Issue status "To Do": +0
- Component "frontend" + .tsx files: +10

Total: 25 → Still low, list both as suggestions
```

### Example 3: False Positive Avoided

```
Title: "Fix memory leak in worker pool"
Branch: hotfix/memory-leak
Author: @carol

Candidate Issue: PROJ-100 "Memory optimization" (assigned to @carol)
- Author is assignee: +25
- Semantic similarity 0.6: +0
- Issue has linked PR/MR from @dave: -30
- Issue status "In Review": -10 (work in progress by other)

Total: -15 → No action (correctly avoided linking to someone else's work)
```

## Why This Matters

- **Reduces false positives** - Multi-signal validation prevents incorrect links
- **Maintains trust** - Authors trust automation that's consistently accurate
- **Preserves traceability** - Correct links enable accurate metrics and history
- **Saves time** - High-confidence links reduce manual verification overhead

## Sources

Heuristics informed by research on issue-commit traceability:
- [FRLink](https://www.sciencedirect.com/science/article/abs/pii/S0950584916303792): File relevance improves link recovery by 40%
- [BTLink](https://link.springer.com/article/10.1007/s10664-023-10342-7): Multi-signal semantic analysis
- [GitHub linking patterns](https://docs.github.com/en/issues/tracking-your-work-with-issues/using-issues/linking-a-pull-request-to-an-issue)
- [Jira Smart Commits](https://support.atlassian.com/jira-software-cloud/docs/process-issues-with-smart-commits/)
