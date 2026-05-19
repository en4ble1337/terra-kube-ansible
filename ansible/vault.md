# Ansible Vault Secure Secret Management

**Ansible Vault** is a feature that allows you to keep sensitive data (such as API keys, passwords, database credentials, and SSL certificates) in encrypted files rather than plain text in your repository.

---

## Core Vault Commands

You interact with encrypted files using the `ansible-vault` command-line utility.

### 1. Create a New Encrypted File
```bash
ansible-vault create group_vars/all/vault.yml
```
*Prompts for password twice and opens your terminal editor (usually nano/vim) to write encrypted keys.*

### 2. View an Encrypted File
```bash
ansible-vault view group_vars/all/vault.yml
```
*Prompts for the vault password and prints the decrypted content to stdout.*

### 3. Edit an Existing File
```bash
ansible-vault edit group_vars/all/vault.yml
```
*Decrypts, opens the editor, and re-encrypts the file automatically upon saving.*

### 4. Encrypt an Existing Plaintext File
```bash
ansible-vault encrypt my_secrets.txt
```

### 5. Decrypt an Encrypted File
```bash
ansible-vault decrypt my_secrets.txt
```

---

## Running Playbooks with Vault Secrets

To execute a playbook that consumes vault variables, you must supply the password to Ansible during invocation:

```bash
# Method 1: Prompt for password on screen
ansible-playbook -i inventories/hosts.ini baseline_system.yml --ask-vault-pass

# Method 2: Reference a git-ignored plaintext password file
ansible-playbook -i inventories/hosts.ini baseline_system.yml --vault-password-file .vault_pass
```

---

## Security Warnings & Git Hygiene

> [!WARNING]
> - **NEVER commit a vault password file**: Ensure `.vault_pass` is added to your `.gitignore` immediately to prevent committing your main encryption key.
> - **Only commit `$ANSIBLE_VAULT;...` prefixed files**: Open vault files in plain text before committing to ensure the content looks like an encrypted block, and not plain YAML.
