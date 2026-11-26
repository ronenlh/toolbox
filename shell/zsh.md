# zsh and Oh My Zsh quick guide

This guide covers setting up `zsh`, Oh My Zsh, and common `~/.zshrc` configuration.

## Check zsh

macOS uses `zsh` by default.

```bash
zsh --version
echo "$SHELL"
```

If needed, switch your login shell to zsh:

```bash
chsh -s /bin/zsh
```

Restart the terminal after changing the login shell.

## Install Oh My Zsh

Install:

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

Oh My Zsh lives under:

```bash
~/.oh-my-zsh
```

Your main shell config is:

```bash
~/.zshrc
```

## Edit `.zshrc`

Open it:

```bash
vim ~/.zshrc
```

Reload after editing:

```bash
source ~/.zshrc
```

## Common `.zshrc` settings

Theme:

```zsh
ZSH_THEME="robbyrussell"
```

Useful plugins:

```zsh
plugins=(
  git
  kubectl
  docker
  brew
  fzf
)
```

Common aliases:

```zsh
alias ll='ls -lah'
alias gs='git status'
alias gco='git checkout'
alias gl='git pull'
alias gp='git push'
alias k='kubectl'
alias kctx='kubectx'
alias kns='kubens'
```

Path additions:

```zsh
export PATH="$HOME/.local/bin:$PATH"
export PATH="/opt/homebrew/bin:$PATH"
```

Default editor:

```zsh
export EDITOR=vim
```

## Oh My Zsh update

Update Oh My Zsh:

```bash
omz update
```

Reload shell:

```bash
exec zsh
```

## Prompt and terminal quality of life

Enable command history search:

```zsh
HISTSIZE=100000
SAVEHIST=100000
setopt SHARE_HISTORY
setopt HIST_IGNORE_DUPS
setopt HIST_REDUCE_BLANKS
```

Search history:

```bash
history | rg kubectl
history | rg databricks
```

## Keep secrets out of `.zshrc`

Avoid writing API tokens directly into `~/.zshrc`.

Prefer one of these:

- export secrets only in the current shell when needed
- use a password manager
- use a dedicated local secrets file that is not committed

Example current-shell-only export:

```bash
export SOME_TOKEN='<token>'
```

Unset it:

```bash
unset SOME_TOKEN
```

## Troubleshooting

Check which config file is being used:

```bash
echo "$ZDOTDIR"
echo "$HOME/.zshrc"
```

Check current aliases:

```bash
alias
```

Check current path:

```bash
echo "$PATH" | tr ':' '\n'
```

Start a clean zsh without user config:

```bash
zsh -f
```
