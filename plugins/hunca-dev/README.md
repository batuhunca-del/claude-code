# hunca-dev

Personalized development workflow plugin for Hunca ([@batuhunca-del](https://github.com/batuhunca-del)).

## What's Included

### Skill: `hunca-dev`
Auto-activating skill that provides Claude with context about your development profile, preferred workflows, and coding conventions. It ensures Claude understands your style and optimizes for your common tasks.

### Command: `/hunca`
All-in-one workflow command with these actions:

| Action | Usage | Description |
|--------|-------|-------------|
| `triage` | `/hunca triage 123` | Triage a GitHub issue with labels |
| `dedupe` | `/hunca dedupe 123` | Find duplicate issues |
| `sweep` | `/hunca sweep` | Review and clean up open issues |
| `ship` | `/hunca ship` | Commit, push, and create a PR |
| `status` | `/hunca status` | Quick project status overview |
| `review` | `/hunca review 456` | Review a pull request |

### Agent: `issue-manager`
Specialized Sonnet agent for bulk GitHub issue operations — batch triaging, stale issue cleanup, and duplicate sweeps.

## Installation

This plugin is included in the `plugins/` directory. Configure it in your project's `.claude/settings.json` or install via `/plugin`.
