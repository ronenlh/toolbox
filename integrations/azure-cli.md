# Azure CLI (`az`) quick guide

```bash
az --version   # azure-cli 2.90.0
```

## Login

```bash
az login
```

Opens a browser SSO flow. For headless/remote sessions:

```bash
az login --use-device-code
```

Login to a specific tenant:

```bash
az login --tenant "YOUR_TENANT_ID"
```

Some tenant configurations also require an explicit scope:

```bash
az login --tenant "YOUR_TENANT_ID" --scope "https://management.core.windows.net//.default"
```

Log out / clear cached credentials:

```bash
az logout
```

## Subscriptions

List all subscriptions visible to your account:

```bash
az account list -o table
```

Show the current active one:

```bash
az account show
```

Switch the active subscription (by name or ID) — required before most `az aks`/resource commands, since AKS clusters live in specific subscriptions:

```bash
az account set --subscription "YOUR_SUBSCRIPTION_NAME"
# or by ID:
az account set --subscription "YOUR_SUBSCRIPTION_ID"
```

## Connecting to an AKS cluster

1. Set the subscription that owns the cluster (see above).
2. Find the cluster + its resource group:
   ```bash
   az aks list -o table
   ```
3. Pull cluster credentials into `~/.kube/config`:
   ```bash
   az aks get-credentials --resource-group <resource-group> --name <cluster-name>
   ```
   Add `--overwrite-existing` if a context with that name already exists and you want to refresh its credentials.

This adds a context to `~/.kube/config` rather than replacing the other contexts, so you accumulate contexts for every cluster you've connected to.

By default `get-credentials` gives you the **cluster-user** (non-admin) kubeconfig context. For an admin context (needed for some RBAC-restricted operations), add `--admin`:

```bash
az aks get-credentials --resource-group <resource-group> --name <cluster-name> --admin
```

## Using the cluster after `get-credentials`

Switch to the new context with `kubectx` (see `kube-extensions.md` / `kubectl-commands.md`):

```bash
kubectx <cluster-context>
kubens <namespace>
kubectl get pods
```

## Other useful `az aks` commands

```bash
az aks show --resource-group <rg> --name <cluster> -o table   # cluster details, k8s version, provisioning state
az aks browse --resource-group <rg> --name <cluster>          # opens k8s dashboard (if enabled)
az aks nodepool list --resource-group <rg> --cluster-name <cluster> -o table
az aks upgrade --resource-group <rg> --name <cluster> --kubernetes-version <version>
```

## Notes

- `az account list -o table` only shows subscriptions your signed-in identity can access — if a cluster's subscription is missing, you likely need access requested first.
- Credentials pulled via `get-credentials` are tied to your Azure AD identity; token refresh happens transparently via `kubectl`/`az` as long as you're still logged in (`az login`).
- If `kubectl` suddenly reports `401`/auth errors against an AKS context, first try `az login` again, then re-run `get-credentials --overwrite-existing`.
