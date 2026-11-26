# Git worktrees quick guide

Git worktrees let you check out multiple branches from one repository at the same time. They are useful when you need to work on a feature while keeping another branch available for reviews, fixes, or tests.

## Create a worktree with a new branch

From your main repository:

```bash
git worktree add -b feature/my-change ../repo-my-change
cd ../repo-my-change
```

This creates the branch `feature/my-change` and checks it out in the sibling directory `repo-my-change`.

## Create a worktree for an existing branch

```bash
git worktree add ../repo-hotfix hotfix/production
```

A branch normally cannot be checked out in two worktrees simultaneously.

## List worktrees

```bash
git worktree list
```

For machine-readable details:

```bash
git worktree list --porcelain
```

## Work normally

Each worktree has its own checked-out files and branch, while sharing the repository's Git history and object database. Inside a worktree, use normal Git commands:

```bash
git status
git add .
git commit -m "Describe the change"
git push -u origin feature/my-change
```

Uncommitted changes are separate between worktrees, but fetched commits and repository configuration are generally shared.

## Remove a worktree

First make sure important changes are committed or saved, then return to another worktree:

```bash
cd /path/to/main-repository
git worktree remove ../repo-my-change
```

Delete the branch separately after it is merged:

```bash
git branch -d feature/my-change
```

Git refuses to remove a worktree with uncommitted changes unless you use `--force`. Review the worktree carefully before forcing removal.

## Clean stale metadata

If a worktree directory was deleted manually, clean its stale registration:

```bash
git worktree prune
```

Prefer `git worktree remove` over manually deleting its directory.

## Lock a long-lived worktree

Lock a worktree stored on removable or occasionally unavailable storage so Git does not prune it:

```bash
git worktree lock --reason "External drive" /path/to/worktree
git worktree unlock /path/to/worktree
```

## Practical layout

Keep related worktrees next to the main repository and name directories after their purpose:

```text
projects/
├── my-app/             # Main worktree
├── my-app-feature-x/   # Feature branch
└── my-app-hotfix/      # Hotfix branch
```

Official documentation: <https://git-scm.com/docs/git-worktree>
