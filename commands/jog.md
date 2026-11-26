# `jog` quick guide

`jog` is a small script that prints the last commands you ran in the **current directory**, sourced from extended zsh history.

**Origin:** [natethinks/jog](https://github.com/natethinks/jog) — the local `~/bin/jog` is a byte-for-byte copy of that repo's `jog` script (confirmed via GitHub code search), and the `zshaddhistory` hook in `~/.zshrc` matches the repo's README exactly, including the `⋮` separator.

## Notes

- Matching is by exact current working directory (`$PWD`), so results differ per folder.
- Only the last 10 matches are shown (`tail`).
- `~/.zsh_history_ext` can grow large over time; rotate or trim it periodically if needed.
