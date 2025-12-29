---
title: "Linear Auto-Linking"
description: "Find and attach relevant Linear issues to changes without linked tickets"
slug: "linear_pr_linking"
when: "PR/MR opened without any Linear issue reference"
actions: "Search author's recent Linear issues, link only with >95% confidence, otherwise suggest candidates"
integrations: "linear"
---

# Linear Auto-Linking

Automatically link relevant Linear issues when changes are created without ticket references, using multi-signal validation to ensure high confidence matches.

## When to Use This

- Opened with no Linear reference in title, description, or branch name
- Author has Linear integration enabled

Skip if already references a Linear issue or has `no-ticket` label.

## How It Works

### 1. Check for Existing References

Scan for issue IDs in:
- Branch name (e.g., `eng-123-fix-login-bug`)
- Title with brackets (e.g., `[ENG-123] Fix login`)
- Description with magic words (e.g., `Fixes ENG-123`)

If found → link directly (explicit reference = 100% confidence).

### 2. Gather Candidate Issues

Search Linear for potential matches:
- Author's assigned issues (In Progress, Todo states)
- Author's recently created issues (14 days)
- Issues author recently commented on (7 days)

**Exclude from candidates:**
- Issues in closed states (Done, Canceled, Completed, Duplicate)
- Issues already linked to another open PR/MR
- Issues assigned to others who have their own active branch

### 3. Score Each Candidate Using Multi-Signal Analysis

#### Tier 1: Strong Positive Signals (15-25 points each)

| Signal | Points | Example |
|--------|--------|---------|
| **Author is assignee** | 25 | Issue assigned to author |
| **Issue state is "In Progress"** | 20 | Author actively working on it |
| **File path overlap** | 20 | Issue mentions `auth/` and diff modifies `src/auth/*` |
| **Branch name partial match** | 15 | Branch `fix-login-timeout` matches issue "Login timeout error" |
| **Semantic title similarity >0.8** | 15 | Title closely matches issue title |

#### Tier 2: Supporting Signals (5-10 points each)

| Signal | Points | Example |
|--------|--------|---------|
| **Author recently updated issue** | 10 | Commented or edited within 48 hours |
| **Issue labels match file types** | 10 | `frontend` label + `.tsx` files changed |
| **Commit message keywords overlap** | 10 | Commit mentions "timeout" and issue title has "timeout" |
| **Same project/team** | 5 | Issue is in the team the author belongs to |
| **Issue created by author** | 5 | Author opened the issue |
| **Recent issue activity** | 5 | Issue updated within 7 days |

#### Tier 3: Negative Signals (subtract points)

| Signal | Points | Reason |
|--------|--------|--------|
| **Issue has linked PR/MR from another author** | -30 | Someone else is working on it |
| **Issue in "Done" or "Canceled" state** | -50 | Already completed |
| **Generic issue title** | -15 | Titles like "Bug fix", "Update", "Changes" |
| **File domain mismatch** | -20 | Issue tagged `backend` but only frontend files changed |
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
- [ ] Issue state allows work (not Done/Canceled/Duplicate)
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

Candidate Issue: ENG-456 "Auth session times out on slow connections"
- Author is assignee: +25
- Issue state "In Progress": +20
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

Candidate Issue: ENG-789 "Profile page improvements"
- Author is assignee: +25
- Same team: +5
- Generic title ("improvements"): -15
- No file path mentioned in issue: +0

Total: 15 → Too low, but...

Candidate Issue: ENG-790 "Add email validation to profile"
- Author created issue: +5
- Commit mentions "validation": +10
- Issue state "Todo": +0
- Label "frontend" + .tsx files: +10

Total: 25 → Still low, list both as suggestions
```

### Example 3: False Positive Avoided

```
Title: "Fix memory leak in worker pool"
Branch: hotfix/memory-leak
Author: @carol

Candidate Issue: ENG-100 "Memory optimization" (assigned to @carol)
- Author is assignee: +25
- Semantic similarity 0.6: +0
- Issue has linked PR/MR from @dave: -30
- Issue state "In Review": -10 (work in progress by other)

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
- [Linear integration docs](https://linear.app/docs/github-integration)
