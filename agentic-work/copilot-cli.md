# GitHub Copilot CLI quick guide

This guide covers the interactive GitHub Copilot CLI: switching modes, managing running work, enqueueing prompts, using agents, and common daily commands.

## Start and exit

Start Copilot CLI from a project directory:

```bash
copilot
```

Show help:

```text
/help
```

Quick help:

```text
?
```

Exit:

```text
/exit
```

Or:

```text
Ctrl-c Ctrl-c
```

Shutdown:

```text
Ctrl-d
```

## Basic input shortcuts

| Shortcut | Action |
|---|---|
| `Shift+Enter` | Insert newline |
| `Ctrl+g` | Edit the prompt in `$EDITOR` |
| `Ctrl+a` | Move to start of line |
| `Ctrl+e` | Move to end of line |
| `Ctrl+w` | Delete previous word |
| `Ctrl+u` | Delete from cursor to beginning |
| `Ctrl+k` | Delete from cursor to end |
| `Ctrl+r` | Reverse-search prompt history |
| `Ctrl+l` | Clear screen |
| `Ctrl+c` | Cancel current input/action |

## Switch modes

Cycle modes:

```text
Shift+Tab
```

Useful mode-related commands:

```text
/model
/permissions
/plan
/autopilot
```

Select the AI model for the current session:

```text
/model
```

Use automatic model selection:

```text
/model auto
```

Create a plan before coding:

```text
/plan
```

Switch permission mode:

```text
/permissions
```

Allow all tools, paths, and URLs for the session:

```text
/allow-all
```

## Enqueue, stash, and background prompts

Enqueue the current prompt so it runs after the current work finishes:

```text
Ctrl+q
```

Stash or pop the current prompt:

```text
Ctrl+s
```

Move the current task to the background:

```text
Ctrl+x b
```

This is useful when a long-running command, agent task, build, or investigation is still running and you want to continue using the CLI.

## See running processes and tasks

View and manage running shell commands, subagents, and tasks:

```text
/tasks
```

Use this when you want to check what is still running, inspect task output, or manage background work.

## Run shell commands

Run a shell command from inside Copilot CLI:

```text
! git status
! pytest tests/some_test.py -q
! gh pr view --comments
```

For long-running builds/tests, let Copilot run them as tasks so they can be tracked with:

```text
/tasks
```

## Mention files, issues, and PRs

Mention files:

```text
@README.md
@src/example.py
```

Mention GitHub issues or pull requests:

```text
#1234
```

For another repository, use the full form:

```text
owner/repo#1234
```

## Agents and skills

Browse/select agents:

```text
/agent
```

Select a specific agent:

```text
/agent <name>
```

Manage skills:

```text
/skills
```

Run a code review agent on current changes:

```text
/review
```

Run a security review on staged/unstaged changes:

```text
/security-review
```

Get independent critique of current work:

```text
/rubber-duck
```

Enable fleet mode for parallel subagent execution:

```text
/fleet
```

## Work with PRs and code changes

Review current directory changes:

```text
/diff
```

Operate on PRs for the current branch:

```text
/pr
```

Ask Copilot to delegate the session to GitHub and create a PR:

```text
/delegate
```

Choose a base branch:

```text
/delegate --base main
```

## Session management

Start a new conversation:

```text
/new
```

Resume another session:

```text
/resume
```

Rename the current session:

```text
/rename
```

Fork the current session into a new one:

```text
/fork
```

Show token/context usage:

```text
/context
```

Compact conversation history:

```text
/compact
```

Search the conversation timeline:

```text
/search
```

Rewind the last turn and revert file changes:

```text
/rewind
```

Copy the last response:

```text
/copy
```

## Environment and configuration

Show loaded environment details:

```text
/env
```

Show current working directory or change it:

```text
/cwd
/cwd /path/to/repo
```

Add an allowed directory:

```text
/add-dir /path/to/repo
```

List allowed directories:

```text
/list-dirs
```

Manage MCP servers:

```text
/mcp
```

Manage plugins:

```text
/plugin
```

Manage language servers:

```text
/lsp
```

Update the CLI:

```text
/update
```

Show version:

```text
/version
```

## Useful display toggles

Toggle all timeline:

```text
Ctrl+o
```

Toggle reasoning display:

```text
Ctrl+t
```

Collapse or expand the autopilot goal panel:

```text
Ctrl+x g
```

Open the most recent link:

```text
Ctrl+x o
```

## Daily workflow

Start from the repository root:

```bash
cd /path/to/repo
copilot
```

Check environment:

```text
/env
/cwd
```

Ask for a plan before coding:

```text
/plan
```

Make or request changes, then inspect the diff:

```text
/diff
```

Run tests or shell commands:

```text
! pytest tests/some_test.py -q
```

Move long-running work to the background:

```text
Ctrl+x b
```

Check running work:

```text
/tasks
```

Queue the next instruction while current work is still running:

```text
Ctrl+q
```

Review PR status:

```text
/pr
```

Exit when done:

```text
/exit
```
