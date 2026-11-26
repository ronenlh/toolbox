# Databricks CLI quick guide

This guide covers installing, authenticating, and using the Databricks CLI from the terminal.

## Install

On macOS:

```bash
brew tap databricks/tap
brew install databricks
```

Check the installed version:

```bash
databricks --version
```

Current local version:

```text
Databricks CLI v0.218.0
```

## Configure authentication

The Databricks CLI reads profiles from:

```bash
~/.databrickscfg
```

Typical config:

```ini
[DEFAULT]
host      = https://YOUR_WORKSPACE_HOST
auth_type = databricks-cli
```

Log in interactively:

```bash
databricks auth login --host https://YOUR_WORKSPACE_HOST
```

Verify the active user:

```bash
databricks current-user me
```

If you have multiple profiles for the same host, commands may fail with:

```text
multiple profiles matched
```

Fix it by either passing a profile:

```bash
databricks jobs list --profile DEFAULT
```

or setting a default profile:

```bash
export DATABRICKS_CONFIG_PROFILE=DEFAULT
```

For a permanent shell default, add it to `~/.zshrc`:

```bash
echo 'export DATABRICKS_CONFIG_PROFILE=DEFAULT' >> ~/.zshrc
source ~/.zshrc
```

## Common workspace commands

List workspace files:

```bash
databricks workspace list /Workspace
```

Create a directory:

```bash
databricks workspace mkdirs /Workspace/Users/<user>/demo
```

Import a local notebook or file:

```bash
databricks workspace import ./notebook.py /Workspace/Users/<user>/notebook.py --format SOURCE --overwrite
```

Export a workspace file:

```bash
databricks workspace export /Workspace/Users/<user>/notebook.py ./notebook.py --format SOURCE
```

Delete a workspace path:

```bash
databricks workspace delete /Workspace/Users/<user>/demo --recursive
```

## Jobs

List jobs:

```bash
databricks jobs list
```

Get one job:

```bash
databricks jobs get <job-id>
```

Run a job now:

```bash
databricks jobs run-now <job-id>
```

List job runs:

```bash
databricks jobs list-runs --job-id <job-id>
```

Get a run:

```bash
databricks jobs get-run <run-id>
```

Get run output:

```bash
databricks jobs get-run-output <run-id>
```

Cancel a run:

```bash
databricks jobs cancel-run <run-id>
```

## Clusters

List clusters:

```bash
databricks clusters list
```

Get cluster details:

```bash
databricks clusters get <cluster-id>
```

Start a cluster:

```bash
databricks clusters start <cluster-id>
```

Restart a cluster:

```bash
databricks clusters restart <cluster-id>
```

Terminate a cluster:

```bash
databricks clusters delete <cluster-id>
```

## DBFS and files

List files:

```bash
databricks fs ls dbfs:/
```

Copy local file to DBFS:

```bash
databricks fs cp ./local-file.txt dbfs:/tmp/local-file.txt
```

Copy DBFS file locally:

```bash
databricks fs cp dbfs:/tmp/local-file.txt ./local-file.txt
```

Remove a DBFS path:

```bash
databricks fs rm dbfs:/tmp/local-file.txt
```

## Secrets

List secret scopes:

```bash
databricks secrets list-scopes
```

List secrets in a scope:

```bash
databricks secrets list-secrets <scope-name>
```

Create or update a secret:

```bash
databricks secrets put-secret <scope-name> <secret-key>
```

Do not paste secrets into shell history when avoidable.

## SQL warehouses and queries

List SQL warehouses:

```bash
databricks warehouses list
```

Get a warehouse:

```bash
databricks warehouses get <warehouse-id>
```

List queries:

```bash
databricks queries list
```

## Databricks Asset Bundles

For a project with a `databricks.yml`, run Databricks Asset Bundle commands from its root.

Validate the bundle:

```bash
databricks bundle validate
```

Deploy the bundle:

```bash
databricks bundle deploy
```

Run a bundle job:

```bash
databricks bundle run <job-name>
```

Destroy deployed bundle resources:

```bash
databricks bundle destroy
```

Use the explicit profile if auth resolution is ambiguous:

```bash
databricks bundle validate --profile DEFAULT
```

## JSON output and `jq`

Many commands support JSON output:

```bash
databricks jobs list --output json
```

Filter with `jq`:

```bash
databricks jobs list --output json \
  | jq -r '.[] | [.job_id, .settings.name] | @tsv'
```

Find jobs by name:

```bash
databricks jobs list --output json \
  | jq -r '.[] | select(.settings.name | test("context"; "i")) | [.job_id, .settings.name] | @tsv'
```

## Useful shell aliases

Add to `~/.zshrc`:

```bash
alias dbx='databricks'
alias dbxjobs='databricks jobs list'
alias dbxme='databricks current-user me'
alias dbxws='databricks workspace list'
```

Reload:

```bash
source ~/.zshrc
```

## Daily workflow

Check auth:

```bash
databricks current-user me
```

List jobs:

```bash
databricks jobs list
```

Find a job:

```bash
databricks jobs list --output json \
  | jq -r '.[] | select(.settings.name | test("my-job"; "i")) | [.job_id, .settings.name] | @tsv'
```

Run it:

```bash
databricks jobs run-now <job-id>
```

Watch recent runs:

```bash
databricks jobs list-runs --job-id <job-id>
```