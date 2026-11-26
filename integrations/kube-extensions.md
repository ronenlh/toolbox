# Kubernetes CLI setup and daily commands

This guide covers a practical terminal setup for Kubernetes: `kubectl`, Krew plugins, `fzf`, `kubectx`, `kubens`, log viewing with `stern`/`k9s`, and parsing JSON logs with `jq`.

## Install the base tools

On macOS:

```bash
brew install kubectl
brew install fzf
brew install kubectx
brew install jq
brew install stern
brew install k9s
```

What each tool is for:

| Tool | Purpose |
|---|---|
| `kubectl` | Main Kubernetes CLI |
| `fzf` | Keyboard-driven fuzzy picker used by tools like `kubectx` and `kubens` |
| `kubectx` | Switch Kubernetes contexts interactively |
| `kubens` | Switch Kubernetes namespaces interactively |
| `jq` | Parse/filter JSON logs and Kubernetes API output |
| `stern` | Stream logs from multiple pods/containers in one terminal view |
| `k9s` | Full-screen terminal UI for browsing clusters, pods, logs, events, and resources |

## Set up kubeconfig

`kubectl` uses kubeconfig files to know which clusters, users, and namespaces are available.

The default kubeconfig path is:

```bash
~/.kube/config
```

Check available contexts:

```bash
kubectl config get-contexts
```

Show the current context:

```bash
kubectl config current-context
```

Switch context manually:

```bash
kubectl config use-context <context-name>
```

Check the current namespace:

```bash
kubectl config view --minify --output 'jsonpath={..namespace}'
```

Set the namespace for the current context:

```bash
kubectl config set-context --current --namespace <namespace>
```

## Install Krew

Krew is the plugin manager for `kubectl`.

Install Krew:

```bash
(
  set -x
  cd "$(mktemp -d)" &&
  OS="$(uname | tr '[:upper:]' '[:lower:]')" &&
  ARCH="$(uname -m | sed -e 's/x86_64/amd64/' -e 's/arm64/arm64/')" &&
  KREW="krew-${OS}_${ARCH}" &&
  curl -fsSLO "https://github.com/kubernetes-sigs/krew/releases/latest/download/${KREW}.tar.gz" &&
  tar zxvf "${KREW}.tar.gz" &&
  "./${KREW}" install krew
)
```

Add Krew plugins to your shell path:

```bash
echo 'export PATH="${KREW_ROOT:-$HOME/.krew}/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

Verify:

```bash
kubectl krew version
kubectl krew list
```

Currently installed Krew plugins on this machine:

```text
krew
tree
```

## Useful Krew plugins

Install plugins:

```bash
kubectl krew install tree
kubectl krew install ctx
kubectl krew install ns
kubectl krew install neat
kubectl krew install view-secret
```

Useful examples:

```bash
kubectl tree deployment my-service
kubectl neat get pod my-pod -o yaml
kubectl view-secret my-secret
```

Notes:

- `tree` shows owner/resource relationships, for example Deployment -> ReplicaSet -> Pod.
- `ctx` and `ns` are Krew alternatives to `kubectx` and `kubens`.
- `neat` removes noisy generated fields from Kubernetes YAML.
- `view-secret` decodes Kubernetes secrets for inspection. Use carefully.

## Use `kubectx` to switch clusters

Interactive context picker:

```bash
kubectx
```

Use arrow keys or type to filter, then press Enter.

Switch directly:

```bash
kubectx example-dev-cluster
kubectx example-stage-cluster
```

Show current context:

```bash
kubectx -c
```

Go back to the previous context:

```bash
kubectx -
```

## Use `kubens` to switch namespaces

Interactive namespace picker:

```bash
kubens
```

Use arrow keys or type to filter, then press Enter.

Switch directly:

```bash
kubens example-namespace
kubens other-namespace
```

Go back to the previous namespace:

```bash
kubens -
```

## Handy aliases

Add these to `~/.zshrc`:

```bash
alias k=kubectl
alias kctx=kubectx
alias kns=kubens
alias kgp='kubectl get pods'
alias kgs='kubectl get svc'
alias kgd='kubectl get deploy'
alias kdp='kubectl describe pod'
alias kl='kubectl logs'
alias klf='kubectl logs -f'
```

Reload:

```bash
source ~/.zshrc
```

## Basic `kubectl` commands

Get pods:

```bash
kubectl get pods
kubectl get pods -A
kubectl get pods -n example-namespace
```

Get deployments and services:

```bash
kubectl get deploy
kubectl get svc
```

Describe a pod:

```bash
kubectl describe pod <pod-name>
```

Get YAML:

```bash
kubectl get deploy <deployment-name> -o yaml
```

Exec into a pod:

```bash
kubectl exec -it <pod-name> -- /bin/sh
```

Port-forward a service:

```bash
kubectl port-forward svc/<service-name> 8080:80
```

Delete a pod:

```bash
kubectl delete pod <pod-name>
```

## View logs for a service in one place

Use `stern` when a service has multiple pods, rolling deployments, or multiple containers. It tails logs from all matching pods in one terminal view.

Tail all pods whose name contains `example-service`:

```bash
stern example-service -n example-namespace
```

Tail by Kubernetes label:

```bash
stern -n example-namespace -l app.kubernetes.io/instance=example-service
```

Show logs from the last hour:

```bash
stern -n example-namespace example-service --since 1h
```

Show timestamps:

```bash
stern -n example-namespace example-service --timestamps
```

Only include one container:

```bash
stern -n example-namespace example-service -c <container-name>
```

If you want an interactive terminal dashboard instead, use `k9s`:

```bash
k9s
```

Useful `k9s` keys:

| Key | Action |
|---|---|
| `:pods` | Go to pods |
| `:deploy` | Go to deployments |
| `:svc` | Go to services |
| `/` | Filter resources |
| `l` | Show logs for selected pod |
| `s` | Open shell in selected pod |
| `d` | Describe selected resource |
| `0` | Show all namespaces |
| `q` | Back/quit |

## Standard `kubectl logs`

Logs for one pod:

```bash
kubectl logs <pod-name>
```

Follow logs:

```bash
kubectl logs -f <pod-name>
```

Logs for a deployment:

```bash
kubectl logs deploy/<deployment-name>
```

Follow all pods in a deployment:

```bash
kubectl logs -f deploy/<deployment-name> --all-containers=true
```

Previous crashed container logs:

```bash
kubectl logs <pod-name> --previous
```

Logs from the last 30 minutes:

```bash
kubectl logs <pod-name> --since=30m
```

## Parse JSON logs with `jq`

If the application writes one JSON object per log line, pipe logs to `jq`:

```bash
kubectl logs -f deploy/<deployment-name> | jq .
```

Common JSON log fields:

```bash
kubectl logs deploy/<deployment-name> \
  | jq -r '[.timestamp, .level, .message] | @tsv'
```

Filter errors:

```bash
kubectl logs deploy/<deployment-name> \
  | jq -r 'select(.level == "ERROR" or .level == "error")'
```

Show only selected fields:

```bash
kubectl logs deploy/<deployment-name> \
  | jq -r '{time: .timestamp, level: .level, message: .message, trace: .traceId}'
```

Handle mixed plain-text and JSON logs without breaking on non-JSON lines:

```bash
kubectl logs -f deploy/<deployment-name> \
  | jq -R 'fromjson? // {message: .}'
```

Extract messages from mixed logs:

```bash
kubectl logs -f deploy/<deployment-name> \
  | jq -Rr 'fromjson? | select(.) | .message'
```

Use `stern` with `jq` for multiple pods. Disable stern's extra formatting so each log line can be parsed cleanly:

```bash
stern -n example-namespace example-service --output raw \
  | jq -R 'fromjson? // {message: .}'
```

Filter JSON logs from multiple pods for errors:

```bash
stern -n example-namespace example-service --output raw \
  | jq -Rr 'fromjson? | select(.level == "ERROR" or .level == "error") | [.timestamp, .level, .message] | @tsv'
```

## Recommended daily workflow

Pick the cluster:

```bash
kubectx
```

Pick the namespace:

```bash
kubens
```

Check workloads:

```bash
kubectl get pods
kubectl get deploy
kubectl get svc
```

Tail service logs in one place:

```bash
stern <service-name>
```

Open the terminal UI when you want to inspect resources interactively:

```bash
k9s
```