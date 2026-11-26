# CA certificates quick guide

This guide covers configuring trusted CA certificates for local CLI tools.

## Homebrew CA bundle

On this machine, the shared CA bundle is:

```bash
/opt/homebrew/etc/ca-certificates/cert.pem
```

Check that it exists:

```bash
ls -l /opt/homebrew/etc/ca-certificates/cert.pem
```

## Shell configuration

Add this to `~/.zshrc`:

```zsh
# CA Certs
CA_CERTS=/opt/homebrew/etc/ca-certificates/cert.pem
export REQUESTS_CA_BUNDLE=$CA_CERTS
export NODE_EXTRA_CA_CERTS=$CA_CERTS
export CURL_CA_BUNDLE=$CA_CERTS
```

Reload:

```bash
source ~/.zshrc
```

## What these variables do

| Variable | Used by |
|---|---|
| `REQUESTS_CA_BUNDLE` | Python `requests`, Azure CLI, many Python CLIs |
| `NODE_EXTRA_CA_CERTS` | Node.js tools, npm-based CLIs |
| `CURL_CA_BUNDLE` | `curl` and tools that rely on libcurl |

## Verify the environment

```bash
env | rg 'CA_CERTS|REQUESTS_CA_BUNDLE|NODE_EXTRA_CA_CERTS|CURL_CA_BUNDLE'
```

Expected:

```text
CA_CERTS=/opt/homebrew/etc/ca-certificates/cert.pem
REQUESTS_CA_BUNDLE=/opt/homebrew/etc/ca-certificates/cert.pem
NODE_EXTRA_CA_CERTS=/opt/homebrew/etc/ca-certificates/cert.pem
CURL_CA_BUNDLE=/opt/homebrew/etc/ca-certificates/cert.pem
```

## Test with common tools

Test `curl`:

```bash
curl -I https://github.com
```

Test Python requests:

```bash
python - <<'PY'
import requests

response = requests.get('https://github.com', timeout=10)
print(response.status_code)
PY
```

Test Node:

```bash
node -e "fetch('https://github.com').then(r => console.log(r.status)).catch(e => { console.error(e); process.exit(1); })"
```

## Temporary override

For one shell only:

```bash
export CA_CERTS=/opt/homebrew/etc/ca-certificates/cert.pem
export REQUESTS_CA_BUNDLE=$CA_CERTS
export NODE_EXTRA_CA_CERTS=$CA_CERTS
export CURL_CA_BUNDLE=$CA_CERTS
```

Unset for the current shell:

```bash
unset CA_CERTS REQUESTS_CA_BUNDLE NODE_EXTRA_CA_CERTS CURL_CA_BUNDLE
```

## Notes

- Keep this in shell config, not in project files.
- Do not commit local certificate paths into application code.
- If a tool still fails certificate validation, check whether it uses a separate trust-store setting.
