# JFrog CLI (`jf`) quick guide

```bash
jf --version
# jf version 2.122.0 (installed via: brew install jfrog-cli)
```

## Existing config

```bash
jf c show
```

```text
Server ID:      my-artifactory
JFrog Platform URL:  https://your-instance.jfrog.io/
Artifactory URL:    https://your-instance.jfrog.io/artifactory/
User:        your-username
Default:       true
```

Config lives at `~/.jfrog/jfrog-cli.conf.v6` (mode `600`) — includes access token + refresh token. Set it up via the interactive `jf c add` prompt.

## Re-authenticating when the token expires

If JFrog access tokens are configured with short lifetimes, they can auto-refresh using a stored refresh token, which may also expire. If both expire, `jf rt curl` etc. return `401 Bad Credentials` / `Token failed verification: expired`, and there's no way to CLI-refresh past that — you must re-run the interactive login:

```bash
jf c add my-artifactory --overwrite
```

Follow the prompts using your JFrog platform URL and credentials. Avoid `--interactive=false` unless you're passing `--access-token` or `--password` explicitly — running it non-interactively with no credentials will **wipe** the stored token/password from the config.

Verify after re-auth:

```bash
jf rt curl -s /api/repositories | head
```

## Common commands

```bash
# List repositories
jf rt curl -s /api/repositories | jq '.[].key'

# Search for artifacts
jf rt search "my-npm-group/*react*"

# Upload / download a file
jf rt upload ./dist/app.tar.gz my-generic-local/releases/
jf rt download my-generic-local/releases/app.tar.gz ./

# Generic REST call (escape hatch for anything not covered)
jf rt curl -s /api/storage/my-npm-group

# Switch between configured servers (if you add more than one)
jf c use <server-id>
jf c list
```

## Related package-manager auth (same Artifactory, different tokens)

These are separate from `jf`'s own token, but hit the same instance:

- **npm** — `~/.npmrc` can have per-scope `_authToken` entries for your JFrog npm registry.
- **pip/uv** — configure package index credentials for your private index.

If npm/pip auth breaks, that's a separate credential from `jf`'s — check `.npmrc` tokens or the `UV_INDEX_PRIVATE_REGISTRY_*` vars, not `jf c show`.
