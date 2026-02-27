# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

## [1.0.0] - 2026-02-04

### Added
- Initial release with **280+ comprehensive code review checks** across 15 categories
- Support for local, branch, and PR review modes
- **Security** (OWASP Top 10 + extended): injection, auth, data exposure, XXE, access control, XSS, deserialization, SSRF, CSRF, JWT, ReDoS
- **Bugs & Logic**: null handling, type issues, async/concurrency, loops, edge cases, state management, error handling, resource management, business logic
- **Performance**: database (N+1, indexes, pagination), API/network, frontend, algorithms, caching
- **Code Quality**: readability, maintainability, SOLID principles, API design, configuration
- **Testing**: coverage, quality, patterns
- **Accessibility**: WCAG compliance checks
- **i18n**: internationalization issues
- **Documentation**: completeness checks
- **DevOps**: health checks, observability, resilience
- **Git**: version control best practices
- **Language-specific checks** (37 checks):
  - React/Next.js (10): hooks, effects, key props, memo
  - TypeScript (7): any abuse, type assertions, return types
  - Python (7): mutable defaults, type hints, context managers
  - Node.js/Express (7): async errors, helmet, rate limiting
  - SQL/Database (6): raw queries, indexes, N+1, migrations
- **Confidence scoring** — Rate issues 0-100, only report ≥70 confidence
- **Git blame analysis** — Skip pre-existing issues, focus on new changes
- **Auto-skip logic** — Skip draft PRs, trivial changes, docs-only updates
- Focused review modes (`--focus security`, `--focus performance`, etc.)
- Severity levels: Critical, High, Medium, Low
- Line-by-line GitHub comment support via `gh api`
- CI/CD integration examples (GitHub Actions)
- Git hook integration examples
- GitHub CLI integration for posting comments
