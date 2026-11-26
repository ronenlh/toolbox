# `watch` quick guide

`watch` reruns a command at regular intervals and refreshes its output in the terminal. It is useful for monitoring pods, files, processes, and other changing local state.

## Install

macOS with Homebrew:

```bash
brew install watch
```

Debian/Ubuntu:

```bash
sudo apt install procps
```

Check the installation:

```bash
watch --version
```

## Usage

```text
watch [options] command
```

Examples:

```bash
watch kubectl get pods
watch -n 2 'kubectl get pods -A'
watch -d 'git status --short'
watch -g 'curl -s http://localhost:8080/health'
```

Press `Ctrl-c` to exit.

Use quotes when the command contains pipes, redirects, variables, or multiple arguments:

```bash
watch -n 1 'kubectl get pods | grep api'
```
