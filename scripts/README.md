# Automation Scripts & Helper Utilities

This directory holds utility shell and PowerShell scripts that simplify common operational tasks such as local state backups, inventory validation, and environment teardown.

---

## Script Manifest & Stubs

| Script Name | Purpose | Implementation Language | Recommended Command |
|---|---|---|---|
| **`backup-state.sh`** | Safely packs up your active Terraform state files | Bash (Linux/macOS) | `./backup-state.sh` |
| **`check-kubeconfig.sh`** | Validates internal cluster api health and loads context | Bash (Linux/macOS) | `./check-kubeconfig.sh` |
| **`clean-all.sh`** | Interactive dry-run script to purge all cluster pods and routes | Bash (Linux/macOS) | `./clean-all.sh` |

---

## Copy-Paste Code Templates

You can instantiate these helper scripts locally to quickly streamline your administrator workflows:

### 1. Terraform State Backup Script (`scripts/backup-state.sh`)

Create a script to pack local `.tfstate` files securely into a timestamped archive before running dangerous operations:

```bash
#!/usr/bin/env bash
# scripts/backup-state.sh
set -euo pipefail

BACKUP_DIR="./terraform/backups"
STATE_FILE="./terraform/terraform.tfstate"
TIMESTAMP=$(date +%Y%m%d_%H%M%S)

mkdir -p "$BACKUP_DIR"

if [ -f "$STATE_FILE" ]; then
    echo "Found local state file. Creating backup..."
    cp "$STATE_FILE" "$BACKUP_DIR/terraform_$TIMESTAMP.tfstate"
    echo "Backup successfully created at: $BACKUP_DIR/terraform_$TIMESTAMP.tfstate"
else
    echo "No active local state file found. Nothing to backup."
fi
```

### 2. Kubeconfig Status Check (`scripts/check-kubeconfig.sh`)

Ensure your environment is successfully communicating with your active cluster before starting deployments:

```bash
#!/usr/bin/env bash
# scripts/check-kubeconfig.sh
set -euo pipefail

if [ -z "${KUBECONFIG:-}" ]; then
    echo "Error: KUBECONFIG environment variable is not set."
    echo "Set it using: export KUBECONFIG=~/.kube/config-infra-forge"
    exit 1
fi

echo "Verifying API server connection for config: $KUBECONFIG"
if kubectl cluster-info > /dev/null 2>&1; then
    echo "Cluster API is responsive. Available Nodes:"
    kubectl get nodes -o wide
else
    echo "Error: Unable to connect to the cluster. Check your server IP and SSH connection."
    exit 1
fi
```

### 3. Teardown & Purge Utility (`scripts/clean-all.sh`)

A safe script to clean up active deployment pods and services to start labs fresh:

```bash
#!/usr/bin/env bash
# scripts/clean-all.sh
set -euo pipefail

echo "WARNING: This will delete active Deployments, Services, and Ingress resources in the default namespace."
read -p "Are you sure you want to proceed? (y/N) " -n 1 -r
echo
if [[ $REPLY =~ ^[Yy]$ ]]; then
    echo "Purging resources..."
    kubectl delete deployments --all
    kubectl delete services --all -l 'app!=kubernetes' # Protect core API service
    kubectl delete ingress --all
    echo "Default namespace successfully cleared."
else
    echo "Teardown aborted."
fi
```
