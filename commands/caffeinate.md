# caffeinate quick guide

`caffeinate` keeps macOS awake from the terminal.

## Keep the Mac awake

```bash
caffeinate -dis
```

Meaning:

| Flag | Meaning |
|---|---|
| `-d` | Prevent display sleep |
| `-i` | Prevent idle system sleep |
| `-s` | Prevent system sleep while plugged into AC power |

Stop it with:

```text
Ctrl-c
```

## Keep awake while a command runs

```bash
caffeinate -dis npm test
```

```bash
caffeinate -dis databricks bundle deploy
```

## Keep awake for a fixed time

One hour:

```bash
caffeinate -dis -t 3600
```
