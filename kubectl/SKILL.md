---
name: kubectl
description: Use the kubectl CLI for Kubernetes operations. Read-only by default; update operations require user approval.
triggers:
  - kubectl
  - kubernetes
  - k8s
  - kube
---

# kubectl skill

You have been invoked to help the user interact with a Kubernetes cluster using the `kubectl` CLI.

## Step 1: Locate kube config

Check if a kubeconfig file is available:

1. Check the default location: `~/.kube/config`
2. Check if `KUBECONFIG` env var is set
3. Check if the user provided a path in their request

If none of the above yield a valid kubeconfig file, **stop immediately** and ask the user:

> No kubeconfig file was found. Please provide the path to your kubeconfig file (e.g. `/path/to/kubeconfig.yaml`), or set the `KUBECONFIG` environment variable.

Do not proceed until a valid kubeconfig path is confirmed.

Once found, use it via `--kubeconfig <path>` or export `KUBECONFIG=<path>` for all subsequent commands.

## Step 2: Classify the operation

Determine whether the user's request is a **read** or **update** operation:

### Read operations (allowed without approval)
These are safe and can be run immediately:
- `kubectl get ...`
- `kubectl describe ...`
- `kubectl logs ...`
- `kubectl explain ...`
- `kubectl top ...`
- `kubectl version`
- `kubectl cluster-info`
- `kubectl api-resources`
- `kubectl api-versions`
- `kubectl config view / get-contexts / current-context`
- `kubectl diff ...`
- `kubectl auth can-i ...`

### Update operations (require user approval before running)
These modify cluster state — always confirm with the user first:
- `kubectl apply ...`
- `kubectl create ...`
- `kubectl delete ...`
- `kubectl edit ...`
- `kubectl patch ...`
- `kubectl replace ...`
- `kubectl scale ...`
- `kubectl rollout ...`
- `kubectl set ...`
- `kubectl label ...`
- `kubectl annotate ...`
- `kubectl taint ...`
- `kubectl cordon / uncordon / drain ...`
- `kubectl exec ...` (can modify state)
- `kubectl port-forward ...` (opens network tunnels — confirm intent)
- `kubectl cp ...`
- Any other command that writes to or modifies the cluster

For update operations, show the exact command you plan to run and ask:

> This command will modify the cluster. Do you want to proceed?
> `kubectl <command>`

Only run the command after explicit user confirmation.

## Step 3: Execute and report

- Run read commands directly and report the output clearly.
- For multi-step tasks, present a plan before executing.
- If a command fails, show the error and suggest remediation.
- Use `-n <namespace>` or `--all-namespaces` as appropriate based on the user's context.
- Format tabular output for readability; use `-o yaml` or `-o json` when the user needs full detail.
