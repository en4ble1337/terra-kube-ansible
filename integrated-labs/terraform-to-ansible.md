# Integrated Lab: Terraform to Ansible Handoff

This lab guides you through bridging the gap between **infrastructure provisioning (Terraform)** and **system configuration (Ansible)**. It details how to automatically generate an Ansible inventory directly from Terraform resource outputs.

---

## The Handoff Challenge

When Terraform provisions new virtual machines (e.g., in Proxmox), the resulting IP addresses are dynamically assigned or generated. To automate further, Ansible needs these exact IP addresses to connect and configure the systems. 

Manual copying and pasting of IPs breaks the automation pipeline. We solve this by having Terraform dynamically write an Ansible `hosts.ini` file using the `local_file` resource.

---

## Step-by-Step Implementation

### 1. Define Terraform Virtual Machines and Outputs

Suppose you use Terraform to spin up two virtual machines (one manager node, one worker node). In your Terraform project, define the resources and outputs:

```hcl
# terraform/main.tf (Snippet)

resource "proxmox_lxc" "web_nodes" {
  count        = 2
  target_node  = "pve"
  hostname     = "web-node-0${count.index + 1}"
  ostemplate   = "local:vztmpl/ubuntu-22.04-standard_22.04-1_amd64.tar.zst"
  cores        = 1
  memory       = 1024
  
  rootfs {
    storage = "local-lvm"
    size    = "8G"
  }

  network {
    name   = "eth0"
    bridge = "vmbr0"
    ip     = "192.168.1.15${count.index}/24"
    gw     = "192.168.1.1"
  }
}
```

### 2. Generate the Ansible Inventory with `local_file`

Add a `local_file` resource to your Terraform configuration. This resource uses string interpolation to build a clean Ansible INI inventory format and write it to disk:

```hcl
# terraform/inventory.tf

resource "local_file" "ansible_inventory" {
  filename = "${path.module}/../../ansible/hosts.ini"
  content  = <<EOT
[webservers]
%{ for index, host in proxmox_lxc.web_nodes ~}
${host.hostname} ansible_host=${split("/", host.network[0].ip)[0]} ansible_user=root
%{ endfor ~}

[all:vars]
ansible_ssh_private_key_file=~/.ssh/id_rsa
ansible_ssh_common_args='-o StrictHostKeyChecking=no'
EOT
}
```

*Note: The `split("/", host.network[0].ip)[0]` cleans up the CIDR subnet notation (e.g. `192.168.1.150/24` to `192.168.1.150`) so Ansible can connect.*

---

## Verifying the Handoff

Once you apply your Terraform project, follow these validation steps:

### 1. Provision Infrastructure
Run Terraform to provision the nodes and generate the inventory:
```bash
terraform apply -auto-approve
```

### 2. Verify File Creation
Check that the inventory has been successfully generated in your `ansible` directory:
```bash
cat ../ansible/hosts.ini
```

**Expected Output:**
```ini
[webservers]
web-node-01 ansible_host=192.168.1.150 ansible_user=root
web-node-02 ansible_host=192.168.1.151 ansible_user=root

[all:vars]
ansible_ssh_private_key_file=~/.ssh/id_rsa
ansible_ssh_common_args='-o StrictHostKeyChecking=no'
```

### 3. Test Connectivity with Ansible Ping
Verify that Ansible can authenticate and reach the new servers using the generated file:
```bash
ansible all -i ../ansible/hosts.ini -m ping
```

**Expected Success Output:**
```json
web-node-01 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
web-node-02 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```

---

## Best Practices

> [!WARNING]
> Ensure the generated `hosts.ini` file is added to your `.gitignore` file. It contains environment-specific IP addresses and local system credentials which should never be checked into git.

> [!TIP]
> Use SSH Keys instead of passwords for authenticating from Terraform to the provisioned VMs. Standardize your public key across cloud-init templates so Ansible can instantly authenticate using its target private key.
