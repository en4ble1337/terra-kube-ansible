# Infra Forge Learning Lab

Hands-on learning repository for Terraform, Ansible, Kubernetes, GPU infrastructure, GitOps, identity, distributed applications, and AI workloads.

## Start here

### [Open the master training tracker](https://github.com/en4ble1337/terra-kube-ansible/issues/12)

That issue is the only progress page you need to bookmark.

## What to do today

1. Open the **Current focus** link in the master tracker.
2. Work on the first unchecked task.
3. Validate that it works.
4. Commit the code, configuration, or documentation.
5. Check the task in GitHub.

## Current phase

### [Phase 1 — Terraform: Infrastructure as Code](https://github.com/en4ble1337/terra-kube-ansible/issues/1)

Primary objective: build and rebuild the Proxmox VM foundation as code while preparing for the HashiCorp Terraform Associate certification.

Do not start the next phase until the current phase completion gate passes.

## Where work is stored

| Folder | Purpose |
|---|---|
| [`terraform/`](./terraform/) | Terraform code and Proxmox provisioning labs |
| [`ansible/`](./ansible/) | Inventories, playbooks, roles, and configuration automation |
| [`kubernetes/`](./kubernetes/) | kubeadm, K3s, GPU Operator, storage, ingress, and operations |
| [`integrated-labs/`](./integrated-labs/) | End-to-end Terraform, Ansible, and Kubernetes workflows |
| [`docs/`](./docs/) | Runbooks, architecture notes, troubleshooting, and evidence |
| [`scripts/`](./scripts/) | Supporting automation and validation scripts |

Folders for later phases can be created when those phases begin. There is no need to maintain empty structure in advance.

## Simple operating rules

- Work on **one phase at a time**.
- A task is complete only when it works and the result is committed or documented.
- Use GitHub Issues for progress tracking.
- Use the repository for code, configuration, notes, and evidence.
- Use ChatGPT for instruction, troubleshooting, reviews, and updating GitHub when requested.
- Do not create a second tracker in Excel, GitHub Projects, or another dashboard.

## Secrets warning

> [!WARNING]
> Do not commit passwords, API tokens, private keys, live kubeconfig files, unencrypted secrets, `.tfvars` files containing credentials, or raw Ansible Vault password files.
