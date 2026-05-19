# Diagnostic & Troubleshooting Logbook

This logbook contains a curated list of common issues encountered during Terraform provisioning, Ansible configuration, and Kubernetes administration. Use it as a quick runbook to isolate, diagnose, and resolve lab environment issues.

---

## 1. Terraform Issues

### Issue 1.1: Proxmox API Authentication Failures
* **Symptom**:
  ```text
  Error: PVE API login failed - Status code 401: Connection refused
  ```
* **Root Cause**: The API token username, ID, or secret value is incorrect, or the user lacks appropriate permissions on the `/` path.
* **Diagnosis**:
  1. Check if the token format is `username@pve!token_name`.
  2. Verify if the token secret matches exactly.
* **Fix**:
  1. Log into the Proxmox UI.
  2. Navigate to **Datacenter** > **Users** > **API Tokens**.
  3. Ensure the token exists, check that it is not expired, and verify that appropriate roles (e.g., `PVEAdmin`) are assigned under **Permissions**.

### Issue 1.2: State Lock Contention
* **Symptom**:
  ```text
  Error: Error acquiring the state lock: Resource temporarily unavailable
  ```
* **Root Cause**: A previous Terraform run was interrupted or killed abruptly, leaving the local or remote state lock file active.
* **Fix**:
  * For local backend, search for a `.terraform.tfstate.lock.info` file and delete it if you are sure no other instance is running.
  * For remote backends, identify the Lock ID from the console and run:
    ```bash
    terraform force-unlock <LOCK_ID>
    ```

---

## 2. Ansible Issues

### Issue 2.1: Host Key Verification Failures
* **Symptom**:
  ```text
  fatal: [webservers]: FAILED! => {"msg": "Using a SSH password instead of a key is not possible because Host Key checking is enabled and sshpass does not support this."}
  ```
* **Root Cause**: The SSH fingerprint of the target host is not present in the control node's `~/.ssh/known_hosts` file.
* **Fix**:
  * Option A: Manually SSH into each VM once to accept the fingerprint.
  * Option B: Disable host key checking globally in your environment variables:
    ```bash
    export ANSIBLE_HOST_KEY_CHECKING=False
    ```
  * Option C: Set it directly in your `hosts.ini` variables:
    ```ini
    [all:vars]
    ansible_ssh_common_args='-o StrictHostKeyChecking=no'
    ```

### Issue 2.2: Ansible Vault Decryption Missing Password
* **Symptom**:
  ```text
  Decryption failed: Attempting to decrypt but no vault password was provided
  ```
* **Root Cause**: Ansible is trying to parse an encrypted file (like variables) without the decryption key.
* **Fix**:
  * Pass the password inline during runtime:
    ```bash
    ansible-playbook -i hosts.ini site.yml --ask-vault-pass
    ```
  * Or point to a vault password text file (ensure the file is in `.gitignore`!):
    ```bash
    ansible-playbook -i hosts.ini site.yml --vault-password-file ~/.ansible_vault_pass
    ```

---

## 3. Kubernetes Issues

### Issue 3.1: Pod Stuck in "Pending" State
* **Symptom**:
  ```bash
  kubectl get pods
  # NAME                     READY   STATUS    RESTARTS   AGE
  # nginx-78f5d6957c-abc12   0/1     Pending   0          5m
  ```
* **Diagnosis**: Run `kubectl describe pod <pod_name>` and inspect the "Events" log at the bottom.
* **Typical Causes**:
  * **Insufficient CPU/RAM**: The cluster nodes do not have enough unallocated resources to schedule the Pod.
  * **Unsatisfied PVC**: The Pod requests storage from a PVC that is still unbound (e.g., storage class not configured).
* **Fix**:
  * Check node resources using `kubectl top nodes` or `kubectl describe node <node_name>`.
  * Adjust the pod resource `requests` or `limits` in the deployment manifest.

### Issue 3.2: Pod Stuck in "CrashLoopBackOff" State
* **Symptom**:
  ```bash
  kubectl get pods
  # NAME                     READY   STATUS              RESTARTS   AGE
  # nginx-78f5d6957c-abc12   0/1     CrashLoopBackOff    3          4m
  ```
* **Root Cause**: The application container inside the Pod successfully started, but crashed shortly after. Kubernetes is repeatedly restarting it.
* **Diagnosis**: Inspect container runtime logs:
  ```bash
  kubectl logs <pod_name> --previous
  ```
* **Typical Causes**:
  * Incorrect container startup command or entrypoint configuration.
  * Missing critical application environment variables or configuration files.
  * Port conflicts on the node.
* **Fix**: Correct the configuration errors identified in the container logs and reapply the manifest.

### Issue 3.3: Service DNS Name Fails to Resolve Internally
* **Symptom**: Pods are unable to communicate with each other using the internal DNS format `<service_name>.<namespace>.svc.cluster.local`.
* **Diagnosis**: Spin up a temporary tool container to test CoreDNS:
  ```bash
  kubectl run busybox --image=busybox:1.28 --restart=Never -- rm -i --tty -- nslookup <service_name>
  ```
* **Fix**:
  1. Verify if CoreDNS pods are running:
     ```bash
     kubectl get pods -n kube-system -l k8s-app=kube-dns
     ```
  2. Ensure the selector labels in your Service YAML exactly match the labels declared on the target Deployment Pods.
