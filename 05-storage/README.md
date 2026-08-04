# Phase 05 — Persistent Storage

- **Progress checklist:** [Storage issue](https://github.com/en4ble1337/terra-kube-ansible/issues/5)
- **Primary platform:** Kubernetes plus a dedicated Synology NFS share
- **Outcome:** Dynamically provisioned storage survives pod replacement and supports documented backup and restore.
- **Estimated effort:** 5–6 sessions

## Session sequence

1. Learn PVs, PVCs, StorageClasses, CSI, access modes, binding, reclaim, and expansion.
2. Build and secure a dedicated Synology NFS share; validate UID/GID and node access.
3. Install a pinned NFS CSI driver and create a StorageClass.
4. Test a PVC and StatefulSet through deletion, rescheduling, and reattachment.
5. Test retain/delete behavior, expansion, and basic performance.
6. Perform a snapshot or backup restore and document failure modes.

## Completion gate

A stateful workload retains data through pod replacement, and intentionally removed test data is recovered through a documented restore procedure.

## Official references

- [Kubernetes persistent volumes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)
- [Kubernetes CSI NFS driver](https://github.com/kubernetes-csi/csi-driver-nfs)
