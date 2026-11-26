# GitHub CLI quick guide

This guide covers setting up GitHub CLI and common PR/build workflows.

## Install GitHub CLI

On macOS:

```bash
brew install gh
```

Check the installed version:

```bash
gh --version
```

Current local version:

```text
gh version 2.98.0
```

## Authenticate

Log in:

```bash
gh auth login
```

Recommended choices:

```text
GitHub.com
SSH
Login with browser
```

Check auth status:

```bash
gh auth status
```

Your local setup is authenticated to `github.com` as:

```text
<your GitHub username>
```

## Useful aliases

Add to `~/.zshrc`:

```bash
alias ghs='gh auth status'
alias ghpr='gh pr view --web'
alias ghprs='gh pr list'
alias ghco='gh pr checkout'
alias ghci='gh run list'
```

Reload:

```bash
source ~/.zshrc
```

## Repository basics

View the current repository:

```bash
gh repo view
```

Open the repository in the browser:

```bash
gh repo view --web
```

Clone a repository:

```bash
gh repo clone OWNER/REPO
```

Fork a repository:

```bash
gh repo fork OWNER/REPO
```

## Pull requests

List open PRs:

```bash
gh pr list
```

List your PRs:

```bash
gh pr list --author @me
```

View the current branch PR:

```bash
gh pr view
```

Open the current branch PR in the browser:

```bash
gh pr view --web
```

Check out a PR locally:

```bash
gh pr checkout <pr-number>
```

Create a PR:

```bash
gh pr create --fill
```

Edit a PR:

```bash
gh pr edit <pr-number> --title "PROJ-1234 short title"
gh pr edit <pr-number> --body-file pr-description.md
```

Merge a PR:

```bash
gh pr merge <pr-number> --squash --delete-branch
```

## Check comments on your PR

View PR discussion and review comments:

```bash
gh pr view <pr-number> --comments
```

View detailed review data as JSON:

```bash
gh pr view <pr-number> --json comments,reviews,reviewThreads
```

Show unresolved review threads:

```bash
gh pr view <pr-number> --json reviewThreads \
  | jq -r '.reviewThreads[] | select(.isResolved == false) | .comments[] | [.path, .line, .body] | @tsv'
```

After addressing comments, push the branch:

```bash
git status
git add <files>
git commit -m "PROJ-1234 address PR feedback"
git push
```

Do not amend or force-push unless that is explicitly intended.

## GitHub Actions / builds

List recent workflow runs:

```bash
gh run list
```

List runs for the current branch:

```bash
gh run list --branch "$(git branch --show-current)"
```

Watch the latest run:

```bash
gh run watch
```

View details for a run:

```bash
gh run view <run-id>
```

View failed logs:

```bash
gh run view <run-id> --log-failed
```

Download logs:

```bash
gh run download <run-id>
```

Rerun failed jobs:

```bash
gh run rerun <run-id> --failed
```

## Common PR workflow

Check branch and status:

```bash
git branch --show-current
git status
```

Push your branch:

```bash
git push -u origin "$(git branch --show-current)"
```

Create/open the PR:

```bash
gh pr create --fill
gh pr view --web
```

Check build status:

```bash
gh pr checks
gh run list --branch "$(git branch --show-current)"
```

Read failed build logs:

```bash
gh run view <run-id> --log-failed
```

Check PR comments:

```bash
gh pr view --comments
```

Address feedback, commit, and push:

```bash
git add <files>
git commit -m "PROJ-1234 address feedback"
git push
```

## Build and deploy workflow

Check local state:

```bash
git status
git branch --show-current
```

Run the smallest relevant local test/lint/build command for your change.

Push:

```bash
git push
```

Watch CI:

```bash
gh run list --branch "$(git branch --show-current)"
gh run watch
```

If CI fails:

```bash
gh run view <run-id> --log-failed
```

If Sonar fails:

See `github-sonar.md` for the SonarQube skill/API workflow.

After fixing:

```bash
git add <files>
git commit -m "PROJ-1234 fix build issues"
git push
gh run watch
```

## Handy combined checks

Current PR checks:

```bash
gh pr checks
```

Current PR comments:

```bash
gh pr view --comments
```

Current branch runs:

```bash
gh run list --branch "$(git branch --show-current)"
```

Failed logs from a run:

```bash
gh run view <run-id> --log-failed
```

Open PR in browser:

```bash
gh pr view --web
```
