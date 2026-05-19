# Kubernetes Container Orchestration Labs

This section documents using **Kubernetes** to deploy, run, and scale containerized applications. It details essential cluster choices, diagnostic commands, service routing models, persistent storage, and package management via Helm.

## Lab Index

| Lab / Guide | Description | Key Focus | Link |
|---|---|---|---|
| **kubectl Basics** | Core client utility commands for context, logs, and shell execs | Cluster Diagnostic | [kubectl Basics](./kubectl-basics.md) |
| **Deployments** | Running scale-out, self-healing application container sets | Stateless Applications | [Deployments Guide](./deployments.md) |
| **Services** | Exposing network layers to direct traffic between internal/external nodes | Network Routing | [Services Guide](./services.md) |
| **Ingress Routing** | Setting up path/domain routing rules for cluster entry | Application Gateways | [Ingress Guide](./ingress.md) |
| **Storage Lifecycle** | Provisioning persistent volumes and volume claims for databases | Stateful Storage | [Storage Guide](./storage.md) |
| **Helm Charts** | Packaging and deploying complete third-party application stacks | Package Management | [Helm Guide](./helm.md) |

## Lab Cluster Options

When setting up your laboratory environment, consider these cluster platforms:

| Option | Ideal Use Case | Pros | Cons |
|---|---|---|---|
| **k3s** | Lightweight homelab or small-scale VM nodes | Ultra-low RAM usage, built-in Traefik/local storage, single binary install | Deviates slightly from standard vanilla configs |
| **kubeadm** | Production-like, vanilla Kubernetes study | Teaches raw clustering, certificate management, standard operations | Complex manual provisioning, high host system overhead |
| **kind** | Ephemeral testing and local disposable labs | Runs clusters inside docker containers, incredibly fast startup | Nodes are not distinct hosts, poor for persistent homelabs |
| **minikube** | Single-node developer local setup | Stable, very widely documented, supports multiple local drivers | Restricted to a single node by default |
| **Managed K8s** | Cloud-native production workflows (EKS/GKE/AKS) | Zero hypervisor overhead, managed cloud integration | Costly for continuous personal learning |
