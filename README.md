# Infra Forge Lab

Hands-on lab repository for learning Terraform, Ansible, Kubernetes, and platform engineering workflows. It serves as a personal lab notebook and implementation reference for provisioning infrastructure, configuring systems, and operating containerized and AI workloads across Proxmox, Linux, Kubernetes, and selected cloud services.

## Learning dashboard

The detailed learning program is tracked through GitHub Issues and rendered by a read-only HTML dashboard in [`docs/index.html`](./docs/index.html).

- **Live task state:** [Roadmap issues](https://github.com/en4ble1337/terra-kube-ansible/issues)
- **Operating guide:** [Learning dashboard workflow](./docs/learning-dashboard.md)
- **Planned GitHub Pages URL:** `https://en4ble1337.github.io/terra-kube-ansible/`

Progress is evidence-based. A task is checked only after the implementation works and the repository contains the relevant code, runbook, command output, screenshot, or explanation.

## Sections

| Section | Description |
|---|---|
| [Terraform](./terraform/) | Infrastructure provisioning labs for Proxmox and selected cloud comparisons |
| [Ansible](./ansible/) | Configuration management labs: inventories, playbooks, roles, and secrets |
| [Kubernetes](./kubernetes/) | kubeadm, K3s comparison, GPU Operator, storage, ingress, Helm, and operations |
| [Integrated Labs](./integrated-labs/) | Terraform-to-Ansible-to-Kubernetes end-to-end workflows |
| [Docs](./docs/) | Architecture, troubleshooting, evidence, glossary, and references |

## Current labs

- [Terraform Proxmox LXC Containers](./terraform/proxmox/lxc-containers.md) — provision and manage LXC containers as code on Proxmox VE.
- [Roadmap 01: Terraform](https://github.com/en4ble1337/terra-kube-ansible/issues/1) — build the repeatable Proxmox VM foundation for the Kubernetes lab.

## Learning roadmap

| Phase | Area | Primary outcome | Estimated effort |
|---:|---|---|---:|
| 1 | Terraform | Rebuildable Proxmox VM foundation | 30 hours |
| 2 | Ansible | Idempotent Kubernetes-node configuration | 18 hours |
| 3 | Kubernetes foundations | kubeadm cluster plus K3s comparison | 36 hours |
| 4 | NVIDIA GPU Operator | Schedulable and observable GPU worker | 18 hours |
| 5 | CSI storage | Persistent Synology NFS-backed workloads | 16 hours |
| 6 | Argo CD | GitOps deployment, drift repair, and rollback | 16 hours |
| 7 | Azure DevOps | Self-hosted CI/CD agent and deployment pipeline | 18 hours |
| 8 | Microsoft Entra | OIDC-protected API with authorization | 18 hours |
| 9 | Event-driven microservices | Resilient NATS-based asynchronous workflow | 20 hours |
| 10 | AI capstone | Authenticated, asynchronous GPU-backed AI service | 24 hours |

The program totals approximately **214 hours**. At 18–24 hours per week, the realistic range is roughly 9–12 weeks. The calendar is a planning aid, not a reason to advance before a completion gate passes.

## Target credentials

- HashiCorp Terraform Associate (004)
- Kubernetes and Cloud Native Associate (KCNA)
- Certified Kubernetes Administrator (CKA) as a stretch goal
- NVIDIA Certified Associate: AI Infrastructure and Operations (NCA-AIIO)
- Microsoft Security, Compliance, and Identity Fundamentals (SC-900)
- Microsoft AI-901 or NVIDIA Generative AI LLM Associate only where it aligns with completed capstone work

## Repository structure

```text
terra-kube-ansible/
├── README.md
├── terraform/
├── ansible/
├── kubernetes/
├── integrated-labs/
├── gitops/
├── pipelines/
├── applications/
├── docs/
│   ├── index.html
│   ├── learning-dashboard.md
│   ├── architecture/
│   └── evidence/
└── scripts/
```

Directories are created as their implementation phases begin; empty placeholders are not required.

## Secrets warning

> [!WARNING]
> Do not commit passwords, API tokens, private keys, live kubeconfig files, unencrypted application secrets, `.tfvars` files containing secrets, or raw Ansible Vault password files. Use environment variables, ignored local files, Ansible Vault, SOPS/age, Sealed Secrets, or approved platform secret stores as appropriate.

## Completion standard

A phase is complete only when:

1. the implementation works;
2. the phase completion gate passes;
3. code and configuration are committed;
4. the runbook covers deployment, validation, troubleshooting, and teardown;
5. evidence is recorded or linked;
6. all roadmap issue tasks are checked.
