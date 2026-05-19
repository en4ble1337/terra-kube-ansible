# Terraform Provisioning Labs

This section documents using **Terraform** to provision hypervisor and cloud infrastructure in a programmatic, repeatable manner. In this lab environment, Terraform is used to provision Proxmox virtual resources (LXC containers and VMs) and manage Kubernetes objects.

## Lab Index

| Lab / Guide | Description | Target Provider | Link |
|---|---|---|---|
| **Proxmox LXC Container Management** | Step-by-step guide to provisioning containers on Proxmox VE | `telmate/proxmox` | [LXC Guide](./proxmox/lxc-containers.md) |
| **Proxmox VM Provisioning** | *Planned* - Bootstrapping VMs with cloud-init | `telmate/proxmox` | [Proxmox Overview](./proxmox/) |

## Planned Topics

- **Terraform Basics**: Core workflow (`init`, `plan`, `apply`, `destroy`), configuration syntax, and formatting.
- **Providers**: Integrating and configuring third-party providers (e.g., Telmate Proxmox, Kubernetes, Helm).
- **Variables**: Using dynamic inputs, maps, and sensitive variable management.
- **Outputs**: Structuring resource state properties to print to stdout or consume downstream (e.g., in Ansible).
- **State Management**: Local state lifecycles, backup strategies, and drift resolution (`terraform refresh`/`import`).
- **Modules**: Abstracting infrastructure configurations into reusable, parameter-driven components.
- **Proxmox VE Integration**: Automating container templates, networking, bridge allocation, and resource quotas.
- **Kubernetes Provider**: Managing namespaces, deployments, and cluster configurations directly through Terraform.
- **Remote State**: *Future* - Transitioning from local state files to robust remote storage (e.g., S3/Consul/HTTP) with locking.
