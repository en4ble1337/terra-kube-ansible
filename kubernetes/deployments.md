# Kubernetes Deployments

A **Deployment** provides declarative updates for Pods and ReplicaSets. It instructs Kubernetes how to create, update, roll back, and self-heal your application containers.

---

## Deployment Manifest Example (`nginx-deployment.yaml`)

Create a basic file defining a load-balanced set of Nginx web servers:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3  # Maintain exactly 3 running pod instances at all times
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:1.25.3
          ports:
            - containerPort: 80
          resources:
            limits:
              cpu: "500m"
              memory: "256Mi"
            requests:
              cpu: "100m"
              memory: "128Mi"
```

---

## Deployment Lifecycle Commands

Use the following commands to manage this deployment inside your cluster:

### 1. Apply / Deploy
Deploy or update the resources inside the cluster:
```bash
kubectl apply -f nginx-deployment.yaml
```

### 2. Validate Deployment Status
Check if the desired pod counts match active replica states:
```bash
# Get deployment roll status
kubectl get deployments

# Track the deployment rollout progress in real-time
kubectl rollout status deployment/nginx-deployment

# Verify that all 3 pods have successfully transitioned to "Running"
kubectl get pods -l app=nginx
```

### 3. Edit Live Configuration (Ad-hoc)
```bash
kubectl edit deployment/nginx-deployment
```

### 4. Delete the Deployment
Safely teardown the pods and replica sets:
```bash
kubectl delete -f nginx-deployment.yaml
```
*Alternatively:*
```bash
kubectl delete deployment/nginx-deployment
```
