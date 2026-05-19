# Kubernetes Persistent Storage

Stateless containers lose all filesystem updates when they crash or restart. To run databases, stateful caches, or log files, Kubernetes abstracts physical storage hardware (NFS, SAN, local disks, or cloud storage disks) using three core API resources:

---

## The Storage Core Model

1. **StorageClass (SC)**: A blueprint defining *how* storage is created dynamically. It defines the storage driver (provisioner) and parameters (e.g. SSD vs HDD).
2. **PersistentVolume (PV)**: The physical representation of a block of storage provisioned on the host or cloud. It is bounded to the cluster life.
3. **PersistentVolumeClaim (PVC)**: A user's request for storage. A developer requests "8GB of SSD storage" without knowing the physical details. Kubernetes matches the PVC to a SC, dynamically provisions a PV, and binds it to the container's pod.

---

## PersistentVolumeClaim Example (`app-pvc.yaml`)

This manifest requests 5GB of storage that can be read/written by a single node at a time:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: lab-pvc
spec:
  accessModes:
    - ReadWriteOnce  # The volume can be mounted as read-write by a single node
  resources:
    requests:
      storage: 5Gi  # Requested disk size
  # storageClassName: "local-path"  # Uncomment to target a specific StorageClass (default SC used if omitted)
```

---

## Mounting the PVC inside a Pod (Usage Example)

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: data-pod
spec:
  volumes:
    - name: storage-volume
      persistentVolumeClaim:
        claimName: lab-pvc  # Binds the PVC claim to this volume
  containers:
    - name: app
      image: alpine
      command: ["/bin/sh", "-c", "while true; do date >> /data/log.txt; sleep 10; done"]
      volumeMounts:
        - mountPath: "/data"  # Mount directory inside the container
          name: storage-volume
```

---

## Verification Commands

```bash
# Create the claim
kubectl apply -f app-pvc.yaml

# Monitor binding state (Status should transition from Pending to Bound)
kubectl get pvc -w

# List all available physical volumes in the cluster
kubectl get pv
```
