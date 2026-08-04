# Phase 04 — NVIDIA GPU Operator

- **Progress checklist:** [GPU Operator issue](https://github.com/en4ble1337/terra-kube-ansible/issues/4)
- **Prerequisite:** Working Kubernetes cluster and validated Proxmox PCIe passthrough
- **Outcome:** Kubernetes advertises, schedules, and monitors an NVIDIA GPU.
- **Estimated effort:** 6 sessions × approximately 3 hours

## Session sequence

1. Map the GPU, PCI devices, IOMMU group, passthrough plan, and rollback.
2. Pass the GPU into one worker VM and validate it independently of Kubernetes.
3. Review GPU Operator architecture, compatibility, driver strategy, and runtime integration.
4. Install a pinned operator release and inspect its components and ClusterPolicy.
5. Run a CUDA validation workload and enforce GPU-node placement.
6. Validate DCGM telemetry, create one controlled failure, and document uninstall and recovery.

## Completion gate

The worker reports allocatable `nvidia.com/gpu`, a requesting workload schedules to the correct node, CUDA succeeds, and telemetry confirms utilization.

## Official reference

- [NVIDIA GPU Operator documentation](https://docs.nvidia.com/datacenter/cloud-native/gpu-operator/latest/)
