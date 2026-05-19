# Kubernetes Ingress Routing

While a Service of type `NodePort` or `LoadBalancer` exposes a single application to the internet, managing hundreds of cloud load balancers or individual ports becomes expensive and unmanageable. 

An **Ingress** acts as a reverse proxy and smart router at the cluster boundary (Layer 7). It consolidates routing rules (e.g. `domain.com/api` -> api-service) under a single entrypoint managed by an **Ingress Controller** (such as NGINX Ingress, Traefik, or HAProxy).

---

## Ingress Manifest Skeleton (`app-ingress.yaml`)

Below is a standard skeleton defining a routing rule for mapping host subdomains to backend Services:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: app-ingress
  annotations:
    # Tell the API to use the standard Nginx controller
    kubernetes.io/ingress.class: "nginx"
    # Enable SSL redirect (requires TLS configurations)
    nginx.ingress.kubernetes.io/ssl-redirect: "false"
spec:
  rules:
    - host: lab.local  # Matches request host header
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: nginx-service  # Targets our stable ClusterIP service
                port:
                  number: 80
```

---

## Deployment & Verification

> [!NOTE]
> An Ingress resource does nothing without an active **Ingress Controller** running in the cluster. Popular options include:
> - **NGINX Ingress Controller**: The vanilla community standard.
> - **Traefik**: Default lightweight ingress packaged inside `k3s`.

```bash
# Apply routing rules
kubectl apply -f app-ingress.yaml

# List ingress rules and associated external IPs
kubectl get ingress

# Detailed spec check
kubectl describe ingress app-ingress
```
