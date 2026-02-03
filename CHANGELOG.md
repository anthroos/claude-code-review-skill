# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

## [1.1.0] - 2026-02-04

### Added
- Expanded from ~20 checks to **240+ comprehensive checks**
- 10 review categories: Security, Bugs, Performance, Code Quality, Testing, Accessibility, i18n, Documentation, DevOps, Git
- Focused review modes (`--focus security`, `--focus performance`, etc.)
- Severity levels: Critical, High, Medium, Low
- Detailed examples for each check category
- Line-by-line GitHub comment support via `gh api`
- CI/CD integration examples (GitHub Actions)
- Git hook integration examples

### Changed
- Restructured SKILL.md with numbered sections
- Improved output format with risk level and issue counts
- Better troubleshooting section

## [1.0.0] - 2026-02-04

### Added
- Initial release
- Basic code review skill for Claude Code CLI
- Support for local, branch, and PR review modes
- Basic security, bugs, performance, and code quality checks
- GitHub CLI integration for posting comments
