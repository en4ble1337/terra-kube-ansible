# Infra Forge Lab

Hands-on lab repository for learning Terraform, Ansible, Kubernetes, and platform engineering workflows. It serves as a personal lab notebook and implementation reference for provisioning infrastructure, configuring systems, and operating containerized workloads across local hypervisors, Linux systems, and cluster environments.

## Sections

| Section | Description |
|---|---|
| [Terraform](./terraform/) | Infrastructure provisioning labs (Proxmox LXC, VMs, providers) |
| [Ansible](./ansible/) | Configuration management labs (inventories, playbooks, roles, vault) |
| [Kubernetes](./kubernetes/) | Container orchestration labs (kubectl, deployments, services, ingress, Helm) |
| [Integrated Labs](./integrated-labs/) | Combined Terraform + Ansible + Kubernetes end-to-end workflows |
| [Docs](./docs/) | Troubleshooting, glossary, and reference notes |

## Current Labs

- [Terraform Proxmox LXC Containers](./terraform/proxmox/lxc-containers.md) — Provision and manage LXC containers as code on Proxmox VE.

## Learning Roadmap

| Lab Area | Description | Target Skills | Status |
|---|---|---|---|
| **Terraform** | Hypervisor & Cloud Provisioning | Provider Setup, State Management, Map Variables, Proxmox LXC | Ready (Initial Lab) |
| **Ansible** | System Configuration | Inventories, Playbooks, Roles, Secret Vaults, Package baselines | Placeholder |
| **Kubernetes** | Application Orchestration | kubectl basics, Deployments, Services, PVC storage, Helm Charts | Placeholder |
| **Integrated Labs** | End-to-End Automation | Terraform to Ansible handoff, Kubernetes nodes bootstrapping | Placeholder |

## Repo Structure

```text
infra-forge-lab/
├── README.md
├── .gitignore
├── terraform/
│   ├── README.md
│   └── proxmox/
│       ├── README.md
│       └── lxc-containers.md
├── ansible/
│   ├── README.md
│   ├── inventories.md
│   ├── playbooks.md
│   ├── roles.md
│   └── vault.md
├── kubernetes/
│   ├── README.md
│   ├── kubectl-basics.md
│   ├── deployments.md
│   ├── services.md
│   ├── ingress.md
│   ├── storage.md
│   └── helm.md
├── integrated-labs/
│   ├── README.md
│   ├── terraform-to-ansible.md
│   ├── terraform-to-kubernetes.md
│   └── full-platform-workflow.md
├── docs/
│   ├── troubleshooting.md
│   ├── glossary.md
│   └── references.md
└── scripts/
    └── README.md
```

## Secrets Warning

> [!WARNING]
> Do not commit passwords, credentials, API tokens, `.tfvars` files with secrets, kubeconfig files for live clusters, or raw Ansible Vault password files. All sensitive configurations should be git-ignored by default using the provided `.gitignore`.

## Status

- **Terraform**: Initial content active (Telmate Proxmox LXC guide imported).
- **Ansible**: Placeholder study guides ready (scaffolded with templates).
- **Kubernetes**: Placeholder study guides ready (scaffolded with templates).
- **Integrated Labs**: Placeholder guides created to map the automation workflow.
