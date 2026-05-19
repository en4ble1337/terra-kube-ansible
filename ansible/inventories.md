# Ansible Inventory Management

An **Inventory** defines the catalog of hosts (virtual machines, containers, and bare metal servers) that Ansible manages. It can be declared as a simple static file, or generated dynamically using plugins.

---

## Static Inventory Example

Static inventories are structured as simple files (usually using the INI format, though YAML is also supported).

Create a sample inventory file named `hosts.ini`:

```ini
[webservers]
web-01.lab.local ansible_host=10.1.20.101 ansible_user=root
web-02.lab.local ansible_host=10.1.20.102 ansible_user=root

[dbservers]
db-01.lab.local ansible_host=10.1.20.103 ansible_user=root

[cache]
redis-01.lab.local ansible_host=10.1.20.104 ansible_user=root
```

---

## Grouping and Parent Groups

Ansible supports group nesting to easily manage configurations globally or selectively.

```ini
# A parent group containing all sub-groups for application deployments
[app_servers:children]
webservers
dbservers
cache

# Variables applied globally to all nodes in the app_servers children groups
[app_servers:vars]
ansible_port=22
dns_nameserver=1.1.1.1
```

---

## Inventory Hygiene Best Practices

1. **Avoid Hardcoding Root Credentials**: Do not specify explicit passwords inside inventory files. Instead, use SSH key-based authentication:
   ```ini
   # Good: Relies on SSH keys loaded in ssh-agent
   node-01 ansible_host=10.0.0.5 ansible_user=ubuntu
   ```
2. **Organize Variables via `group_vars`**: Keep your inventories short and readable by moving variables out of the inventory file and into folder structures:
   ```text
   ansible/
   ├── inventories/
   │   └── lab.ini
   └── group_vars/
       ├── all.yml            # Variables applied to all hosts
       ├── webservers.yml     # Variables applied only to [webservers]
       └── dbservers.yml      # Variables applied only to [dbservers]
   ```
3. **Use DNS names**: Where possible, specify hostnames as the primary identifiers (e.g. `web-01.lab.local`) and bind the IP through `ansible_host=...` variables. This separates logical configurations from changing network layers.
