# Phase 06 — GitOps with Argo CD

- **Progress checklist:** [Argo CD issue](https://github.com/en4ble1337/terra-kube-ansible/issues/6)
- **Outcome:** Git becomes the source of truth for application deployment, drift repair, and rollback.
- **Estimated effort:** 5–6 sessions

## Session sequence

1. Learn reconciliation, desired state, drift, pull-based deployment, and promotion.
2. Design the repository's cluster and application layout.
3. Install and secure a pinned Argo CD release.
4. Deploy an application from Git with a restricted Argo CD project.
5. Enable and test automated sync, pruning, and self-healing.
6. Introduce drift and a failed release, then recover through Git and document break-glass procedures.

## Completion gate

A Git commit deploys the workload, an out-of-band change is detected and repaired, and a broken release is recovered through a documented rollback.

## Official reference

- [Argo CD documentation](https://argo-cd.readthedocs.io/en/stable/)
