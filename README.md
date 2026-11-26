# Dev Toolbox

This directory is a teaching toolbox for helping junior engineers learn effective local working practices, both with and without AI agents. It collects practical notes for command-line tools, platform integrations, development utilities, agent harnesses, browser automation, and the local shell environment.

The guides focus on practical commands, setup details, shortcuts, and working methods that juniors can apply across projects.

## Organization

### `integrations/`

Guides for working with platform tools, including Jira, Azure, JFrog Artifactory, Databricks, Sonar, and Kubernetes tooling.

- [Azure CLI](integrations/azure-cli.md) - Azure accounts, subscriptions, and AKS access
- [Databricks](integrations/databricks.md) - Databricks development workflows
- [GitHub](integrations/github.md) - GitHub CLI commands and workflows
- [JFrog](integrations/jfrog.md) - Artifact and package repository workflows
- [Jira Cloud and TWG](integrations/jiracloud-twg.md) - Work tracking and Jira operations
- [Kubernetes extensions](integrations/kube-extensions.md) - VS Code Kubernetes tooling
- [kubectl commands](integrations/kubectl-commands.md) - Kubernetes inspection and operations
- [Sonar](integrations/sonar.md) - Sonar CLI, analysis, and quality checks

### `commands/`

Short, focused, often one-time CLI commands and recipes for local operations.

- [Command notes](commands/) - Reusable command-line recipes
- [Caffeinate](commands/caffeinate.md) - Keep macOS awake during long-running work
- [History with ripgrep](commands/history-rg.md) - Search shell history with `rg`
- [Jog](commands/jog.md) - Quick local command-line workflow
- [jq](commands/jq.md) - Query and transform JSON from the command line
- [Kill a port](commands/kill-port.md) - Find and stop a process using a port
- [man](commands/man.md) - Read local command and system documentation
- [nvim](commands/nvim.md) - Install and use Neovim for navigation and editing
- [watch](commands/watch.md) - Rerun a command and monitor changing output

### `frameworks/`

Local development frameworks and utilities that structure or support longer-running work.

- [Bruno](frameworks/bruno.md) - API requests and local API exploration
- [Git worktrees](frameworks/git-worktrees.md) - Isolated working directories and branches
- [tmux](frameworks/tmux.md) - Persistent terminal sessions

### `agentic-work/`

Agent harnesses and agent-accessible tools for delegating, inspecting, automating, and reviewing work.

- [Codex CLI](agentic-work/codex.md) - Terminal-based coding-agent workflows
- [Copilot CLI](agentic-work/copilot-cli.md) - Agent-assisted work from the terminal
- [Herdr](agentic-work/herdr.md) - Terminal workspace manager for running and monitoring coding agents
- [Playwright MCP](agentic-work/playwright-mcp.md) - Browser automation through MCP

### `shell/`

Notes about the local shell environment and configuration, including certificates and version managers.

- [Shell notes](shell/) - Shell configuration and environment setup
- [Certificates](shell/certs.md) - Local certificate configuration
- [Homebrew](shell/homebrew.md) - Homebrew setup and package management
- [Version managers](shell/version-managers.md) - Runtime and tool version management
- [zsh](shell/zsh.md) - zsh configuration and usage

### `philosophy/`

Short principles for designing and working with local tools.

- [Unix philosophy](philosophy/unix-philosohy.md) - A skimmable summary of Unix design principles
- [Twelve-Factor App](philosophy/twelve-factor-app.md) - A checklist for building and running software as a service

## Working principles

1. Start with the smallest command or focused note that can answer the question.
2. Keep credentials, tokens, and other secrets out of notes and command history where possible.
3. Treat agent-generated changes like a teammate's change: inspect the diff and run the relevant checks.
4. Prefer explicit commands and documented prerequisites over hidden local state.
5. Keep notes short enough to scan and specific enough to use while working.

The directory name is **dev-toolbox**: it describes both the practical tools and the working methods collected here, without limiting the contents to commands or agent workflows.

## Course slides

- [Dev Toolbox Marp slides](slides/dev-toolbox.md) - One orientation slide per guide, grouped by section

### View in a browser

Install the Marp CLI if needed:

```bash
npm install --global @marp-team/marp-cli
```

From the repository root, start the Marp preview server:

```bash
marp --preview slides/dev-toolbox.md
```

Then open the local URL shown in the terminal, usually `http://localhost:8080`.
