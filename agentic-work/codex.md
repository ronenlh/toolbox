# Short Guide to Codex CLI

Codex is an AI coding agent that works from your terminal. Run it inside a project so it can inspect files, explain code, make changes, and run checks.

## 1. Start Codex

```bash
cd /path/to/your/project
codex
```

If needed, authenticate first:

```bash
codex login
```

You can also begin with a task immediately:

```bash
codex "Explain this repository and show me how to run its tests"
```

## 2. Ask for a concrete outcome

Good prompts include:

- `Find the cause of the failing tests and explain it.`
- `Add input validation to the signup endpoint and test it.`
- `Review my current changes for bugs and security problems.`
- `Refactor this module without changing its public API.`

Mention constraints such as files to avoid, required test commands, compatibility requirements, or whether you want diagnosis only.

## 3. Review its work

Codex may inspect files, edit code, and run commands. Read approval requests carefully, especially for network access or destructive commands. Before accepting the result, review the diff and run the relevant checks:

```bash
git diff
git status
```

Ask Codex to fix anything you notice; the same session retains the conversation context.

## 4. Useful commands

```bash
codex exec "run the tests and summarize failures"  # Non-interactive task
codex review                                      # Review current code changes
codex resume --last                               # Continue the latest session
codex doctor                                      # Diagnose setup problems
codex --help                                      # List commands and options
```

## Practical tips

- Start from a clean Git state or commit your work first.
- Keep tasks focused and state what “done” means.
- Provide error messages, reproduction steps, and the expected behavior.
- Treat generated changes like a teammate's pull request: inspect and test them.
- Avoid `--dangerously-bypass-approvals-and-sandbox` unless the environment is already securely isolated.

Official documentation: <https://developers.openai.com/codex/>
