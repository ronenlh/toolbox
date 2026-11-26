# Short Guide to Herdr

Herdr is a terminal workspace manager for AI coding agents. Like tmux, a background server owns the terminals, so panes keep running when you detach or close the window. Unlike tmux, it is mouse-first and agent-aware. It detects Copilot CLI, Codex, Claude, and other agents inside panes and shows each one's state (`working`, `blocked`, `done`, `idle`) in a sidebar across all your projects.

Docs: <https://herdr.dev/docs/>

## 1. Install and start

```bash
curl -fsSL https://herdr.dev/install.sh | sh   # installs to ~/.local/bin/herdr
herdr --version
cd /path/to/project
herdr                                           # launch or attach to the default session
```

Update with `herdr update`. Stop everything (all panes) with `herdr server stop`.

## 2. Concepts

- **Session**: the persistent background server. `herdr` attaches to the default one; most people need only that.
- **Workspace**: one per repo, task, or investigation. The sidebar rolls agent states up per workspace.
- **Tab**: a layout inside a workspace, for example `agents`, `logs`, or `server`.
- **Pane**: a real terminal. It keeps running after you detach.
- **Agent**: a recognized coding agent running in a pane.

## 3. Everyday use

- Everything is clickable: panes, tabs, workspaces, split borders, and right-click menus.
- The prefix key is `ctrl+b`. Useful bindings:
  - `prefix+v` splits right.
  - `prefix+minus` splits down.
  - `prefix+c` opens a new tab.
  - `prefix+q` detaches.
  - `prefix+?` shows all bindings.
- Run `herdr` again to reattach. Everything kept running.
- Don't run `herdr` inside a Herdr pane (`HERDR_ENV=1` is set there); nested launches are blocked.

## 4. Agent integrations

Integrations report agent lifecycle state and/or native session restore:

```bash
herdr integration status            # e.g. copilot: current (v4), codex: current, claude: current
herdr integration install copilot   # also: codex, claude, opencode, ...
```

## 5. Inspect and drive from the CLI

Commands return JSON. Use them from scripts or from another agent:

```bash
herdr status                         # client/server versions, server running?
herdr session list
herdr workspace list                 # workspaces with rolled-up agent_status
herdr agent list                     # every agent: pane_id, cwd, agent_status
herdr agent read <pane-id> --lines 50          # recent terminal output, e.g. w1:p2
herdr agent wait <pane-id> --until done --timeout 600000
herdr agent prompt <pane-id> "run the tests" --wait
herdr worktree list
herdr worktree create --branch feat/x --base main   # new git worktree opened as a workspace
herdr pane split --current --direction right
herdr pane run <pane-id> npm test
```

To let a coding agent control Herdr from inside a pane, install the official skill:

```bash
npx skills add herdrdev/herdr --skill herdr -g
# or print it: herdr --skill
```

## 6. Configuration and troubleshooting

- Config: `~/.config/herdr/config.toml`. Print the defaults with `herdr --default-config` and apply edits with `herdr server reload-config`.
- Logs and the socket live in `~/.config/herdr/`.
- If an agent shows the wrong state, run `herdr agent explain <pane-id> --json` and `herdr integration status`.
- If a keybinding does nothing, the terminal or OS consumed it. See <https://herdr.dev/docs/keyboard/>.
- Herdr is not tmux. Don't use tmux commands or `.tmux.conf` syntax.
