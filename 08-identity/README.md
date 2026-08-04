# Phase 08 — Microsoft Entra, OIDC, and Authorization

- **Progress checklist:** [Identity issue](https://github.com/en4ble1337/terra-kube-ansible/issues/8)
- **Outcome:** A Kubernetes-hosted API validates Microsoft Entra tokens and enforces explicit permissions.
- **Estimated effort:** 6 sessions

## Session sequence

1. Learn authentication, authorization, OAuth 2.0, OIDC, PKCE, tokens, scopes, roles, claims, and consent.
2. Register the protected API and define its permission model.
3. Register the client and obtain tokens through a maintained library.
4. Validate issuer, audience, signature, expiry, and required claims in the API.
5. Deploy the API and test missing, invalid, underprivileged, and authorized requests.
6. Review logs, rotate credentials, create a threat model, and map the work to SC-900 objectives.

## Completion gate

Recorded tests prove that missing, invalid, underprivileged, and authorized requests produce the expected results.

## Official reference

- [Microsoft identity platform OIDC](https://learn.microsoft.com/en-us/entra/identity-platform/v2-protocols-oidc)
