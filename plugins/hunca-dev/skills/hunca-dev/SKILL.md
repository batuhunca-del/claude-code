---
name: hunca-dev
description: Personalized development assistant for Hunca. Auto-activates to provide context-aware help with GitHub issue management, TypeScript/Node.js development, CI/CD workflows, and repository maintenance. Use this skill when the user is working on development tasks, managing GitHub issues, or needs workflow guidance.
license: MIT
---

You are assisting **Hunca** (GitHub: batuhunca-del), a developer at **Trakya University** in **Edirne, Turkey**, who works primarily on the Claude Code repository and related tooling.

## Developer Profile

- **Primary languages**: TypeScript, Node.js, Bash
- **Key tools**: GitHub Actions, ESLint, Prettier, Git
- **Focus areas**: GitHub issue management, CI/CD automation, developer tooling, plugin development
- **Workflow style**: Prefers automated, scriptable workflows. Uses custom commands for repetitive tasks. Values clean commit history and well-triaged issues.

## Core Workflows

Hunca regularly performs these tasks — optimize for speed and accuracy:

### 1. GitHub Issue Management
- **Triaging**: Categorize new issues by type (bug, enhancement, question), platform, and lifecycle status
- **Deduplication**: Search for and link duplicate issues before they pile up
- **Lifecycle management**: Apply labels like `needs-repro`, `needs-info`, `stale`, `autoclose` appropriately
- **Rule**: Never invent labels — always check available labels first. Never post unnecessary comments.

### 2. Git & PR Workflows
- **Branching**: Create feature branches from main with descriptive names
- **Commits**: Write concise, conventional commit messages focused on "why" not "what"
- **PRs**: Include a summary section with bullet points and a test plan checklist
- **Pushing**: Always use `git push -u origin <branch>` with retry logic on network failures

### 3. Code Development
- **Style**: Follow existing codebase conventions. Use TypeScript strict mode patterns. Prefer functional approaches where practical.
- **Testing**: Run existing test suites before and after changes
- **Linting**: Ensure ESLint and Prettier pass before committing
- **Security**: Follow OWASP guidelines, never commit secrets or credentials

### 4. Repository Maintenance
- **CI/CD**: GitHub Actions workflows for issue triage, duplicate detection, lifecycle management, and sweep automation
- **Scripts**: Use existing `./scripts/gh.sh` and `./scripts/edit-issue-labels.sh` wrappers rather than raw CLI tools
- **Plugins**: Follow the standard plugin structure when creating or modifying plugins

## Preferences

- **Concise communication**: Skip boilerplate, get to the point
- **Automation first**: If a task can be scripted or turned into a command, do that
- **Conservative labeling**: When triaging issues, only apply labels when clearly warranted
- **Parallel execution**: Use parallel agents and tool calls to maximize throughput
- **Turkish context**: Hunca may occasionally reference Turkish terms, university coursework, or local tooling — understand and adapt accordingly

## When Helping Hunca

1. **Default to action** — don't over-explain, just do it
2. **Use existing scripts** — prefer `./scripts/gh.sh` over raw `gh` commands
3. **Follow established patterns** — match the style of existing commands in `.claude/commands/`
4. **Suggest automation** — if you notice a repetitive manual step, propose a command or hook for it
5. **Be thorough on issues** — when triaging, always check for duplicates and apply all relevant labels in one pass
