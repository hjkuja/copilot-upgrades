# Safety Boundaries

This skill is intentionally conservative around credentials, secrets, and broad API access.

## Hard exclusions

Do not use any of the following through this skill:

- `gh auth token`
- `gh auth status --show-token`
- any `gh auth status --json ... --show-token` combination
- all `gh secret` commands
- all `gh variable` commands
- all `gh ssh-key` commands
- all `gh gpg-key` commands
- all `gh repo deploy-key` commands
- generic `gh api` as a broad fallback for arbitrary endpoints

Why:
- tokens are directly exposed by `gh auth token` and `--show-token`
- variables can expose plain values
- secrets and key-management surfaces are sensitive even when they only reveal metadata
- broad `gh api` usage makes it too easy to drift into sensitive or overpowered endpoints

## Limited auth and config commands

Safe by default:

- `gh auth status` without `--show-token`
- `gh config get`

Explicit-only:

- `gh auth login`
- `gh auth logout`
- `gh auth refresh`
- `gh auth setup-git`
- `gh auth switch`
- `gh config set`

Use these only when the user explicitly asks to change auth state, scopes, credential-helper behavior, or local GitHub CLI configuration.

## Confirmation-required actions

Require clear explicit intent before running actions that are destructive, forceful, or hard to undo, including:

- repository deletion, archiving, unarchiving, renaming, or visibility changes
- issue deletion or transfer
- release deletion
- workflow disabling
- run cancellation or deletion
- forceful checkout behavior such as `gh pr checkout --force`
- PR merge, revert, or close when the user has not clearly asked for that outcome

## Narrow `gh api` fallback

If a high-level command truly cannot do the requested issue, PR, release, or repository task:

1. Prefer the dedicated `gh` subcommand first.
2. Use `gh api` only for the narrow missing gap.
3. Keep the endpoint scope limited to normal repository, PR, issue, or release workflows.
4. Trim output aggressively with `--jq` or equivalent shaping.
5. Never use it for auth, secrets, variables, keys, or broad account and admin discovery.

If the task starts to depend on broad `gh api` usage, treat it as out of scope for this skill and ask before proceeding.
