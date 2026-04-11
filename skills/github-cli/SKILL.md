---
name: github-cli
description: 'Safe, practical GitHub CLI guidance for pull requests, issues, releases, repository operations, search, browsing, and selected Actions workflows. Use this when the user wants to use the gh command for normal GitHub work while avoiding tokens, secrets, variables, keys, and broad API access.'
---

# GitHub CLI

Use `gh` for day-to-day GitHub workflows. Prefer the dedicated high-level command families over generic API calls.

Detailed reference material lives in the `references/` folder - load on demand.

---

## References

| Reference | When to load |
|---|---|
| [Command Map](references/command-map.md) | Choosing the right `gh` command family quickly |
| [Core Workflows](references/core-workflows.md) | Exact commands for PRs, issues, releases, repos, search, browse, and Actions |
| [Safety Boundaries](references/safety-boundaries.md) | Exclusions, confirmation rules, and sensitive-data constraints |

---

## 1. Default Operating Rules

1. Confirm repository context first. If not already in the right repository, use `-R owner/repo`.
2. Prefer dedicated commands like `gh pr`, `gh issue`, `gh release`, `gh repo`, `gh search`, `gh workflow`, and `gh run` over `gh api`.
3. Use non-interactive flags when the user's intent is explicit. Use interactive flows when the user wants hands-on CLI guidance or key details are missing.
4. Prefer structured output with `--json`, `--jq`, or `--template` when extracting fields or summarizing many results.
5. After mutating commands, report the resulting PR, issue, release, or repository URL or identifier when available.

## 2. Safe Defaults

- Safe primary areas:
  - PRs: create, list, view, checkout, edit, comment, review, checks, status
  - Issues: create, list, view, edit, comment, close, reopen
  - Releases: create, list, view, edit, upload when explicitly requested
  - Repositories: clone, view, create, fork, list, set default repository
  - Discovery: search, browse, status
  - Actions: workflow list, workflow run, run list, run view, run watch
  - Auth and config diagnostics: `gh auth status`, `gh config get`, limited `gh config set`
- Sensitive areas are excluded; see [Safety Boundaries](references/safety-boundaries.md).

## 3. Command Selection Heuristics

| User intent | Prefer |
|---|---|
| Open, create, edit, review, or check out a PR | `gh pr ...` |
| Add, edit, list, or comment on an issue | `gh issue ...` |
| Create or inspect a release | `gh release ...` |
| Clone, create, fork, or inspect a repository | `gh repo ...` |
| Find PRs or issues across repositories | `gh search prs` / `gh search issues` |
| Open GitHub pages in a browser | `gh browse` |
| Check personal GitHub work queues | `gh status` |
| Trigger or inspect Actions workflows and runs | `gh workflow ...` / `gh run ...` |
| Check whether `gh` is authenticated | `gh auth status` |

## 4. Common Workflows

### Pull requests

```bash
gh pr list --state open --limit 20
gh pr checkout 123
gh pr create --base main --title "..." --body "..."
gh pr edit 123 --add-reviewer octo-user --add-label enhancement
```

### Issues

```bash
gh issue list --state open --assignee "@me"
gh issue create --title "..." --body "..." --label bug
gh issue edit 123 --add-assignee "@me" --milestone "vNext"
```

### Releases

```bash
gh release list --exclude-drafts
gh release view v1.2.3
gh release create v1.2.3 --generate-notes
```

### Repositories

```bash
gh repo clone owner/repo
gh repo view owner/repo --web
gh repo create my-project --private --source=. --remote=origin
gh repo fork owner/repo --clone
```

### Search and browse

```bash
gh search prs --repo owner/repo --review-requested=@me --state open
gh search issues --owner my-org --label bug --state open
gh browse 123
gh browse --releases
```

### Actions

```bash
gh workflow list
gh workflow run ci.yml --ref main
gh run view 12345 --log-failed
```

## 5. Interaction Model

- If the user asks for a normal GitHub workflow, stay in the high-level `gh` commands.
- If the task is destructive or difficult to undo, require clear explicit intent before running it.
- If a workflow needs elevated scopes such as `project`, mention that `gh auth refresh -s project` may be needed, but do not default to auth mutations unless the user asks.
