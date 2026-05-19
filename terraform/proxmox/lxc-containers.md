# Terraform Proxmox LXC Container Management Guide

Detailed hands-on guide for provisioning and managing Linux Containers (LXC) on Proxmox VE using Terraform and the `telmate/proxmox` provider.

---

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Installation](#installation)
3. [Proxmox API Token Setup](#proxmox-api-token-setup)
4. [Quick Start - Basic Setup](#quick-start---basic-setup)
5. [Basic Setup - Multiple Containers](#basic-setup---multiple-containers)
6. [Advanced Setup - Using Variables](#advanced-setup---using-variables)
7. [Usage](#usage)
8. [Common Operations](#common-operations)
9. [Troubleshooting](#troubleshooting)
10. [Best Practices](#best-practices)
11. [Comparison: Basic vs Variables Setup](#comparison-basic-vs-variables-setup)

---

## Prerequisites

- Ubuntu 22.04 LTS server (or equivalent workstation terminal)
- Proxmox VE 7.x or 8.x hypervisor
- Administrative root or sudo access on the workstation
- Unrestricted network connectivity to the Proxmox VE management IP

---

## Installation

### Install Terraform on Ubuntu 22.04

```bash
# Update system packages
sudo apt-get update && sudo apt-get upgrade -y

# Install required dependencies
sudo apt-get install -y gnupg software-properties-common

# Add HashiCorp GPG key
wget -O- https://apt.releases.hashicorp.com/gpg | \
    gpg --dearmor | \
    sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg

# Verify the key fingerprint
gpg --no-default-keyring \
    --keyring /usr/share/keyrings/hashicorp-archive-keyring.gpg \
    --fingerprint

# Add HashiCorp repository
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
    https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
    sudo tee /etc/apt/sources.list.d/hashicorp.list

# Update and install Terraform
sudo apt-get update
sudo apt-get install terraform

# Verify installation
terraform version
```

Expected output:
```text
Terraform v1.x.x
on linux_amd64
```

### Install Additional Tools

```bash
# Install jq for JSON parsing and git for version control
sudo apt-get install -y jq git
```

---

## Proxmox API Token Setup

### Create API Token via Web UI

1. Log in to your Proxmox VE web management portal at `https://<your-proxmox-ip>:8006`.
2. Navigate to **Datacenter → Permissions → API Tokens**.
3. Click **Add**.
4. Fill in the parameters:
   - **User**: `root@pam`
   - **Token ID**: `terraform`
   - **Privilege Separation**: Unchecked (☐)
   - **Expire**: `never`
5. Click **Add**.
6. **IMPORTANT**: Copy the generated API Secret immediately. It is shown only once and cannot be retrieved later!

*Example formats:*
- **Token ID**: `root@pam!terraform`
- **Secret**: `433151eb-f03a-4ff9-9ecc-1bf9f3d9068e`

### Create API Token via CLI (Alternative)

Instead of the web interface, you can SSH directly into the Proxmox VE host and execute:

```bash
# SSH into Proxmox server
ssh root@your-proxmox-ip

# Create token
pveum user token add root@pam terraform --privsep=0

# Grant permissions
pveum acl modify / -token 'root@pam!terraform' -role Administrator
```

---

## Quick Start - Basic Setup

Perfect for deploying your very first test container.

### Step 1: Create Project Directory

```bash
mkdir -p ~/terraform-projects/proxmox-basic
cd ~/terraform-projects/proxmox-basic
```

### Step 2: Create `providers.tf`

Create the provider definition file using a heredoc:

```bash
cat > providers.tf << 'EOF'
terraform {
  required_version = ">= 1.1.0"
  required_providers {
    proxmox = {
      source  = "telmate/proxmox"
      version = ">= 2.9.5"
    }
  }
}

provider "proxmox" {
  pm_tls_insecure     = true
  pm_api_url          = "https://10.1.20.136:8006/api2/json"  # Change to your Proxmox IP
  pm_api_token_id     = "root@pam!terraform"
  pm_api_token_secret = "your-secret-token-here"  # Replace with your token
}
EOF
```

> [!NOTE]
> Always use port **8006** (default PVE API port) rather than 443 in `pm_api_url`.

### Step 3: Create `lxc_container.tf`

Create the baseline resource definition file using a heredoc:

```bash
cat > lxc_container.tf << 'EOF'
resource "proxmox_lxc" "basic-container" {
  target_node  = "ai-node1"  # Change to your Proxmox node name
  hostname     = "test-container-01"
  ostemplate   = "local:vztmpl/ubuntu-22.04-standard_22.04-1_amd64.tar.zst"
  password     = "rootroot"  # Change this to a secure password
  unprivileged = true
  
  rootfs {
    storage = "local-lvm"  # Change to your storage pool name
    size    = "8G"
  }
  
  network {
    name   = "eth0"
    bridge = "vmbr0"
    ip     = "dhcp"
    ip6    = "dhcp"
  }
  
  features {
    nesting = true
  }
}
EOF
```

### Step 4: Initialize and Apply

```bash
# Initialize Terraform
terraform init

# Preview the planned deployment changes
terraform plan

# Deploy the container
terraform apply
```

Type `yes` when prompted.

### Step 5: Verify

```bash
# View the created container details
terraform show

# List currently tracked resources in state
terraform state list
```

---

## Basic Setup - Multiple Containers

To spin up multiple containers without writing variable loops, declare multiple resource blocks:

### Example `lxc_container.tf` (Multiple Containers)

```hcl
# First container
resource "proxmox_lxc" "web-server" {
  target_node  = "ai-node1"
  hostname     = "web-server-01"
  ostemplate   = "local:vztmpl/ubuntu-22.04-standard_22.04-1_amd64.tar.zst"
  password     = "rootroot"
  unprivileged = true
  
  rootfs {
    storage = "local-lvm"
    size    = "20G"
  }
  
  network {
    name   = "eth0"
    bridge = "vmbr0"
    ip     = "dhcp"
    ip6    = "dhcp"
  }
  
  features {
    nesting = true
  }
}

# Second container
resource "proxmox_lxc" "db-server" {
  target_node  = "ai-node1"
  hostname     = "db-server-01"
  ostemplate   = "local:vztmpl/ubuntu-22.04-standard_22.04-1_amd64.tar.zst"
  password     = "rootroot"
  unprivileged = true
  cores        = 4
  memory       = 4096
  
  rootfs {
    storage = "local-lvm"
    size    = "50G"
  }
  
  network {
    name   = "eth0"
    bridge = "vmbr0"
    ip     = "dhcp"
    ip6    = "dhcp"
  }
  
  features {
    nesting = true
  }
}

# Third container
resource "proxmox_lxc" "cache-server" {
  target_node  = "ai-node1"
  hostname     = "cache-server-01"
  ostemplate   = "local:vztmpl/ubuntu-22.04-standard_22.04-1_amd64.tar.zst"
  password     = "rootroot"
  unprivileged = true
  
  rootfs {
    storage = "local-lvm"
    size    = "10G"
  }
  
  network {
    name   = "eth0"
    bridge = "vmbr0"
    ip     = "dhcp"
    ip6    = "dhcp"
  }
  
  features {
    nesting = true
  }
}
```

Apply all blocks:
```bash
terraform plan
terraform apply -auto-approve
```

---

## Advanced Setup - Using Variables

For production, large architectures, or scale-out nodes, manage configs through structured files.

### Recommended Project Layout
```text
~/terraform-projects/proxmox-lxc/
├── .gitignore
├── README.md
├── providers.tf
├── variables.tf
├── lxc_containers.tf
├── outputs.tf
├── terraform.tfvars          # NEVER commit this to git
├── plans/                    # Target directory for execution outputs
└── backups/                  # Manual state backups folder
```

### Complete Advanced Configurations

#### 1. `providers.tf`
Without hardcoded secrets, using environment variables automatically.

```hcl
terraform {
  required_version = ">= 1.1.0"
  required_providers {
    proxmox = {
      source  = "telmate/proxmox"
      version = ">= 2.9.5"
    }
  }
}

provider "proxmox" {
  pm_tls_insecure = true
}
```

Load your secrets into your shell environment:
```bash
export PM_API_TOKEN_ID="root@pam!terraform"
export PM_API_TOKEN_SECRET="your-secret-token-here"
export PM_API_URL="https://10.1.20.136:8006/api2/json"
```

#### 2. `variables.tf`

```hcl
variable "containers" {
  description = "Map of LXC containers to create with their specifications"
  type = map(object({
    hostname    = string
    vmid        = optional(number)
    cores       = optional(number, 1)
    memory      = optional(number, 512)
    swap        = optional(number, 512)
    disk_size   = optional(string, "8G")
    storage     = optional(string, "local-lvm")
    ip          = optional(string, "dhcp")
    ip6         = optional(string, "dhcp")
    gateway     = optional(string, null)
    nameserver  = optional(string, null)
    searchdomain = optional(string, null)
    bridge      = optional(string, "vmbr0")
    start       = optional(bool, false)
    onboot      = optional(bool, false)
    protection  = optional(bool, false)
    unprivileged = optional(bool, true)
    nesting     = optional(bool, true)
    description = optional(string, "")
  }))
  
  default = {
    web-server = {
      hostname    = "web-01"
      vmid        = 101
      cores       = 2
      memory      = 2048
      disk_size   = "20G"
      description = "Web server container"
    }
    db-server = {
      hostname    = "db-01"
      vmid        = 102
      cores       = 4
      memory      = 4096
      disk_size   = "50G"
      description = "Database server container"
    }
    cache-server = {
      hostname    = "cache-01"
      vmid        = 103
      cores       = 2
      memory      = 1024
      disk_size   = "10G"
      description = "Redis cache server"
    }
  }
}

variable "target_node" {
  description = "Proxmox node name"
  type        = string
  default     = "ai-node1"
}

variable "ostemplate" {
  description = "LXC OS template path"
  type        = string
  default     = "local:vztmpl/ubuntu-22.04-standard_22.04-1_amd64.tar.zst"
}

variable "default_password" {
  description = "Default root password for containers"
  type        = string
  default     = "changeme"
  sensitive   = true
}

variable "ssh_public_keys" {
  description = "SSH public keys to add to containers"
  type        = string
  default     = ""
}
```

#### 3. `lxc_containers.tf`

```hcl
resource "proxmox_lxc" "containers" {
  for_each = var.containers
  
  # Basic settings
  target_node  = var.target_node
  hostname     = each.value.hostname
  vmid         = each.value.vmid
  description  = each.value.description
  ostemplate   = var.ostemplate
  password     = var.default_password
  unprivileged = each.value.unprivileged
  start        = each.value.start
  onboot       = each.value.onboot
  protection   = each.value.protection
  
  # SSH keys
  ssh_public_keys = var.ssh_public_keys != "" ? var.ssh_public_keys : null
  
  # Resource allocation
  cores      = each.value.cores
  memory     = each.value.memory
  swap       = each.value.swap
  
  # Root filesystem
  rootfs {
    storage = each.value.storage
    size    = each.value.disk_size
  }
  
  # Network configuration
  network {
    name   = "eth0"
    bridge = each.value.bridge
    ip     = each.value.ip
    ip6    = each.value.ip6
    gw     = each.value.gateway
  }
  
  # Container features
  features {
    nesting = each.value.nesting
  }
  
  # Nameserver configuration (optional)
  nameserver = each.value.nameserver
  searchdomain = each.value.searchdomain
}
```

#### 4. `outputs.tf`

```hcl
output "container_info" {
  description = "Information about created containers"
  value = {
    for k, v in proxmox_lxc.containers : k => {
      id       = v.id
      vmid     = v.vmid
      hostname = v.hostname
      ip       = v.network[0].ip
    }
  }
}

output "container_ids" {
  description = "Map of container names to VMIDs"
  value = {
    for k, v in proxmox_lxc.containers : k => v.vmid
  }
}
```

#### 5. `terraform.tfvars`

> [!WARNING]
> Keep `terraform.tfvars` git-ignored at all times. Do not push this file!

```hcl
# Override target node
target_node = "pve-node2"

# Override default password
default_password = "MySecurePassword123!"

# Add SSH keys
ssh_public_keys = <<-EOT
  ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQC... user@host
EOT

# Override container definitions
containers = {
  production-web = {
    hostname  = "prod-web-01"
    vmid      = 201
    cores     = 4
    memory    = 8192
    disk_size = "100G"
    ip        = "10.0.0.10/24"
    gateway   = "10.0.0.1"
  }
  production-db = {
    hostname  = "prod-db-01"
    vmid      = 202
    cores     = 8
    memory    = 16384
    disk_size = "200G"
    ip        = "10.0.0.11/24"
    gateway   = "10.0.0.1"
  }
}
```

---

## Usage

### Initial Setup
```bash
# Navigate to project directory
cd ~/terraform-projects/proxmox-lxc

# Initialize directory (downloads provider plugins)
terraform init

# Validate configuration syntax
terraform validate

# Re-format code cleanly
terraform fmt
```

### Save and Review Plans
```bash
# Saved execution plan output
terraform plan -out=plans/deployment-$(date +%Y%m%d-%H%M%S).tfplan

# Show details of saved execution plan
terraform show plans/deployment-*.tfplan
```

### Apply Changes
```bash
# Standard apply
terraform apply

# Apply auto-approved (use with caution)
terraform apply -auto-approve

# Apply from specific plan output
terraform apply plans/deployment-*.tfplan
```

### Teardown / Destroy
```bash
# Destroy all infrastructure
terraform destroy

# Targetted destroy
terraform destroy -target='proxmox_lxc.containers["web-server"]'
```

---

## Common Operations

### Add a Container (Variables)
Add a new map block to the `containers` default in `variables.tf`:
```hcl
  app-server = {
    hostname  = "app-01"
    vmid      = 104
    cores     = 2
    memory    = 2048
    disk_size = "15G"
  }
```
Then run:
```bash
terraform plan
terraform apply
```

### Remove a Container (Variables)
Simply delete the container's block within the default map configuration inside `variables.tf` and apply.

### Import an Existing Container
If a container was created manually in the Proxmox UI and needs to be imported into Terraform:
```bash
# Variables method:
terraform import 'proxmox_lxc.containers["existing"]' ai-node1/lxc/100
```

---

## Troubleshooting

### Connection Refused (TCP 443)
- **Symptom**: `dial tcp <ip>:443: connect: connection refused`
- **Fix**: Check `pm_api_url` parameter. Proxmox VE UI and API runs on port **8006** (e.g. `https://10.1.20.136:8006/api2/json`).

### Authentication Failures
- **Symptom**: `authentication failure`
- **Fix**: Verify correct Token ID format (`root@pam!terraform` - note the exclamation mark). Verify privileges in Proxmox (must have Administrator permissions). Check that **Privilege Separation** is unchecked.

### Template Not Found
- **Symptom**: `template not found`
- **Fix**: Ensure PVE has downloaded the template locally first:
```bash
pveam update
pveam available | grep ubuntu-22.04
pveam download local ubuntu-22.04-standard_22.04-1_amd64.tar.zst
```

---

## Best Practices

1. **Keep Secrets Secret**: Never commit `.tfvars` or shell scripts setting `PM_API_TOKEN_SECRET` to GitHub.
2. **Nesting Features**: Ensure `nesting = true` is enabled in `features { }` on LXC resources. This is required for running nested container tools (like Docker or systemd workloads) inside LXC.
3. **Use Unprivileged containers**: Standardize on `unprivileged = true` unless a specific raw device mount is required. This increases container isolation and host security.
4. **Always Plan**: Review the execution modifications with `terraform plan` before executing any broad `terraform apply`.

---

## Comparison: Basic vs Variables Setup

| Feature | Basic Setup | Variables Setup |
|---------|-------------|-----------------|
| **Complexity** | ⭐ Simple | ⭐⭐⭐ Moderate |
| **Best for** | 1-3 containers | 5+ containers |
| **Reusability** | Low | High |
| **Maintenance** | Copy-paste changes | Change in one place |
| **Learning curve** | Easy | Moderate |
| **Scalability** | Poor | Excellent |
