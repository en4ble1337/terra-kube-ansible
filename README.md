# Infra Forge Learning Lab

A guided, hands-on learning path for Terraform, Ansible, Kubernetes, GPU infrastructure, storage, GitOps, CI/CD, identity, event-driven systems, and AI workloads.

This repository has one purpose: **tell you what to learn next, give you a lab to prove it, and preserve the working result.**

## Start here

### Current phase: [01 — Terraform](./01-terraform/README.md)

### Progress tracker: [START HERE — Training Progress](https://github.com/en4ble1337/terra-kube-ansible/issues/12)

Do not browse random folders looking for a starting point. Open the current phase guide and follow it from the first incomplete session.

## The operating model

Each phase contains three layers:

1. **Learn** — complete the assigned course or official material.
2. **Build** — implement the lab in Proxmox or the required external service.
3. **Prove** — validate it, document it, commit it, and update the GitHub issue.

The phase guide is the instruction manual. The GitHub issue is the checklist. The committed repository is the evidence.

## Daily workflow

For a normal three-hour study block:

| Time | Activity |
|---:|---|
| 30–45 min | Course or official study material |
| 90–120 min | Hands-on lab work |
| 20–30 min | Validation, notes, commit, and checklist update |

At the start of a session:

1. Open the current phase guide.
2. Find the first incomplete session.
3. Open the linked phase issue.
4. Complete only that session's learning and lab objectives.

At the end of a session:

1. Run the listed validation.
2. Record the result in the phase evidence file.
3. Commit and push the working change.
4. Check the corresponding issue task.

## Curriculum

| Phase | Guide | Primary outcome | Estimated effort |
|---:|---|---|---:|
| 1 | [Terraform](./01-terraform/README.md) | Rebuildable Proxmox VM infrastructure | 30 hours |
| 2 | [Ansible](./02-ansible/README.md) | Idempotent Linux and Kubernetes-node configuration | 18 hours |
| 3 | [Kubernetes](./03-kubernetes/README.md) | kubeadm cluster plus K3s comparison | 36 hours |
| 4 | [NVIDIA GPU Operator](./04-gpu-operator/README.md) | Schedulable and observable Kubernetes GPU worker | 18 hours |
| 5 | [Persistent Storage](./05-storage/README.md) | CSI-backed persistent workloads and recovery | 16 hours |
| 6 | [GitOps](./06-gitops/README.md) | Argo CD deployment, drift repair, and rollback | 16 hours |
| 7 | [CI/CD](./07-cicd/README.md) | Azure DevOps validation and deployment pipeline | 18 hours |
| 8 | [Identity and OIDC](./08-identity/README.md) | Entra-protected API with authorization | 18 hours |
| 9 | [Event-Driven Systems](./09-event-driven/README.md) | Resilient asynchronous microservices | 20 hours |
| 10 | [AI Capstone](./10-ai-capstone/README.md) | Authenticated, GPU-backed AI workflow | 24 hours |

See [ROADMAP.md](./ROADMAP.md) for dependencies, certification targets, and the complete sequence.

## Repository rules

- Work on **one phase at a time**.
- Do not mark a task complete because a video was watched.
- A task is complete when it works and the result is committed or documented.
- Proxmox is the default platform. Use public cloud only when the objective requires it.
- Use full VMs for the primary Kubernetes lab, not LXC.
- Keep secrets out of Git.
- Preserve failed attempts and root-cause notes when they teach something useful.
- Avoid building dashboards or project-management systems for this repository.

## Standard phase layout

As each phase progresses, create only the folders you actually need:

```text
01-terraform/
├── README.md          # learning guide
├── labs/              # working code and configuration
├── notes/             # concise technical notes and decisions
└── evidence/          # validation and completion evidence
```

Use the templates in [`templates/`](./templates/) when creating notes and evidence.

## Backup of the previous repository

The repository state before this rebuild is preserved on branch:

```text
backup/pre-rebuild-2026-08-03
```

It is reference material only. Do not use it as the active learning path.
