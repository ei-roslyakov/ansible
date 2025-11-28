# APT Package Management

This directory contains Ansible playbooks for managing APT packages on Debian/Ubuntu systems.

## Playbooks

### apt-update.yml

Updates the APT package cache and upgrades all installed packages on target hosts.

**Features:**

- Updates APT cache with 1-hour validity
- Upgrades all installed packages

**Usage:**

```bash
ansible-playbook -i inventory/hosts.ini apt-update.yml
```

**Example with specific hosts:**

```bash
ansible-playbook -i inventory/hosts.ini apt-update.yml --limit ubuntu-servers
```

## Requirements

- Target hosts must be Debian/Ubuntu based systems
- SSH access with sudo privileges
- APT package manager available
