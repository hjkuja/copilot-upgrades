# Core Workflows

These are the normal, high-value `gh` workflows to prefer.

## 1. Establish repository context

Use the current directory when it is already the target repository. Otherwise pass `-R owner/repo`.

```bash
gh repo view
gh repo view owner/repo
gh repo clone owner/repo
```

Useful pattern for automation or summaries:

```bash
gh repo view owner/repo --json nameWithOwner,description,url,defaultBranchRef
```

## 2. Pull requests

### List and filter

```bash
gh pr list --state open --limit 20
gh pr list --author "@me" --search "status:success review:required"
```

Use `gh search prs` when the user wants results across repositories or broader search qualifiers.

```bash
gh search prs --owner my-org --review-requested=@me --state open
gh search prs --repo owner/repo --label bug --state open
```

### Create

```bash
gh pr create --base main --title "Add release automation" --body "Summary and testing notes"
gh pr create --fill
gh pr create --draft --web
```

Notes:
- `gh pr create` may prompt to push the branch or create a fork.
- Use `--head` when the user wants to control the source branch explicitly.
- Mention issue linkage with body text like `Fixes #123` when relevant.

### Inspect and check out

```bash
gh pr view 123
gh pr checks 123
gh pr checkout 123
gh pr checkout 123 --branch fix-123-local
```

Only use `gh pr checkout --force` when the user explicitly wants the local branch reset.

### Edit

```bash
gh pr edit 123 --title "Improve release automation"
gh pr edit 123 --add-reviewer octo-user --add-label enhancement
gh pr edit 123 --body-file pr-body.md
```

## 3. Issues

### List and filter

```bash
gh issue list --state open --assignee "@me"
gh issue list --label bug --label "help wanted"
gh issue list --search "error no:assignee sort:created-asc"
```

Use `gh search issues` when the search needs to span repositories.

```bash
gh search issues --owner my-org --label bug --state open
gh search issues "broken feature" --repo owner/repo
```

### Create

```bash
gh issue create --title "Release workflow fails on tags" --body "Observed behavior and repro"
gh issue create --label bug --assignee "@me"
gh issue create --template "Bug Report"
```

### Edit

```bash
gh issue edit 123 --title "Release workflow fails on tag pushes"
gh issue edit 123 --add-assignee "@me" --add-label bug
gh issue edit 123 --milestone "vNext"
```

If the user wants issue or PR project assignment, mention that `gh auth refresh -s project` may be required.

## 4. Releases

### Inspect releases

```bash
gh release list --exclude-drafts
gh release view
gh release view v1.2.3 --json tagName,name,isDraft,isPrerelease,url
```

### Create releases

```bash
gh release create v1.2.3 --generate-notes
gh release create v1.2.3 --notes "Bugfix release"
gh release create v1.2.3 -F release-notes.md
gh release create v1.2.3 --draft --prerelease
```

Use these options deliberately:
- `--verify-tag` when the tag must already exist remotely
- `--notes-from-tag` when the tag annotation should become the release notes
- asset upload arguments when the user explicitly wants binaries attached

## 5. Repository operations

```bash
gh repo clone owner/repo
gh repo view owner/repo --web
gh repo create my-project --private --source=. --remote=origin
gh repo fork owner/repo --clone
gh repo set-default owner/repo
```

Notes:
- `gh repo clone` respects the configured `git_protocol`.
- `gh repo fork` may rename remotes and set `upstream`.

## 6. Search, browse, and status

```bash
gh browse
gh browse 217
gh browse src/main.go:312 --blame
gh status
gh status -o my-org
```

Use `gh browse --no-browser` when the user wants the URL without opening a browser.

## 7. Actions

```bash
gh workflow list
gh workflow run ci.yml --ref main
gh workflow run deploy.yml -f environment=staging
gh run view 12345
gh run view 12345 --log-failed
```

Notes:
- `gh workflow run` only works for workflows that support `workflow_dispatch`.
- Prefer `gh run view --log-failed` before full logs to keep output smaller.

## 8. Output shaping

Prefer structured output for multi-item results.

```bash
gh pr list --json number,title,url --jq '.[] | {number, title, url}'
gh issue list --json number,title,labels,url
gh release list --json tagName,name,publishedAt
```
