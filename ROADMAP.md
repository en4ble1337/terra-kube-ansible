# Learning Roadmap

## Objective

Build a complete platform-engineering and private-AI lab while earning selected foundational credentials that validate the underlying knowledge.

The sequence is dependency-driven. Terraform creates the machines. Ansible configures them. Kubernetes clusters them. Later phases add GPUs, storage, delivery controls, identity, distributed applications, and AI workloads.

## Phase sequence

| Phase | Depends on | Completion outcome | Credential alignment |
|---:|---|---|---|
| 1. Terraform | Existing Proxmox access | Three reproducible Ubuntu VMs and documented lifecycle operations | HashiCorp Terraform Associate (004) |
| 2. Ansible | Terraform VMs | Repeatable, idempotent Kubernetes-node configuration | Practical competency |
| 3. Kubernetes | Configured VMs | Functional kubeadm cluster and documented K3s comparison | KCNA; CKA stretch |
| 4. GPU Operator | Kubernetes and GPU passthrough | GPU advertised, scheduled, and monitored in Kubernetes | NVIDIA NCA-AIIO |
| 5. Storage | Kubernetes | Persistent workloads, backup, and restore | Practical competency |
| 6. GitOps | Kubernetes and Git | Reconciled deployment, drift repair, and rollback | Practical competency |
| 7. CI/CD | Git and Kubernetes | Automated validation, build, approval, and deployment | AZ-400 later |
| 8. Identity | Application and Entra access | OIDC authentication and authorization | SC-900 |
| 9. Event-driven systems | Kubernetes, storage, and GitOps | Durable asynchronous processing and recovery | Practical competency |
| 10. AI capstone | All prior phases | End-to-end authenticated GPU-backed AI service | NCA-AIIO; AI credential optional |

## Certification strategy

Certifications are checkpoints, not substitutes for labs.

### Commit to these first

1. **HashiCorp Terraform Associate (004)** after Phase 1.
2. **KCNA** after Phase 3 if you want a foundational Kubernetes badge.
3. **NVIDIA NCA-AIIO** after Phase 4 or the capstone.
4. **Microsoft SC-900** after Phase 8.

### Treat these as stretch goals

- **CKA** after substantially more command-line Kubernetes practice.
- **AZ-400** only after Azure administration and pipeline experience are strong enough to justify it.
- Additional AI credentials only when they align with the capstone rather than duplicating existing knowledge.

## Pacing

The program contains approximately 214 hours of planned work.

| Weekly effort | Expected duration |
|---:|---:|
| 18 hours | About 12 weeks |
| 21 hours | About 10 weeks |
| 24 hours | About 9 weeks |

Do not advance because a calendar week ended. Advance when the phase completion gate passes.

## Definition of done

A phase is complete only when:

- the implementation works;
- the documented validation passes;
- code and configuration are committed;
- the runbook explains deployment, troubleshooting, and teardown;
- evidence is recorded;
- the phase issue checklist is complete;
- the phase can be reproduced without undocumented manual repair.
