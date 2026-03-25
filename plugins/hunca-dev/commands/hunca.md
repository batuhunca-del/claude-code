---
allowed-tools: Bash(git:*), Bash(./scripts/gh.sh:*), Bash(./scripts/edit-issue-labels.sh:*), Bash(./scripts/comment-on-duplicates.sh:*), Bash(npm:*), Bash(npx:*)
description: Hunca's all-in-one workflow command — triage, dedupe, review, commit, or maintain
---

## Context

- Current branch: !`git branch --show-current`
- Git status: !`git status --short`
- Recent commits: !`git log --oneline -5`

## Your Task

You are Hunca's personal workflow assistant. Based on `$ARGUMENTS`, determine and execute the appropriate action:

### Supported Actions

**`triage <issue-number>`** — Triage a GitHub issue:
1. Fetch available labels with `./scripts/gh.sh label list`
2. View the issue and its comments
3. Apply appropriate category and lifecycle labels using `./scripts/edit-issue-labels.sh`
4. Check for duplicates with `./scripts/gh.sh search issues`

**`dedupe <issue-number>`** — Find duplicates for an issue:
1. View the issue and summarize it
2. Search with diverse keywords using parallel agents
3. Filter false positives
4. Post results using `./scripts/comment-on-duplicates.sh`

**`sweep`** — Review and clean up open issues:
1. List recent open issues with `./scripts/gh.sh issue list --state open --limit 20`
2. Identify stale issues, duplicates, and issues needing labels
3. Suggest batch actions

**`ship`** — Commit, push, and create a PR:
1. Review all staged and unstaged changes
2. Create a descriptive commit
3. Push to origin with `-u` flag
4. Create a PR with summary and test plan

**`status`** — Quick project status overview:
1. Show git status, current branch, and recent commits
2. List open PRs and recent issues
3. Summarize any CI failures

**`review <pr-number>`** — Review a pull request:
1. View the PR diff and description
2. Check for code quality, security, and convention adherence
3. Provide actionable feedback

If no action is specified or the argument doesn't match, ask Hunca what they'd like to do and list the available actions.
