# Claude Code Review Skill

Free AI-powered code review using Claude Code CLI — an open-source alternative to CodeRabbit.

## What is this?

This is a **skill** for [Claude Code](https://claude.ai/claude-code) that enables automated code review:

- **Local review** — check your changes before committing
- **Branch review** — compare your feature branch to main
- **PR review** — review GitHub PRs and post comments automatically

## Why use this instead of CodeRabbit?

| Feature | CodeRabbit | This Skill |
|---------|-----------|------------|
| Price | $15-30/user/month | Free (pay for Claude API only) |
| Setup | SaaS integration | Copy one file |
| Customization | Limited | Full control |
| Privacy | Code goes to their servers | Runs locally |

## Installation

### Option 1: Add to your project (recommended)

Create `.claude/skills/code-review/SKILL.md` in your repo:

```bash
mkdir -p .claude/skills/code-review
curl -o .claude/skills/code-review/SKILL.md https://raw.githubusercontent.com/anthroos/claude-code-review-skill/main/SKILL.md
```

### Option 2: Global installation

Add to your global Claude Code config:

```bash
mkdir -p ~/.claude/skills/code-review
curl -o ~/.claude/skills/code-review/SKILL.md https://raw.githubusercontent.com/anthroos/claude-code-review-skill/main/SKILL.md
```

## Prerequisites

1. **Claude Code CLI** — [Install here](https://claude.ai/claude-code)
2. **GitHub CLI** (for PR reviews) — `brew install gh && gh auth login`

## Usage

### Review local changes

```bash
claude "review my changes"
```

### Review a PR

```bash
claude "review PR 123"
```

### Review and post comments to GitHub

```bash
claude "review PR 123 and post comments"
```

### Security-focused review

```bash
claude "security review PR 456"
```

## What it checks

- **Security** — SQL injection, XSS, hardcoded secrets, OWASP Top 10
- **Bugs** — null handling, off-by-one errors, race conditions
- **Code quality** — DRY violations, naming, complexity
- **Performance** — N+1 queries, memory leaks

## Example output

```markdown
## Code Review Summary

### Critical Issues
- [src/api.ts:45] SQL injection vulnerability - user input passed directly to query

### Warnings
- [src/utils.ts:12] Unused variable 'tempData'
- [src/auth.ts:78] Missing error handling for network failures

### Suggestions
- [src/components/Form.tsx:23] Consider extracting validation logic to separate function

### Good practices noticed
- Consistent error handling pattern in services/
- Good test coverage for auth module
```

## Git hook integration

Add to `.git/hooks/pre-push`:

```bash
#!/bin/bash
echo "Running AI code review..."
claude "quick review of my changes, only show critical issues" --print
```

## CI/CD integration

Add to your GitHub Actions workflow:

```yaml
- name: AI Code Review
  run: |
    claude "review this PR and post a comment with findings" --print
  env:
    ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
```

## License

MIT — use freely, modify as needed.

## Credits

Built for the Claude Code community.
