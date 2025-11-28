# Ansible

Personal Ansible configuration repository for automation and configuration management.

## Repository Structure

```text
├── README.md                           # This file
├── requirements.txt                    # Python dependencies
├── requirements.yaml                   # Ansible collections
├── inventory/                          # Inventory files
│   └── example_inventory.ini           # INI format inventory
├── playbooks/                          # Ansible playbooks
├── roles/                              # Custom Ansible roles
└── scripts/                            # Helper scripts
```

## Usage

This repository contains Ansible configurations for managing various hosts and services.

## Quick Start

1. Copy and customize the inventory file:

   ```bash
   cp inventory/example_inventory.ini inventory/hosts.ini
   # Edit inventory/hosts.ini with your actual host details
   ```

2. Test connectivity:

   ```bash
   ansible all -i inventory/hosts.ini -m ping
   ```

3. Run a playbook:

   ```bash
   ansible-playbook -i inventory/hosts.ini playbooks/your-playbook.yml
   ```

## Setup

### 1. Create Virtual Environment

```bash
# Create virtual environment
python3 -m venv .venv

# Activate virtual environment
source .venv/bin/activate  # Linux/macOS
# .venv\Scripts\activate   # Windows
```

### 2. Install Dependencies

```bash
# Install required packages
pip install -r requirements.txt

# Install Ansible collections
ansible-galaxy collection install -r requirements.yaml

# Install pre-commit hooks (optional)
pre-commit install
```

### 3. Verify Installation

```bash
# Check Ansible version
ansible --version

# Test configuration
ansible-lint --version
```

## Requirements

- Python 3.13 or higher
- SSH access to target hosts with sudo privileges
