# User Management

Creates and manages Linux users.

## Usage

```bash
ansible-playbook -i inventory.ini user-add.yml -e @users-list.yml
```

## Configuration

Edit `users-list.yml` with user definitions:

```yaml
users:
  - username: alice
    groups: [sudo, docker]
    sudo: true
    ssh_key: "ssh-rsa AAAAB3NzaC1... alice@laptop"
  - username: bob
    groups: [docker]
    ssh_key: "ssh-rsa AAAAB3NzaC1... bob@workstation"
```

**Key options:** `username` (required), `groups`, `sudo`, `ssh_key`, `shell`, `state`
    sudo: true
    sudo_nopasswd: false
    password: "$6$rounds=656000$YourHashedPassword..."
    ssh_key: "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDExample... charlie@macbook"
```

**Run:**
```bash
ansible-playbook -i inventory.ini users-add.yml -e @users_dev_team.yml
```

#### Example 2: Service Accounts

**service_accounts.yml:**
```yaml
users:
  - username: deploy
    comment: "Application Deployment Account"
    groups:
      - www-data
    sudo: true
    sudo_nopasswd: true
    uid: 9001
    ssh_key: "{{ lookup('file', '~/.ssh/deploy_key.pub') }}"

  - username: monitoring
    comment: "Monitoring Service - Prometheus"
    shell: /bin/bash
    groups:
      - monitoring
    sudo: false
    uid: 9002

  - username: backup
    comment: "Backup Service Account"
    shell: /bin/bash
    groups:
      - backup
    sudo: true
    sudo_nopasswd: true
    uid: 9003
```

#### Example 3: Remove Users

**remove_old_users.yml:**
```yaml
users:
  - username: old_employee
    state: absent

  - username: contractor_temp
    state: absent

  - username: test_user
    state: absent
```

**Run:**
```bash
ansible-playbook -i inventory.ini users-add.yml -e @remove_old_users.yml
```

#### Example 4: Mixed Operations

**users_mixed.yml:**
```yaml
users:
  # Add new user
  - username: newdev
    comment: "New Developer"
    groups: [sudo, developers]
    sudo: true
    ssh_key: "ssh-rsa AAAAB3NzaC..."

  # Update existing user (add to group)
  - username: existing_user
    groups: [docker, newgroup]
    sudo: true

  # Remove old user
  - username: old_user
    state: absent
```

## Security Best Practices

### 1. Password Management

**Generate secure password hash:**
```bash
# Method 1: Using mkpasswd (Debian/Ubuntu)
mkpasswd --method=sha-512

# Method 2: Using Python
python3 -c 'import crypt; print(crypt.crypt("YourPassword", crypt.mksalt(crypt.METHOD_SHA512)))'

# Method 3: Using OpenSSL
openssl passwd -6 -salt $(openssl rand -base64 6)
```

**Use Ansible Vault for sensitive data:**
```bash
# Create encrypted file
ansible-vault create users_secure.yml

# Edit encrypted file
ansible-vault edit users_secure.yml

# Run playbook with vault
ansible-playbook -i inventory.ini users-add.yml \
  -e @users_secure.yml \
  --ask-vault-pass
```

**Example with vault:**
```yaml
users:
  - username: admin
    comment: "Admin User"
    sudo: true
    password: !vault |
      $ANSIBLE_VAULT;1.1;AES256
      66386439653761323634616234376533...
    ssh_key: !vault |
      $ANSIBLE_VAULT;1.1;AES256
      33636234336335383534643335643765...
```
