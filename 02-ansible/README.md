# Phase 02 — Ansible

- **Progress checklist:** [Ansible issue](https://github.com/en4ble1337/terra-kube-ansible/issues/2)
- **Prerequisite:** Terraform completion gate
- **Outcome:** Newly created Ubuntu VMs become Kubernetes-ready through an idempotent playbook.
- **Estimated effort:** 6 sessions × approximately 3 hours

## Session sequence

1. **Control node and SSH** — install Ansible, configure SSH keys, inventory the Terraform-created hosts, and validate `ansible all -m ping`.
2. **Core concepts** — inventories, variables, facts, modules, playbooks, handlers, templates, tags, and collections.
3. **Baseline role** — users, packages, time synchronization, updates, utilities, and service configuration.
4. **Kubernetes-node role** — swap, kernel modules, sysctl, containerd, repositories, and required packages.
5. **Secrets and quality** — Ansible Vault or equivalent, assertions, `ansible-lint`, syntax checks, and safe variable handling.
6. **Idempotency and break/fix** — run twice, prove no unintended changes, introduce a controlled error, troubleshoot it, and document recovery.

## Deliverables

```text
02-ansible/
├── README.md
├── labs/
│   ├── inventory/
│   ├── playbooks/
│   └── roles/
├── notes/
└── evidence/completion.md
```

## Completion gate

A newly provisioned Terraform VM must become Kubernetes-ready without interactive edits. A second complete playbook run must report no unintended changes.

## Official reference

- [Ansible documentation](https://docs.ansible.com/ansible/latest/index.html)
