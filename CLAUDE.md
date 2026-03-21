# CLAUDE.md - Claude Code Repository Guide

## Project Overview

Claude Code is Anthropic's agentic coding tool that lives in the terminal, understands codebases, and helps developers through natural language. This repository contains the **plugin ecosystem**, examples, automation scripts, and development tooling — not the core CLI binary itself.

## Repository Structure

```
.
├── plugins/              # 13 bundled plugins (commands, agents, skills, hooks)
├── examples/             # Configuration examples (settings, hooks)
├── scripts/              # GitHub issue automation (TypeScript + Bash)
├── Script/               # PowerShell dev container launcher (Windows)
├── .claude/              # Project-level Claude commands
├── .claude-plugin/       # Plugin marketplace registry (marketplace.json)
├── .github/workflows/    # 12 GitHub Actions (CI, issue triage, dedup, lifecycle)
├── .devcontainer/        # Dev container config (Node 20, zsh, Claude Code)
├── .vscode/              # VS Code extension recommendations
├── CHANGELOG.md          # Version history (current: 2.1.63)
├── README.md             # Installation and usage guide
├── SECURITY.md           # HackerOne vulnerability disclosure
└── LICENSE.md            # MIT License (Anthropic PBC)
```

## Plugin System

### Standard Plugin Structure

```
plugins/<plugin-name>/
├── .claude-plugin/plugin.json   # Plugin metadata (name, version, description)
├── commands/                    # Slash commands (*.md with YAML frontmatter)
├── agents/                      # Specialized agents (*.md with frontmatter)
├── skills/                      # Progressive-disclosure skills (SKILL.md + resources/)
├── hooks/                       # Event hooks (hooks.json + Python/Bash scripts)
└── README.md                    # Plugin documentation
```

### Bundled Plugins (13 total)

| Plugin | Category | Purpose |
|---|---|---|
| agent-sdk-dev | development | Agent SDK scaffolding & verification |
| claude-opus-4-5-migration | development | Model migration automation |
| code-review | productivity | PR review with 5 specialized agents |
| commit-commands | productivity | Git workflow (/commit, /commit-push-pr, /clean_gone) |
| explanatory-output-style | learning | Educational implementation insights |
| feature-dev | development | 7-phase structured feature development |
| frontend-design | development | Production frontend design guidance |
| hookify | productivity | Custom hook creation from patterns |
| learning-output-style | learning | Interactive learning mode |
| plugin-dev | development | Plugin development toolkit (7 skills, 3 agents) |
| pr-review-toolkit | productivity | Multi-agent PR review (6 agents) |
| ralph-wiggum | development | Autonomous iteration loops |
| security-guidance | security | Dangerous pattern detection hooks |

### Plugin Registry

All plugins are registered in `.claude-plugin/marketplace.json` with schema, metadata, source paths, and categories.

## Key File Formats

### Commands (`commands/*.md`)

```markdown
---
description: What the command does
argument-hint: <optional-args>
allowed-tools: ["Read", "Write", "Bash"]
---

# Prompt content — instructions TO Claude, not to the user
```

### Agents (`agents/*.md`)

```markdown
---
name: agent-name
description: When to use this agent
model: claude-opus-4-5-sonnet  # optional
tools: [Read, Write, Bash]     # optional
---

# System Prompt
Full behavioral instructions for the agent...
```

### Skills (`SKILL.md` + resources)

```markdown
---
name: Skill Name
description: Trigger phrases for auto-loading
version: 1.0.0
---

# Core content (1,200-2,000 words)
Progressive disclosure: lean core + detailed references/examples in subdirectories
```

### Hooks (`hooks/hooks.json`)

```json
{
  "description": "Hook purpose",
  "hooks": {
    "PreToolUse|PostToolUse|Stop|SessionStart|UserPromptSubmit|PreCompact": [{
      "type": "command",
      "command": "python3 ${CLAUDE_PLUGIN_ROOT}/hooks/script.py",
      "matcher": "Bash|Edit|Write"
    }]
  }
}
```

**Hook exit codes**: 0 = success, 1 = show stderr only, 2 = block the operation.

## Naming Conventions

- **Plugins**: `kebab-case` directory names matching `plugin.json` "name"
- **Commands**: `kebab-case.md` (e.g., `commit-push-pr.md`)
- **Agents**: `lowercase-with-hyphens.md` (e.g., `code-architect.md`)
- **Skills**: `SKILL.md` as entry point, descriptive lowercase references
- **Hook scripts**: `snake_case.py` or `kebab-case.sh`

## Development Environment

### Dev Container (recommended)

- **Base**: Node.js 20
- **Tools**: git, zsh, fzf, gh (GitHub CLI), jq, git-delta
- **Memory**: NODE_OPTIONS=--max-old-space-size=4096
- **Shell**: zsh with Powerline10k
- **Non-root user**: `node`

### Required Tools

- Node.js 20+ (runtime)
- Python 3.7+ (hooks)
- Bash 4+ (scripts)
- Git
- GitHub CLI (`gh`) for PR workflows

### VS Code Extensions

- `anthropic.claude-code` — Claude Code
- `dbaeumer.vscode-eslint` — ESLint
- `esbenp.prettier-vscode` — Prettier (default formatter, format on save)
- `eamodio.gitlens` — Git history/blame
- `ms-vscode-remote.remote-containers` — Dev containers

## Git Conventions

- **Line endings**: LF everywhere (`* text=auto eol=lf` in `.gitattributes`)
- **Branches**: Feature branches with descriptive names
- **Commits**: Descriptive messages; use `/commit` or `/commit-push-pr` commands
- **Ignored**: Only `.DS_Store` in `.gitignore`

## GitHub Automation

12 workflows handle issue lifecycle:

- **claude.yml** — Main CI/CD
- **claude-issue-triage.yml** — Auto-triage new issues
- **claude-dedupe-issues.yml** — Duplicate detection
- **auto-close-duplicates.yml** — Close confirmed duplicates
- **lock-closed-issues.yml** — Lock stale closed issues
- **issue-lifecycle-comment.yml** — Lifecycle notifications
- **non-write-users-check.yml** — Permission validation

Scripts in `scripts/` support these workflows (TypeScript for complex logic, Bash for simple ops).

## Common Development Workflows

### Creating a New Plugin

```
/plugin-dev:create-plugin
```
Follows: Discovery -> Planning -> Design -> Structure -> Implementation -> Validation -> Testing -> Docs

### Feature Development

```
/feature-dev "Description"
```
Follows: Discovery -> Exploration -> Clarification -> Design -> Implementation -> Review -> Summary

### Code Review

```
/code-review              # Local review
/code-review --comment    # Post as PR comment
```

### Git Workflow

```
/commit                   # Stage and commit
/commit-push-pr           # Commit + push + open PR
/clean_gone               # Clean up stale local branches
```

## Key Design Principles

1. **Progressive disclosure** — Lean metadata always visible; full docs loaded only when triggered
2. **Agent-first** — Specialized agents for distinct tasks (review, architecture, exploration)
3. **Markdown as config** — Human-readable, version-controllable, no compilation needed
4. **Portable paths** — Always use `${CLAUDE_PLUGIN_ROOT}` in hooks for cross-platform compatibility
5. **Security by default** — Hooks validate inputs; confidence scoring filters false positives
6. **No build step** — All plugin files are interpreted at runtime; changes take effect immediately

## Writing Plugin Content

- Command/agent/skill content is instructions **to Claude**, not messages to the user
- Use imperative voice: "Analyze the code...", "Generate a report..."
- Include strong trigger phrases in skill descriptions for reliable auto-loading
- Keep SKILL.md core to 1,200-2,000 words; put details in `resources/references/`
- Provide working code examples in `resources/examples/`
- Specify `allowed-tools` in commands to limit scope appropriately
