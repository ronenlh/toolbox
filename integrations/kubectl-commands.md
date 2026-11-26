# Common kubectl commands quick guide

This is a short reference for daily Kubernetes commands.

## Context and namespace

Use `kubectx` to switch clusters/contexts interactively:

```bash
kubectx
```

Show current context:

```bash
kubectx -c
```

Switch directly:

```bash
kubectx <context-name>
```

Switch back to the previous context:

```bash
kubectx -
```

Use `kubens` to switch namespaces interactively:

```bash
kubens
```

Switch directly:

```bash
kubens <namespace>
```

Switch back to the previous namespace:

```bash
kubens -
```

## Get resources

Get pods:

```bash
kubectl get pods
kubectl get pods -n <namespace>
kubectl get pods -A
```

Get services:

```bash
kubectl get svc
kubectl get svc -n <namespace>
```

Get deployments:

```bash
kubectl get deploy
kubectl get deploy -n <namespace>
```

Get all common resources in a namespace:

```bash
kubectl get all -n <namespace>
```

Get wide output:

```bash
kubectl get pods -o wide
```

Watch pods:

```bash
kubectl get pods -w
```

## Describe and inspect

Describe a pod:

```bash
kubectl describe pod <pod-name>
```

Describe a service:

```bash
kubectl describe svc <service-name>
```

Describe a deployment:

```bash
kubectl describe deploy <deployment-name>
```

Get YAML:

```bash
kubectl get pod <pod-name> -o yaml
kubectl get deploy <deployment-name> -o yaml
```

Get JSON and parse with `jq`:

```bash
kubectl get pod <pod-name> -o json | jq .
```

## Logs

Show pod logs:

```bash
kubectl logs <pod-name>
```

Follow pod logs:

```bash
kubectl logs -f <pod-name>
```

Show logs for a specific container:

```bash
kubectl logs <pod-name> -c <container-name>
```

Show previous crashed container logs:

```bash
kubectl logs <pod-name> --previous
```

Show logs from the last 30 minutes:

```bash
kubectl logs <pod-name> --since=30m
```

Show deployment logs:

```bash
kubectl logs deploy/<deployment-name>
```

Follow logs from all containers in a deployment:

```bash
kubectl logs -f deploy/<deployment-name> --all-containers=true
```

Tail multiple pods for a service with `stern`:

```bash
stern <service-name> -n <namespace>
```

## Exec into pods

Open a shell:

```bash
kubectl exec -it <pod-name> -- /bin/sh
```

If the container has bash:

```bash
kubectl exec -it <pod-name> -- /bin/bash
```

Exec into a specific container:

```bash
kubectl exec -it <pod-name> -c <container-name> -- /bin/sh
```

Run one command:

```bash
kubectl exec <pod-name> -- env
kubectl exec <pod-name> -- printenv
```

## Port forwarding

Forward a service port:

```bash
kubectl port-forward svc/<service-name> 8080:80
```

Forward a pod port:

```bash
kubectl port-forward pod/<pod-name> 8080:8080
```

## Restart and scale

Restart a deployment:

```bash
kubectl rollout restart deploy/<deployment-name>
```

Check rollout status:

```bash
kubectl rollout status deploy/<deployment-name>
```

Scale a deployment:

```bash
kubectl scale deploy/<deployment-name> --replicas=2
```

## Delete resources

Delete a pod:

```bash
kubectl delete pod <pod-name>
```

Delete a deployment:

```bash
kubectl delete deploy <deployment-name>
```

Delete a service:

```bash
kubectl delete svc <service-name>
```

## Events

Show namespace events:

```bash
kubectl get events --sort-by=.lastTimestamp
```

Show recent warning events:

```bash
kubectl get events --field-selector type=Warning --sort-by=.lastTimestamp
```

## Useful patterns

Use an explicit namespace:

```bash
kubectl -n <namespace> get pods
kubectl -n <namespace> logs -f <pod-name>
```

Use an explicit context:

```bash
kubectl --context <context-name> get pods -n <namespace>
```

Find pods by name:

```bash
kubectl get pods -A | rg '<name>'
```

Find resources by label:

```bash
kubectl get pods -l app.kubernetes.io/instance=<release-name>
kubectl get svc -l app.kubernetes.io/instance=<release-name>
```
