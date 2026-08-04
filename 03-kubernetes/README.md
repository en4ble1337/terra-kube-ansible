# Phase 03 — Kubernetes Foundations

- **Progress checklist:** [Kubernetes issue](https://github.com/en4ble1337/terra-kube-ansible/issues/3)
- **Prerequisites:** Terraform and Ansible completion gates
- **Primary path:** kubeadm first, K3s second, RKE2 optional
- **Outcome:** A functional three-node kubeadm cluster and a documented lightweight K3s comparison.
- **Estimated effort:** 12 sessions × approximately 3 hours

## Why this order

Use full Proxmox VMs for the primary cluster. kubeadm exposes the control plane, container runtime, networking, joining, upgrades, and troubleshooting. K3s is then useful because you can identify what the lightweight distribution packages and abstracts. RKE2 is a stretch comparison, not the first learning target.

## Session sequence

1. Kubernetes architecture and reconciliation
2. Pods, controllers, services, configuration, and namespaces
3. Host prerequisites, containerd, cgroups, and version pinning
4. kubeadm control-plane initialization
5. CNI installation and worker joins
6. Deployments, probes, requests, limits, ConfigMaps, and Secrets
7. Service networking, CoreDNS, and troubleshooting
8. MetalLB and ingress for home-lab access
9. RBAC and least-privilege service accounts
10. Node maintenance, drain, disruption, and reconciliation
11. Controlled break/fix and operations runbook
12. K3s rebuild and comparison; RKE2 optional

## Completion gate

The kubeadm cluster must run a reachable replicated workload, survive a controlled pod or worker disruption, and include documented troubleshooting. The K3s comparison must explain what is abstracted rather than only showing a successful install.

## Official references

- [Creating a cluster with kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/)
- [K3s documentation](https://docs.k3s.io/)
- [RKE2 documentation](https://docs.rke2.io/)
