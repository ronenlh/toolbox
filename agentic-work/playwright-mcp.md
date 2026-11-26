# Microsoft Playwright MCP quick guide

This guide covers setting up the `microsoft/playwright-mcp` server so Copilot CLI (and other MCP-compatible agents) can drive a real browser: navigate pages, take snapshots, click, fill forms, and log in to protected sites.

## Prerequisites

Node.js and `npx` must be available:

```bash
node --version
npx --version
```

## MCP server config

The MCP server is configured in:

```bash
~/.copilot/mcp-config.json
```

Entry used on this machine:

```json
{
  "mcpServers": {
    "microsoft/playwright-mcp": {
      "type": "stdio",
      "command": "npx",
      "args": [
        "@playwright/mcp@latest"
      ]
    }
  }
}
```

No manual `npm install` is required — `npx` downloads/caches `@playwright/mcp` on first use.

## Enable/inspect it from Copilot CLI

Manage MCP servers interactively:

```text
/mcp
```

Show loaded environment, including active MCP servers:

```text
/env
```

## What it gives the agent

Once loaded, browser tools become available (exact tool names are provider-prefixed, e.g. `microsoft-playwright-mcp-browser_navigate`):

| Capability | Typical use |
|---|---|
| Navigate to a URL | Open an internal wiki/dashboard page |
| Take a snapshot | Read page structure/text without screenshots |
| Click / fill / select | Interact with forms, buttons, dropdowns |
| Handle login flows | SSO/OIDC pages that require interactive auth |

Use this instead of `web_fetch` when a page requires authentication, JavaScript rendering, or interactive login (for example, a company wiki behind SSO).

## Typical login-gated page workflow

1. Ask the agent to navigate to the target URL.
2. If it lands on your organization's login page, complete SSO/auth interactively in the opened browser session.
3. Ask the agent to take a snapshot again once authenticated — session cookies persist for subsequent navigations in the same run.

## Troubleshooting

If the browser tool isn't available, confirm the server started without errors:

```text
/env
```

If `npx` can't fetch the package, check network access and see `certs.md` for CA setup.

Clear npx's cache if a stale/broken version is cached:

```bash
npx clear-npx-cache
```

## Updating

Since the config uses `@playwright/mcp@latest`, restarting the CLI session (or `/mcp` reload) picks up the newest release automatically — no manual update step needed.
