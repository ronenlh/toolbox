# Bruno API collections

Bruno stores API requests in plain-text `.bru` files, making them suitable for
version control and agent-assisted editing. The following collection and API
are examples; adapt them to your own service.

## Collection structure

```text
bruno.json
environments/
  local.bru
  dev.bru
requests/
  health.bru
```

Define a base URL in each environment:

```bru
vars {
  baseUrl: http://localhost:3000
}
```

Use the variable in a request such as `requests/health.bru`:

```bru
meta {
  name: health
  type: http
  seq: 1
}

get {
  url: {{baseUrl}}/health
  body: none
  auth: none
}

docs {
  Check whether the service is healthy.
}
```

Keep actual tokens and secrets out of collection files and Git history. Use
Bruno's environment and secret facilities for values that differ by
environment.

## Working with an agent

Ask the agent to follow the collection's existing conventions and adapt requests
to the service's documented API. For example:

```text
Add a Bruno request for the service's health endpoint.
Use the existing baseUrl environment variable and add a docs block.
Do not hardcode credentials or environment-specific URLs.
```

Review the diff, check request URLs and authentication, and run the changed
requests against an authorized environment.

## Bruno CLI

Install the CLI and check its version:

```bash
npm install -g @usebruno/cli
bru --version
```

Run a request from the collection root:

```bash
bru run "requests/health" --env local
```

Run a folder recursively, stopping on the first failure:

```bash
bru run "requests" --env local -r --bail
```

Override an environment variable for one run:

```bash
bru run "requests/health" --env local --env-var baseUrl=http://localhost:3000
```

Write a JSON report without request or response bodies:

```bash
bru run "requests" --env local -r \
  --reporter-json ./bruno-results.json \
  --reporter-skip-body
```

Use the Bruno desktop app when editing or debugging requests interactively.
