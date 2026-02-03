# Code Review

> AI-powered code review for PRs and local changes — free alternative to CodeRabbit

## When to use

- "do code review"
- "review my PR"
- "review PR #123"
- "check my changes before commit"
- "what's wrong with my code"
- "/review"
- "/review-pr"

## Dependencies

- External: `gh` CLI (GitHub), `git`

## Modes

### 1. Local review (uncommitted changes)

Reviews `git diff` — changes not yet committed.

### 2. Branch review (vs main/master)

Reviews all changes in current branch compared to main.

### 3. PR review (GitHub)

Fetches diff from GitHub PR and can post comments.

## How to execute

### Step 1: Determine mode

Ask user or detect automatically:
- If PR number provided → PR review
- If uncommitted changes exist → local review
- If on feature branch → branch review

### Step 2: Get diff

**Local:**
```bash
git diff HEAD
```

**Branch (vs main):**
```bash
# First detect default branch
DEFAULT_BRANCH=$(git symbolic-ref refs/remotes/origin/HEAD 2>/dev/null | sed 's@^refs/remotes/origin/@@' || echo "main")
git diff $DEFAULT_BRANCH...HEAD
```

**PR:**
```bash
gh pr diff <PR_NUMBER>
```

### Step 3: Get context

For better review, read related files:
```bash
# List changed files
git diff --name-only HEAD

# Read each file fully for context
```

### Step 4: Analysis

Check for:

**Bugs & Logic:**
- Off-by-one errors
- Null/undefined handling
- Race conditions
- Edge cases

**Security (OWASP Top 10):**
- SQL injection
- XSS
- Command injection
- Hardcoded secrets
- Insecure dependencies

**Code Quality:**
- DRY violations
- Excessive complexity
- Unclear naming
- Missing error handling
- Inconsistent style

**Performance:**
- N+1 queries
- Memory leaks
- Unnecessary re-renders (React)
- Missing indexes (DB)

### Step 5: Output result

**Format for local review:**
```markdown
## Code Review Summary

### Critical Issues
- [file:line] Problem description

### Warnings
- [file:line] Description

### Suggestions
- [file:line] Improvement suggestion

### Good practices noticed
- What was done well
```

**For GitHub PR — post comments:**
```bash
# General comment
gh pr comment <PR_NUMBER> --body "## AI Code Review

[Review content]"

# Or line comments via API
# Replace {owner}, {repo}, {pr} with actual values, e.g.:
# gh api repos/myuser/myrepo/pulls/123/comments
gh api repos/{owner}/{repo}/pulls/{pr}/comments \
  --method POST \
  -f body="Comment text" \
  -f path="file.ts" \
  -f line=42 \
  -f side="RIGHT"
```

## Parameters

| Parameter | Description | Default |
|-----------|-------------|---------|
| `PR_NUMBER` | Pull Request number | - |
| `--post` | Post comments to GitHub | false |
| `--severity` | Minimum level (critical/warning/suggestion) | suggestion |
| `--focus` | Focus: security, performance, style, all | all |

## Examples

### Example 1: Quick local review

```
User: check my changes

Claude: [git diff HEAD]
Claude: [analysis]
Claude: Found 2 issues:
1. src/api.ts:45 - Potential SQL injection
2. src/utils.ts:12 - Unused variable
```

### Example 2: PR review with posting

```
User: review PR 123 and post comments

Claude: [gh pr diff 123]
Claude: [analysis]
Claude: [gh pr comment 123 --body "..."]
Claude: Done! Posted 3 comments to PR.
```

### Example 3: Security-focused review

```
User: security review PR 456

Claude: [focus=security]
Claude: [detailed OWASP analysis]
```

## Review Checklist

```markdown
## Security
- [ ] No hardcoded secrets/API keys
- [ ] Input validation present
- [ ] No SQL/command injection
- [ ] Proper authentication checks
- [ ] Sensitive data not logged

## Logic
- [ ] Edge cases handled
- [ ] Error handling present
- [ ] No obvious bugs
- [ ] Business logic correct

## Code Quality
- [ ] Readable and maintainable
- [ ] No code duplication
- [ ] Proper naming
- [ ] Tests included (if applicable)

## Performance
- [ ] No N+1 queries
- [ ] Efficient algorithms
- [ ] No memory leaks
```

## Limitations

- Cannot run code (static analysis only)
- Cannot see runtime behavior
- Needs context for complex architectural decisions
- GitHub API rate limits when posting many comments

## Troubleshooting

| Problem | Solution |
|---------|----------|
| `gh: command not found` | `brew install gh && gh auth login` |
| No diff output | Check if changes exist: `git status` |
| PR not found | Check PR number and access rights |
| Can't post comments | Check permissions: `gh auth status` |
