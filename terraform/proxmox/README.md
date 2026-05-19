# Proxmox VE Provisioning Labs

This sub-section focuses on automating and managing hypervisor resources on **Proxmox Virtual Environment (PVE)** using Terraform. By treating our hypervisor as code, we can instantly spin up, modify, and teardown containerized or virtualized nodes for our laboratory experiments.

## Labs & Guides

- [Terraform Proxmox LXC Container Management Guide](./lxc-containers.md) — Comprehensive, battle-tested walkthrough of provisioning lightweight LXC containers using the `telmate/proxmox` provider. Supports basic single-resource layouts and advanced parameter maps.

## Roadmap & Future Labs

### 1. VM Provisioning via Cloud-Init
- Automate complete Virtual Machine lifecycles instead of lightweight containers.
- Bootstrap operating systems using Ubuntu cloud images and customized `cloud-init` configurations (managing default users, packages, and SSH keys automatically during boot).
- Build reusable VM templates for scale-out Kubernetes nodes.

### 2. Hypervisor Network & Storage Orchestration
- Provision custom virtual bridges (`vmbr1`, `vmbr2`) and VLANs dynamically.
- Manage persistent disk allocations across varied storage classes (e.g., Local LVM, NFS, Ceph, or ZFS).
- Setup automatic container template downloads via Proxmox storage APIs.
