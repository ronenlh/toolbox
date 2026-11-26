# tmux quick setup and daily commands

tmux lets you keep multiple terminal panes and windows inside one terminal session. This is useful for running a server, tests, logs, and shell commands side by side.

## Install tmux

On macOS:

```bash
brew install tmux
```

Start a new session:

```bash
tmux
```

Start a named session:

```bash
tmux new -s dev
```

Detach from tmux without stopping the session:

```text
Ctrl-b d
```

Reattach later:

```bash
tmux attach -t dev
```

List sessions:

```bash
tmux ls
```

## Suggested `~/.tmux.conf`

Create or edit your tmux config:

```bash
vim ~/.tmux.conf
```

Add:

```tmux
# Use a bigger history buffer.
set -g history-limit 50000

# Start windows and panes at 1 instead of 0.
set -g base-index 1
setw -g pane-base-index 1

# Reload config with Prefix + r.
bind r source-file ~/.tmux.conf \; display-message "tmux config reloaded"

# Split panes using intuitive keys.
bind | split-window -h
bind - split-window -v

# Move between panes with vim-style keys.
bind h select-pane -L
bind j select-pane -D
bind k select-pane -U
bind l select-pane -R

# Resize panes with capital vim-style keys.
bind H resize-pane -L 5
bind J resize-pane -D 5
bind K resize-pane -U 5
bind L resize-pane -R 5

# Show pane names in pane borders.
set -g pane-border-status top
set -g pane-border-format " #{pane_index}: #{pane_title} "

# Keep window names manual instead of auto-renaming them.
set -g allow-rename off
```

Reload the config:

```bash
tmux source-file ~/.tmux.conf
```

Or from inside tmux:

```text
Ctrl-b r
```

## Prefix key

Most tmux shortcuts start with the prefix:

```text
Ctrl-b
```

For example, `Ctrl-b %` means press `Ctrl-b`, release it, then press `%`.

## Create panes

Split the current pane vertically, creating a pane on the right:

```text
Ctrl-b %
```

Split the current pane horizontally, creating a pane below:

```text
Ctrl-b "
```

With the config above, you can also use:

```text
Ctrl-b |
Ctrl-b -
```

## Move between panes

Default tmux navigation:

```text
Ctrl-b Left
Ctrl-b Right
Ctrl-b Up
Ctrl-b Down
```

With the config above:

```text
Ctrl-b h
Ctrl-b j
Ctrl-b k
Ctrl-b l
```

Switch to the last active pane:

```text
Ctrl-b ;
```

Show pane numbers temporarily and jump to one:

```text
Ctrl-b q
```

Then press the pane number.

## Rename panes

tmux supports pane titles. Rename the current pane:

```bash
tmux select-pane -T "api"
```

Examples:

```bash
tmux select-pane -T "server"
tmux select-pane -T "tests"
tmux select-pane -T "logs"
tmux select-pane -T "shell"
```

Rename a specific pane:

```bash
tmux list-panes -F '#{pane_id} #{pane_index} #{pane_title}'
tmux select-pane -t %3 -T "worker"
```

Pane names are visible when this is enabled:

```tmux
set -g pane-border-status top
set -g pane-border-format " #{pane_index}: #{pane_title} "
```

## Resize panes

Default resize command mode:

```text
Ctrl-b :
resize-pane -L 10
resize-pane -R 10
resize-pane -U 5
resize-pane -D 5
```

With the config above:

```text
Ctrl-b H
Ctrl-b J
Ctrl-b K
Ctrl-b L
```

## Close panes

Close the current pane:

```text
exit
```

Or:

```text
Ctrl-d
```

Kill the current pane from tmux:

```text
Ctrl-b x
```

## Windows

Create a new window:

```text
Ctrl-b c
```

Rename the current window:

```text
Ctrl-b ,
```

Move to next or previous window:

```text
Ctrl-b n
Ctrl-b p
```

Jump to a window by number:

```text
Ctrl-b 1
Ctrl-b 2
Ctrl-b 3
```

## Copy mode and scrollback

Enter copy/scroll mode:

```text
Ctrl-b [
```

Scroll with arrow keys, Page Up/Page Down, or vim keys if enabled by your terminal setup.

Exit copy mode:

```text
q
```

## A simple daily layout

Start a named session:

```bash
tmux new -s dev
```

Rename the first pane:

```bash
tmux select-pane -T "shell"
```

Create panes:

```text
Ctrl-b |
Ctrl-b -
```

Rename each pane as you move into it:

```bash
tmux select-pane -T "server"
tmux select-pane -T "tests"
tmux select-pane -T "logs"
```

Detach when done:

```text
Ctrl-b d
```

Reattach:

```bash
tmux attach -t dev
```