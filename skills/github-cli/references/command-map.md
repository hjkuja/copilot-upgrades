# Command Map

Use this as the quick routing guide for `gh`.

| Command family | Scope | Best for | Notes |
|---|---|---|---|
| `gh pr` | Primary | Create, list, view, edit, review, and check out pull requests | Start here for almost all PR work |
| `gh issue` | Primary | Create, list, view, edit, comment on, close, and reopen issues | Prefer this over `gh api` for issue work |
| `gh release` | Primary | Create, inspect, and manage releases and assets | Use `--generate-notes` or explicit notes |
| `gh repo` | Primary | Clone, view, create, fork, and inspect repositories | Use explicit `OWNER/REPO` when outside a repo |
| `gh search prs` | Primary | Cross-repository PR discovery | Better than `gh pr list` for broad searches |
| `gh search issues` | Primary | Cross-repository issue discovery | Better than `gh issue list` for broad searches |
| `gh browse` | Primary | Jump from terminal to the GitHub web UI | Use `--no-browser` to print the URL only |
| `gh status` | Primary | Personal GitHub work queue across repos | Good for assigned work and mentions |
| `gh workflow` | Secondary | List and trigger `workflow_dispatch` workflows | Only use `run` when the workflow supports manual dispatch |
| `gh run` | Secondary | Inspect workflow runs and logs | Prefer `--log-failed` before full logs |
| `gh auth status` | Secondary | Check whether `gh` is authenticated | Never add `--show-token` |
| `gh config get` | Secondary | Inspect local GitHub CLI config | Safe diagnostic command |
| `gh config set` | Explicit-only | Change local GitHub CLI behavior | Only on direct user request |
| `gh auth login/logout/refresh/setup-git/switch` | Explicit-only | Change auth state or credential-helper setup | Use only when the user explicitly wants auth changes |
| `gh api` | Limited fallback | Narrow issue, PR, release, or repo gaps not covered by high-level commands | Not a general default |
| `gh secret`, `gh variable`, `gh ssh-key`, `gh gpg-key`, `gh repo deploy-key` | Excluded | Sensitive-data or credential-management surfaces | Do not use in this skill |

## Selection shortcuts

- Same repository listing or filtering: prefer `gh pr list` or `gh issue list`.
- Cross-repository discovery: prefer `gh search prs` or `gh search issues`.
- Browser navigation: prefer `gh browse` or `--web`.
- Structured terminal output: prefer `--json`, then narrow with `--jq` or `--template`.
