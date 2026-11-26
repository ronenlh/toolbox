# `man` quick guide

`man` opens the manual page for a command, configuration file, system call, or library.

## Basic usage

```bash
man command
man man
man 5 ssh_config
man -k keyword
man -f command
```

- `man command` - Read a command's manual.
- `man 5 ssh_config` - Open section 5, for file formats and configuration files.
- `man -k keyword` - Search manual page names and descriptions.
- `man -f command` - Show the command's short description and available sections.

Inside a manual page:

| Key | Action |
|---|---|
| `Space` | Next page |
| `b` | Previous page |
| `/text` | Search forward |
| `n` | Next search match |
| `q` | Quit |

Useful examples:

```bash
man watch
man git
man 1 printf
man 3 printf
```

The section number matters when a command and a library function share a name. Use `man man` for the complete reference.
