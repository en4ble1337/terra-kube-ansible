# Ansible Configuration Management Labs

This section documents using **Ansible** to manage configuration states, software baselines, users, security hardening, and orchestration workflows across virtual machines and containers in the laboratory. By codifying our system setups, we avoid "snowflake servers" and ensure environment reproducibility.

## Lab Index

| Lab / Guide | Description | Key Focus | Link |
|---|---|---|---|
| **Inventory Setup** | Defining static and grouping layouts for host catalogs | Host Organization | [Inventories Guide](./inventories.md) |
| **Playbooks & Tasks** | Basic execution playbooks, checking syntax, and validation | Orchestration & Tasks | [Playbooks Guide](./playbooks.md) |
| **Roles Architecture** | Modularizing configuration code into structured roles | Code Organization | [Roles Guide](./roles.md) |
| **Ansible Vault** | Encrypting variables, secrets, and credentials safely | Secret Management | [Vault Guide](./vault.md) |

## Planned Topics

- **Ansible Basics**: Ad-hoc commands, syntax, module structure, and configuring default `ansible.cfg` files.
- **Inventories**: Transitioning from simple static host lists to dynamic inventory providers (e.g., querying Proxmox APIs for online VM IPs).
- **Playbooks**: Building multi-stage playbooks for rolling package upgrades, security benchmarks, and state verification.
- **Roles**: Standardizing role directories for generic operations such as user setup, SSH configuration, Docker runtimes, and monitoring exporters.
- **Idempotency**: Designing tasks that safely run multiple times without causing side effects or server state disruption.
- **Secrets Management**: Using Ansible Vault for environment credential variable injection during execution.
