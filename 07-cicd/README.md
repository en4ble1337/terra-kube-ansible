# Phase 07 — CI/CD with Azure DevOps

- **Progress checklist:** [Azure DevOps issue](https://github.com/en4ble1337/terra-kube-ansible/issues/7)
- **Primary design:** Azure DevOps service with a low-privilege self-hosted agent on Proxmox
- **Outcome:** Pull-request validation, auditable artifacts, approval, image build, and controlled Kubernetes deployment.
- **Estimated effort:** 6 sessions

## Session sequence

1. Create the project and deploy the isolated self-hosted agent.
2. Validate Terraform formatting, initialization, validation, and static analysis.
3. Add Ansible and Kubernetes schema/lint checks.
4. Build and publish a small application image.
5. Deploy through a least-privilege Kubernetes identity with environment approval.
6. Add branch policy, create a failing run, diagnose it, and document agent recovery and credential boundaries.

## Completion gate

A code change triggers required validation, produces artifacts, observes an approval boundary, and deploys a controlled workload without exposing credentials.

## Official reference

- [Azure Pipelines documentation](https://learn.microsoft.com/en-us/azure/devops/pipelines/)
