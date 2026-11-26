---
marp: true
theme: default
paginate: true
header: "Dev Toolbox"
footer: "Local Engineering Course"
style: |
   section { font-size: 26px; }
   h1 { font-size: 1.7em; }
   h2 { font-size: 1.15em; }
---

# Dev Toolbox
### Practical tools and methods for working locally

A course companion for working with and without agents.

<!--
Course note: this deck is an orientation, not a replacement for the guides.
Participants can open the repository for complete commands, setup details, and
working examples.
-->

---

# How to use this toolbox

- Find the section that matches the task.
- Use the slide for the purpose and the linked guide for the details.
- Prefer the smallest focused tool or command that answers the question.
- Review and verify agent-generated changes.

**Six sections:** integrations · commands · frameworks · agentic-work · shell · philosophy

---

# Integrations
### Working with platform tools

Azure, Databricks, JFrog, Jira, Kubernetes, and Sonar.

These guides focus on authentication, repository and work-item operations, clusters, artifacts, CI/CD, and quality gates.

---

# Azure CLI

Install with Homebrew, then authenticate:

```bash
brew install azure-cli
az login
az account set --subscription <subscription-id>
```

Use `az` to:

- Authenticate to Azure and select a tenant.
- List and switch subscriptions.
- Connect to AKS clusters and manage credentials.

[Open the guide](../integrations/azure-cli.md)

---

# Databricks

Install and authenticate the Databricks CLI:

```bash
brew tap databricks/tap && brew install databricks
databricks auth login
databricks workspace list
```

Use the Databricks CLI to:

- Install and authenticate the local client.
- Configure profiles and targets.
- Work with Databricks resources from the terminal.

[Open the guide](../integrations/databricks.md)

---

# JFrog Artifactory

Install and configure JFrog CLI:

```bash
brew install jfrog-cli
jf c add
jf rt ping
```

Use JFrog CLI to:

- Configure Artifactory servers and authentication.
- Search, download, upload, and inspect artifacts.
- Work with package repositories from the command line.

[Open the guide](../integrations/jfrog.md)

---

# Jira Cloud and TWG

Configure the documented Jira/TWG integration, then start with a read-only search or query:

```bash
twg --help
twg jira --help
```

Use the Jira/TWG workflows to:

- Search and inspect Jira work items.
- Update, comment on, transition, and link work.
- Work with connected Atlassian context from the terminal.

[Open the guide](../integrations/jiracloud-twg.md)

---

# Kubernetes extensions

Install the core tools with Homebrew, then verify the active context:

```bash
brew install kubectl fzf
kubectl config current-context
kubectl get namespaces
```

Set up a productive Kubernetes terminal workflow with:

- `kubectl`, Krew, `fzf`, and context/namespace helpers.
- Fast resource inspection, logs, and JSON parsing.
- Tools that make cluster work easier to navigate.

[Open the guide](../integrations/kube-extensions.md)

---

# kubectl commands

Start by checking the context and listing resources:

```bash
kubectl config current-context
kubectl get pods
kubectl logs <pod>
```

A quick reference for:

- Contexts, clusters, namespaces, and resources.
- Pods, deployments, services, and logs.
- Common inspection and operational commands.

[Open the guide](../integrations/kubectl-commands.md)

---

# SonarQube

Install the CLI if needed, then use the documented API checks:

```bash
brew install curl jq
curl --version
jq --version
```

Use the Sonar guide to investigate:

- Build failures and quality gates.
- Rule violations and issue locations.
- Analysis results and remediation work.

[Open the guide](../integrations/sonar.md)

---

# Commands
### Small, focused terminal recipes

These notes are for quick operations that solve one narrow local problem.

---

# caffeinate

Keep macOS awake while long-running work is active:

```bash
caffeinate -dims
caffeinate -dims npm test
```

Stop a standalone session with `Ctrl-c`.

- Tests and builds
- Deployments
- Downloads or other terminal jobs

[Open the guide](../commands/caffeinate.md)

---

# GitHub CLI

Install and authenticate:

```bash
brew install gh
gh auth login
gh repo view
```

Use `gh` to:

- Authenticate and inspect repositories.
- Work with issues, pull requests, and checks.
- Query or automate GitHub workflows from the terminal.

[Open the guide](../integrations/github.md)

---

# Search shell history with `rg`

Search previous commands quickly:

```bash
history | rg 'kubectl|pytest|marp'
```

Recover previous commands quickly by:

- Searching shell history with ripgrep.
- Filtering by command, directory, or tool name.
- Reusing known-good local workflows.

[Open the guide](../commands/history-rg.md)

---

# jog

Run `jog` to see recent commands from the current directory:

```bash
jog
```

Use `jog` to see recent commands run in the current directory.

It is a small local-memory aid for rediscovering how a project was recently built, tested, or operated.

[Open the guide](../commands/jog.md)

---

# jq

Install and try it with a JSON file:

```bash
brew install jq
jq '.items[]' file.json
```

Use `jq` in the terminal to:

- Parse and filter JSON.
- Extract fields from CLI and API output.
- Reshape data for the next command in a pipeline.

[Open the guide](../commands/jq.md)

---

# kill-port

Free a development port:

```bash
kill-port 8080
```

Use the command to:

- Finding the process listening on it.
- Inspecting the process before stopping it.
- Terminating the process that blocks local work.

[Open the guide](../commands/kill-port.md)

---

# watch

Rerun a command and monitor changing output. On macOS, the recommended install is Homebrew:

```bash
brew install watch
```

On Debian/Ubuntu, use `sudo apt install procps`. A source or curl-based install is only useful when a package manager is unavailable; prefer the package manager so upgrades and removal remain managed.

Basic commands:

```bash
watch kubectl get pods
watch -n 2 'kubectl get pods -A'
watch -d 'git status --short'
watch -g 'curl -s http://localhost:8080/health'
```

`-n` sets the interval, `-d` highlights changes, `-g` exits on output change, and `-e` exits on command failure.


[Open the guide](../commands/watch.md)

---

# man

Read the local manual before reaching for a web search:

```bash
man command
man 5 ssh_config
man -k keyword
man -f command
```

Inside a page: `Space` moves forward, `b` moves back, `/text` searches, `n` finds the next match, and `q` quits.

[Open the guide](../commands/man.md)

---

# nvim

Install the recommended way with Homebrew:

```bash
brew install neovim
nvim path/to/file.md
```

Optional `vim` alias in `~/.zshrc`:

```bash
alias vim='nvim'
source ~/.zshrc
vim path/to/file.md
```

If a package manager is unavailable, use the matching official release archive with `curl`; see the [Neovim install guide](https://neovim.io/doc/install/).

Essential navigation and editing:

```text
i       insert
Esc     normal mode
h j k l move
/text   search
n / N   next / previous match
:w      save       :q      quit
```

Use `:help keyword` or `K` for built-in help. Browse plugins and configurations at [Neovimcraft](https://neovimcraft.com/), and use the [Vim cheat sheet](https://vim.rtorr.com/) for more motions.

[Open the guide](../commands/nvim.md)

---

# Frameworks
### Utilities that structure local development

These tools support longer-running, multi-process, or multi-branch work.

---

# Bruno

Use Bruno collections to:

- Keep API requests in the repository.
- Run and inspect API workflows locally.
- Let an agent maintain or use a consistent request collection.

[Open the guide](../frameworks/bruno.md)

---

# Git worktrees

Use worktrees to:

- Work on multiple branches simultaneously.
- Keep each task in an isolated directory.
- Share repository objects without duplicating the full repository.

[Open the guide](../frameworks/git-worktrees.md)

---

# tmux

Use tmux to manage:

- Persistent terminal sessions and panes.
- Long-running servers, tests, and logs.
- Work that should survive disconnects or terminal changes.

[Open the guide](../frameworks/tmux.md)

---

# Agentic Work
### Working with agent harnesses and MCP

Agents can inspect context, make changes, run checks, and automate browser or work-system tasks.

Use them with clear goals, constraints, and review of the result.

---

# Codex CLI

Run an AI coding agent in a project to:

- Inspect and explain a repository.
- Make focused code changes.
- Run tests and review the resulting diff.

[Open the guide](../agentic-work/codex.md)

---

# Copilot CLI

Use Copilot from the terminal for:

- Interactive coding-agent sessions.
- Different modes, agents, and queued prompts.
- Repeatable daily development workflows.

[Open the guide](../agentic-work/copilot-cli.md)

---

# Playwright MCP

Connect an agent to browser automation so it can:

- Navigate and inspect web applications.
- Fill forms and interact with pages.
- Test workflows through an MCP server.

[Open the guide](../agentic-work/playwright-mcp.md)

---

# Shell
### The local environment underneath the tools

Certificates, package managers, runtimes, and zsh configuration determine whether local commands can connect and run correctly.

---

# CA certificates

Configure trusted corporate CA certificates so local tools can:

- Verify internal HTTPS connections.
- Use package managers and APIs safely.
- Avoid confusing TLS failures caused by missing trust configuration.

[Open the guide](../shell/certs.md)

---

# Homebrew

Use Homebrew on macOS or Linux to:

- Install command-line tools.
- Manage local packages and updates.
- Keep development dependencies easy to reproduce.

[Open the guide](../shell/homebrew.md)

---

# Version managers

Use `nvm`, `pyenv`, and `jenv` to:

- Select project-specific Node.js, Python, and Java versions.
- Keep multiple runtime versions on one machine.
- Match local runtimes to repository expectations.

[Open the guide](../shell/version-managers.md)

---

# zsh and Oh My Zsh

Configure the shell you use every day:

- Set up zsh and Oh My Zsh.
- Organize aliases, functions, and environment variables.
- Make the terminal predictable across projects.

[Open the guide](../shell/zsh.md)

---

# Philosophy
### Principles for local tools and services

The toolbox also captures design principles that help us choose and build tools.

---

# Unix Philosophy

The 17 rules from *The Art of Unix Programming* emphasize:

- Small, clear, modular parts.
- Composition through clean interfaces and text streams.
- Transparency, robustness, and least surprise.
- Repair, extensibility, and measured optimization.

The shorter “do one thing well” principles come from Doug McIlroy, quoted in the same chapter.

[Open the guide](../philosophy/unix-philosohy.md)

---

# Twelve-Factor App

A checklist for service-oriented applications:

- One codebase, explicit dependencies, and environment configuration.
- Separated build, release, and run stages.
- Stateless processes, attached services, and port binding.
- Operational parity, event-stream logs, and one-off admin tasks.

[Open the guide](../philosophy/twelve-factor-app.md)

---

# Start here

1. Clone the toolbox:

   `git clone git@github.com:ronenlh/toolbox.git`

2. Pick a guide for the task in front of you.
3. Run the smallest useful command.
4. Inspect the output, diff, and environment when the task is complete.

**The repository is the course material.** Keep it open while working through the examples.
