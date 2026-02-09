# Ansible-Baseline

This project provides a reusable Ansible baseline for configuring and hardening Linux servers. It is intended to be applied to fresh or existing systems to
establish a consistent, secure starting state before deploying applications or services.

The playbook follows a role-based structure and is designed to be simple,
idempotent, and easy to extend.

---

## Scope

The baseline performs the following tasks on managed hosts:

- Installs common system packages
- Configures time synchronization using Chrony
- Enables and configures Firewalld
- Applies basic SSH hardening:
  - Disables root login
  - Disables password-based authentication
  - Enforces key-based access
- Creates a non-root administrative user
- Deploys SSH public keys for administrative access

---

## Project Layout
```text
ansible-baseline/
├── group_vars/
│   └── all.yml
├── inventory/
│   └── hosts
├── roles/
│   ├── common/
│   │   └── tasks/
│   │       └── main.yml
│   ├── firewall/
│   │   └── tasks/
│   │       └── main.yml
│   ├── ssh/
│   │   ├── tasks/
│   │   │   └── main.yml
│   │   └── handlers/
│   │       └── main.yml
│   ├── users/
│   │   ├── tasks/
│   │   │   └── main.yml
│   │   └── files/
│   │       └── devopsuser.pub
│   └── time/
│       └── tasks/
│           └── main.yml
└── site.yml
```

Each role contains a `tasks/main.yml` file and is responsible for a single area
of system configuration. SSH-related service restarts are handled using Ansible
handlers.

---

## Usage

Run the baseline against the inventory:

```bash
ansible-playbook -i inventory/hosts site.yml
```

## Author
Jithin Joseph John


