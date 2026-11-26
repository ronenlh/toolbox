# `history | rg` quick guide

Use shell history with `rg` to find commands you ran before.

## Search command history

```bash
history | rg databricks
```

```bash
history | rg kubectl
```

```bash
history | rg "helm uninstall"
```

## Case-insensitive search

```bash
history | rg -i sonar
```

## Search for multiple words

```bash
history | rg "gh pr|gh run"
```

## Show context around matches

```bash
history | rg -C 2 databricks
```

## Re-run a command safely

Copy the command from history, inspect it, edit anything dangerous, then run it manually.

Avoid blindly running commands by history number if they delete, deploy, uninstall, or mutate remote systems.
