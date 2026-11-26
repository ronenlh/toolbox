# `kill-port` quick guide

`kill-port` frees up a port by killing whatever process is bound to it — handy after a crashed development process leaves a port (e.g. `3300`, `7687`) stuck in use.

Installed globally via npm:

```bash
npm i -g kill-port
# kill-port@2.0.1
```

## Usage

```bash
kill-port 3300
```

Kill multiple ports at once:

```bash
kill-port 3300 7687 3001
```

If nothing is listening, it just reports that instead of erroring:

```text
Could not kill process on port 9999. No process running on port.
```

## Real examples from history

```bash
kill-port 3300     # free a local development server
kill-port 7687     # free a stuck neo4j bolt port-forward
kill-port 5000     # free a leftover local dev server
```

## Equivalent one-liner (no install needed)

If `kill-port` isn't available on a machine:

```bash
lsof -ti :3300 | xargs kill -9
```

## Notes

- Requires the process to actually own the port (won't work across permission boundaries, e.g. killing another user's process).
