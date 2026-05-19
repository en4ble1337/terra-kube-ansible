# Ansible Playbooks & Idempotency

**Playbooks** are the orchestration blueprints of Ansible. Written in YAML, they describe a list of configuration steps (called tasks) that are mapped to selected host groups.

---

## Basic Playbook Example

Here is a baseline playbook named `baseline_system.yml` that updates package repositories and installs essential utilities on Debian/Ubuntu hosts:

```yaml
---
- name: Configure baseline packages and utilities
  hosts: app_servers
  become: true  # Run tasks with sudo/root privileges
  
  tasks:
    - name: Ensure system package cache is updated (apt update)
      ansible.builtin.apt:
        update_cache: true
        cache_valid_time: 3600  # Avoid re-running apt update if run in last hour

    - name: Ensure baseline utility packages are present
      ansible.builtin.apt:
        name:
          - curl
          - wget
          - git
          - jq
          - tmux
          - htop
        state: present
```

---

## Understanding Idempotency

An operation is **idempotent** if running it multiple times yields the exact same state without causing unwanted side effects.

- **Non-idempotent approach**: Running `mkdir /tmp/myfolder` or downloading a file via `wget` raw bash scripts (each execution re-creates or fails if directories exist).
- **Idempotent approach**: Using Ansible's built-in `file` module:
  ```yaml
  - name: Ensure target directory exists
    ansible.builtin.file:
      path: /tmp/myfolder
      state: directory
  ```
  Ansible will check if the directory exists. If yes, it does nothing and reports `ok`. If no, it creates it and reports `changed`.

---

## Validation & Dry-Run Commands

Before applying a playbook to live servers, execute these safe validation commands:

### 1. Syntax Verification
Check for formatting, nesting, or spelling errors:
```bash
ansible-playbook -i inventories/hosts.ini baseline_system.yml --syntax-check
```

### 2. Execution Dry Run (Check Mode)
Preview changes without writing them to disk:
```bash
ansible-playbook -i inventories/hosts.ini baseline_system.yml --check
```

### 3. State Diff Analysis
Print precise file diffs of modifications that *will* occur during run:
```bash
ansible-playbook -i inventories/hosts.ini baseline_system.yml --check --diff
```
