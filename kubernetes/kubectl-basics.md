# kubectl Command Reference

`kubectl` is the primary command-line client used to communicate with the Kubernetes API server. This guide documents the most frequent diagnostic and operational commands.

---

## 1. Context and Cluster Inspection

```bash
# Print current active cluster context
kubectl config current-context

# List all available contexts in your kubeconfig
kubectl config get-contexts

# Switch context to a different cluster
kubectl config use-context my-lab-cluster

# View api-server status and node health
kubectl cluster-info
kubectl get nodes -o wide
```

---

## 2. Namespace Operations

Namespaces are virtual clusters inside a physical Kubernetes cluster, separating workloads.

```bash
# List all active namespaces
kubectl get namespaces

# Create a new namespace
kubectl create namespace lab-playground

# Set default namespace for current shell context (prevents typing -n)
kubectl config set-context --current --namespace=lab-playground
```

---

## 3. Pod Inspection and Diagnostics

```bash
# List pods in the active namespace
kubectl get pods

# List pods across all namespaces
kubectl get pods -A

# List pods with details (IPs, node mapping)
kubectl get pods -o wide

# Monitor pods state continuously in terminal
kubectl get pods -w
```

---

## 4. Viewing Logs

```bash
# Print stdout logs for a pod
kubectl logs <pod-name>

# Stream live tailing logs (like tail -f)
kubectl logs -f <pod-name>

# Get logs from a specific container inside a multi-container pod
kubectl logs <pod-name> -c <container-name>

# View logs from previously crashed instances of a container
kubectl logs <pod-name> --previous
```

---

## 5. Describe Resource Details

`describe` queries the exact event queue and spec variables of a resource, crucial for debugging failures (like `ImagePullBackOff` or `Pending`).

```bash
# Detailed inspection of a specific pod
kubectl describe pod <pod-name>

# Detailed inspection of a specific node
kubectl describe node <node-name>
```

---

## 6. Interactive Command Execution

```bash
# Run an interactive bash shell inside a running container
kubectl exec -it <pod-name> -- /bin/bash

# Execute a quick diagnostic command directly without interactive shell
kubectl exec <pod-name> -- curl -I localhost:80
```
