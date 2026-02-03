# Claude Code Review Skill

Enterprise-grade AI-powered code review using Claude Code CLI — comprehensive open-source alternative to CodeRabbit.

## What is this?

This is a **skill** for [Claude Code](https://claude.ai/claude-code) that enables automated, comprehensive code review with **280+ checks** across 15 categories:

- **Security** — OWASP Top 10 + extended security checks
- **Bugs & Logic** — Null handling, async issues, edge cases
- **Performance** — Database, API, frontend, algorithms
- **Code Quality** — SOLID principles, maintainability, readability
- **Testing** — Coverage, test quality, patterns
- **Accessibility** — WCAG compliance checks
- **i18n** — Internationalization issues
- **Documentation** — Missing or outdated docs
- **DevOps** — Health checks, observability, resilience
- **Git** — Version control best practices
- **React/Next.js** — Hooks, effects, component patterns
- **TypeScript** — Type safety, assertions, generics
- **Python** — Type hints, context managers, patterns
- **Node.js/Express** — Async handling, security middleware
- **SQL/Database** — Queries, indexes, ORM patterns

## Why use this instead of CodeRabbit?

| Feature | CodeRabbit | Anthropic Official | This Skill |
|---------|-----------|---------------------|------------|
| Price | $15-30/user/month | Free (API only) | Free (API only) |
| Checks | ~50 | No fixed list | **280+** |
| Approach | SaaS | 4 parallel agents | Checklist-based |
| Focus | General | CLAUDE.md compliance | Security, Perf, Quality |
| Confidence scoring | No | Yes (≥80) | Yes (≥70) |
| Git blame analysis | No | Yes | Yes |
| Language-specific | Limited | No | React, TS, Python, Node |
| Privacy | Their servers | Local | Local |

## Installation

### Option 1: Add to your project (recommended)

```bash
mkdir -p .claude/skills/code-review
curl -o .claude/skills/code-review/SKILL.md \
  https://raw.githubusercontent.com/anthroos/claude-code-review-skill/main/SKILL.md
```

### Option 2: Global installation

```bash
mkdir -p ~/.claude/skills/code-review
curl -o ~/.claude/skills/code-review/SKILL.md \
  https://raw.githubusercontent.com/anthroos/claude-code-review-skill/main/SKILL.md
```

## Prerequisites

1. **Claude Code CLI** — [Install here](https://claude.ai/claude-code)
2. **GitHub CLI** (for PR reviews) — `brew install gh && gh auth login`

## Usage

### Full comprehensive review

```bash
claude "full code review"
```

### Review a PR

```bash
claude "review PR 123"
```

### Security-focused review

```bash
claude "security review my changes"
```

### Performance review

```bash
claude "check performance issues in PR 456"
```

### Review and post to GitHub

```bash
claude "review PR 123 and post comments"
```

## What it checks (280+ rules)

### Security (63 checks)
- **Injection** — SQL, NoSQL, Command, LDAP, XPath, Template, Header, Log injection
- **Authentication** — Brute-force, session fixation, weak tokens, MFA
- **Data Exposure** — Hardcoded secrets, secrets in logs, weak crypto, HTTPS
- **XXE** — XML external entities, unsafe deserialization
- **Access Control** — IDOR, privilege escalation, CORS, path traversal
- **Misconfiguration** — Debug mode, default creds, security headers
- **XSS** — Reflected, Stored, DOM-based, CSP, React unsafe patterns
- **Deserialization** — eval(), prototype pollution
- **Dependencies** — Outdated packages, typosquatting
- **Additional** — SSRF, CSRF, JWT issues, ReDoS, file uploads, race conditions

### Bugs & Logic (50 checks)
- **Null/Undefined** — Null dereference, missing checks, falsy confusion
- **Type Issues** — Coercion, implicit conversion, unsafe casts
- **Async** — Missing await, unhandled rejections, race conditions, deadlocks
- **Loops** — Off-by-one, infinite loops, mutation during iteration
- **Edge Cases** — Empty arrays, zero division, overflow, timezone, unicode
- **State** — Stale state, mutations, unnecessary re-renders
- **Error Handling** — Empty catch, generic handling, missing finally
- **Resources** — Leaks (memory, files, connections, timers)
- **Business Logic** — Wrong calculations, missing validation, rollback

### Performance (38 checks)
- **Database** — N+1, missing indexes, SELECT *, pagination, pooling
- **API/Network** — Caching, over/under-fetching, compression, timeouts
- **Frontend** — Bundle size, re-renders, images, lazy loading
- **Algorithms** — O(n²), memoization, data structures
- **Caching** — Cache layer, invalidation, stampede

### Code Quality (36 checks)
- **Readability** — Naming, magic numbers, long functions, nesting
- **Maintainability** — DRY, coupling, abstractions, dead code
- **SOLID** — All 5 principles
- **API Design** — Consistency, HTTP methods, status codes, versioning
- **Configuration** — Hardcoded config, missing defaults, secrets

### Testing (16 checks)
- **Coverage** — Unit, integration, edge cases, error cases
- **Quality** — Flaky tests, speed, interdependence, assertions
- **Patterns** — Organization, naming, AAA, fixtures

### Accessibility (10 checks)
- Alt text, labels, contrast, keyboard, ARIA, focus, headings

### i18n (8 checks)
- Hardcoded strings, date/number/currency formatting, RTL, pluralization

### Documentation (6 checks)
- README, API docs, JSDoc, changelog, setup instructions

### DevOps (10 checks)
- Health checks, graceful shutdown, retry, circuit breaker, observability

### Git (7 checks)
- Large files, secrets in history, merge conflicts, commit messages

### React/Next.js (10 checks)
- useEffect deps, cleanup, stale closures, key props, memo overuse

### TypeScript (7 checks)
- Any abuse, type assertions, missing return types, non-null assertions

### Python (7 checks)
- Mutable defaults, type hints, bare except, context managers

### Node.js/Express (7 checks)
- Async errors, helmet, rate limiting, input validation

### SQL/Database (6 checks)
- Raw queries, missing indexes, N+1 in ORM, migrations

## Key Features

- **Confidence scoring** — Only reports issues with ≥70% confidence, reducing noise
- **Git blame analysis** — Skips pre-existing issues, focuses on new changes
- **Auto-skip logic** — Ignores draft PRs, trivial changes, docs-only updates
- **Language detection** — Applies React/TS/Python/Node checks when relevant

## Example Output

```markdown
## Code Review Summary

**Reviewed:** 5 files, 234 lines changed
**Risk Level:** High

### Critical Issues (2)
1. [src/api/auth.ts:45] **SQL Injection** — User input passed directly to query
   → Use parameterized queries: `db.query('SELECT * FROM users WHERE id = ?', [userId])`

2. [src/utils/crypto.ts:12] **Weak cryptography** — Using MD5 for password hashing
   → Use bcrypt or argon2 instead

### High Priority (3)
1. [src/services/user.ts:78] **IDOR vulnerability** — Missing ownership check
2. [src/api/data.ts:23] **N+1 query** — 50 queries in loop, use JOIN or batch
3. [src/components/Form.tsx:156] **XSS** — dangerouslySetInnerHTML with user content

### Medium Priority (5)
1. [src/utils/helpers.ts:34] **DRY violation** — Duplicate code in 3 places
2. [src/api/users.ts:89] **Missing error handling** — Empty catch block
...

### Good Practices
- Consistent error handling in services/
- Good TypeScript usage with proper types
- Comprehensive test coverage for auth module
```

## Git Hook Integration

Add to `.git/hooks/pre-push`:

```bash
#!/bin/bash
set -e

echo "Running AI code review..."

# Run review and capture output
REVIEW_OUTPUT=$(claude "quick review of staged changes, list only critical issues as bullet points" --print 2>&1) || true

# Check if critical issues were found
if echo "$REVIEW_OUTPUT" | grep -qi "critical\|security\|injection\|vulnerability"; then
  echo ""
  echo "⚠️  Potential critical issues found:"
  echo "$REVIEW_OUTPUT"
  echo ""
  read -p "Push anyway? (y/n) " -n 1 -r
  echo
  if [[ ! $REPLY =~ ^[Yy]$ ]]; then
    echo "Push cancelled."
    exit 1
  fi
fi

echo "✓ Review passed"
```

## CI/CD Integration

### GitHub Actions

```yaml
name: AI Code Review
on: [pull_request]

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Install Claude Code
        run: npm install -g @anthropic-ai/claude-code
      - name: Run Review
        run: claude "review this PR, post comment with findings" --print
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

## Customization

You can modify the SKILL.md to:
- Add company-specific rules
- Remove irrelevant checks (e.g., a11y for backend projects)
- Add framework-specific checks (Next.js, Django, etc.)
- Change severity levels
- Add custom patterns to detect

## Severity Levels

| Level | Action | Examples |
|-------|--------|----------|
| **Critical** | Must fix before merge | SQL injection, hardcoded secrets |
| **High** | Should fix before merge | XSS, N+1 queries, auth bypass |
| **Medium** | Fix soon | DRY violations, missing tests |
| **Low** | Nice to have | Naming, comments |

## License

MIT — use freely, modify as needed.

## Contributing

PRs welcome! Add new checks, improve detection patterns, or add language-specific rules.

## Credits

Built for the Claude Code community.
