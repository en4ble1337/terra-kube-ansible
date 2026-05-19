# Ansible Roles Architecture

**Roles** allow you to bundle tasks, handlers, variables, files, and templates into clean, modular, and reusable directories. Instead of creating a single massive playbook, you delegate execution to structured roles.

---

## Role Directory Layout

A standard role (e.g. `common`) is expected to match this file structure:

```text
ansible/roles/common/
├── defaults/
│   └── main.yml      # Lowest-precedence default variables
├── files/
│   └── motd.txt      # Static files to copy to target systems
├── handlers/
│   └── main.yml      # Service restarts triggered by tasks (notify)
├── meta/
│   └── main.yml      # Dependency declarations and author metadata
├── tasks/
│   └── main.yml      # List of primary tasks executed by this role
├── templates/
│   └── sshd.conf.j2  # Jinja2 template files parsed on target
└── vars/
    └── main.yml      # High-precedence variables for this role
```

---

## Planned Roles Blueprints

The following modular roles are planned for implementation as the laboratory grows:

### 1. `common`
- Standardizes host package catalogs (updates, utilities).
- Copies baseline configuration files (DNS setups, host headers, custom `motd`).
- configures systemd parameters (timezone, network services).

### 2. `users`
- Automates developer user accounts creation.
- Manages local administrator grouping permissions (`sudoers` passwordless configurations).
- Inject SSH public keys into users' `.ssh/authorized_keys` catalogs.

### 3. `docker`
- Installs repository configurations and dependencies for the Docker runtime engine.
- Configures storage drivers and background daemon endpoints.
- Automates container networks, volumes, and installs `docker-compose`.

### 4. `monitoring`
- Provisions monitoring collector daemons (e.g., Prometheus `node_exporter`).
- Automates log shippers (e.g., Vector, Promtail).
- Configures localized firewalls to secure metrics endpoints.

### 5. `kubernetes-prep`
- Disables memory swapping partitions (`swapoff -a` permanently in `/etc/fstab`).
- Configures kernel modules dependencies (`overlay`, `br_netfilter`).
- Modifies sysctl parameters for network bridging properties (`net.bridge.bridge-nf-call-iptables = 1`).
- Prepares base systems to receive cluster orchestrators.
