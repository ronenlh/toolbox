# `nvim` quick guide

Neovim is a Vim-compatible terminal editor with a Lua configuration and an extensible plugin ecosystem.

## Install

Use the package manager when possible.

macOS or Linux with Homebrew:

```bash
brew install neovim
```

Start Neovim with `nvim`, not `neovim`:

```bash
nvim
nvim path/to/file.md
```

To use the familiar `vim` command as an alias, add this to `~/.zshrc`:

```bash
alias vim='nvim'
source ~/.zshrc
vim path/to/file.md
```

When a package manager is unavailable, use the official release archive. For macOS Apple silicon:

```bash
curl -LO https://github.com/neovim/neovim/releases/latest/download/nvim-macos-arm64.tar.gz
tar xzf nvim-macos-arm64.tar.gz
./nvim-macos-arm64/bin/nvim
```

Choose the matching archive for Intel macOS or Linux from the [official installation guide](https://neovim.io/doc/install/).

Verify the installation and configuration:

```bash
nvim --version
nvim --clean
```

Inside Neovim, run `:checkhealth` to diagnose providers and configuration.

## The basic model

- **Normal mode:** navigate and issue commands. Press `Esc` to return here.
- **Insert mode:** type text. Press `i` to enter it.
- **Command-line mode:** enter commands such as `:w` and `:q`.
- **Visual mode:** select text with `v`, `V`, or `Ctrl-v`.

## Navigation

| Keys | Action |
|---|---|
| `h` `j` `k` `l` | Move left, down, up, right |
| `w` / `b` | Move to the next / previous word |
| `0` / `$` | Start / end of the line |
| `gg` / `G` | First / last line |
| `:42` | Jump to line 42 |
| `%` | Jump between matching brackets |
| `Ctrl-d` / `Ctrl-u` | Half-page down / up |
| `Ctrl-f` / `Ctrl-b` | Full-page down / up |
| `zt` / `zz` / `zb` | Put the cursor at the top / center / bottom |

Prefix a motion with a number, such as `5j`, to repeat it.

## Search and replace

```text
/text       search forward
?text       search backward
n            next match
N            previous match
:noh        clear search highlighting
:%s/old/new/g    replace every match in the file
:%s/old/new/gc   replace with confirmation
```

Search across files from the shell with `rg`, or use `:vimgrep /pattern/ **/*` inside Neovim.

## First editing commands

```text
i       insert before the cursor
a       append after the cursor
o       open a line below
u       undo
Ctrl-r  redo
yy      copy the current line
dd      delete the current line
p       paste after the cursor
:w      save
:q      quit
:wq     save and quit
:q!     quit without saving
```

Use `:help keyword` or press `K` over a word for built-in help. `vimtutor` is a useful interactive introduction to Vim motions.

## Plugins and configurations

Start with a small configuration and add plugins for a concrete need. Browse plugins and pre-built configurations at [Neovimcraft](https://neovimcraft.com/). Its catalog includes options such as `lazy.nvim`, `telescope.nvim`, Treesitter, LSP tooling, and complete starter configurations.

For the full motion reference, see the [Vim cheat sheet](https://vim.rtorr.com/). For platform-specific installation details, use the [official Neovim installation guide](https://neovim.io/doc/install/).
