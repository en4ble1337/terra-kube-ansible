# Phase 09 — Event-Driven Microservices

- **Progress checklist:** [Event-driven issue](https://github.com/en4ble1337/terra-kube-ansible/issues/9)
- **Default broker:** NATS JetStream unless a documented requirement justifies another choice
- **Outcome:** Durable asynchronous processing with retries, idempotency, observability, and recovery.
- **Estimated effort:** 6–7 sessions

## Session sequence

1. Define a narrow producer, worker, and result flow plus failure paths.
2. Design versioned events with IDs, timestamps, source, type, and correlation.
3. Deploy the broker with persistence and resource limits.
4. Build and deploy producer and consumer services.
5. Implement acknowledgements, retry, backoff, failed-message handling, and idempotency.
6. Add structured logs, metrics, backlog visibility, and independent worker scaling.
7. Run broker/consumer outages and invalid-event tests, then deploy through GitOps and CI/CD.

## Completion gate

Queued work survives a controlled outage, duplicate delivery does not duplicate results, and invalid messages are rejected or quarantined.

## Official references

- [NATS documentation](https://docs.nats.io/)
- [CloudEvents](https://cloudevents.io/)
