# Kubernetes Services

Pods in a Kubernetes cluster are dynamic and ephemeral. Since they can be killed and replaced at any time, their IP addresses constantly change. A **Service** provides a single, stable IP address and DNS hostname to route traffic reliably to a group of active Pods.

---

## Service Types at a High Level

| Type | Access Scope | Description | Use Case |
|---|---|---|---|
| **ClusterIP** | Internal Only | Exposes the Service on a cluster-internal IP. Default type. | Databases, backend API services, redis caching layers. |
| **NodePort** | External via Node IP | Exposes the Service on each Node's IP at a static port (30000-32767). | Simple development testing, on-premise static routing. |
| **LoadBalancer** | External via Cloud LB | Exposes the Service externally using a cloud provider's external load balancer. | Production web traffic ingress in AWS, Azure, or GCP. |

---

## Service Manifest Example (`nginx-service.yaml`)

This Service binds to port `80` internally and targets the `nginx` Pods exposed by the `nginx-deployment`:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  type: ClusterIP  # Change to NodePort or LoadBalancer if external routing is needed
  selector:
    app: nginx  # Must match the labels defined in the Deployment spec template
  ports:
    - protocol: TCP
      port: 80        # The port exposed by this service inside the cluster
      targetPort: 80  # The target port the Nginx container is listening on
```

---

## Service Management Commands

```bash
# Deploy the service
kubectl apply -f nginx-service.yaml

# List services in active namespace
kubectl get services

# Show detailed endpoints bound to this service
kubectl describe service nginx-service

# Query dns resolving inside a test container:
# Expected resolve hostname: nginx-service.<namespace>.svc.cluster.local
kubectl exec -it <pod-name> -- nslookup nginx-service
```
