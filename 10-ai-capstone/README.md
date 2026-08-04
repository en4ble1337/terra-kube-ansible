# Phase 10 — AI and Agentic Capstone

- **Progress checklist:** [AI capstone issue](https://github.com/en4ble1337/terra-kube-ansible/issues/10)
- **Outcome:** An authenticated API submits asynchronous AI jobs to a GPU worker, persists results, and exposes operational telemetry.
- **Estimated effort:** 8 sessions × approximately 3 hours

## Session sequence

1. Select one focused use case and define success criteria.
2. Select a model and serving runtime that fit available GPU memory.
3. Deploy a GPU-backed OpenAI-compatible inference endpoint with health and placement controls.
4. Build the Entra-protected request API and asynchronous job contract.
5. Build the GPU worker and persisted result workflow.
6. Add status, correlation, timeout, retry, failure handling, and one constrained tool-using agent capability.
7. Build through CI/CD, deploy through Argo CD, and add application, queue, latency, error, and GPU metrics.
8. Run controlled failures, measure performance, document the threat model, and record the end-to-end demonstration.

## Completion gate

An authorized user receives a job ID, the job is processed asynchronously on the GPU, the result persists, observability confirms the path, and a controlled failure is recovered through documented operations.
