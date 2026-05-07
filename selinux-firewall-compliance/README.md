#  SELinux & Firewall Compliance Automation (Ansible)
##  Overview

This project is an Ansible-based security compliance automation system that enforces and maintains Linux server hardening standards across multiple nodes.

It focuses on:

SELinux enforcement and policy consistency
Firewalld service and port compliance
Drift detection and auto-remediation
Role-based access control (web vs db servers)
Continuous validation and audit reporting

# Objectives
Enforce secure baseline configurations across Linux systems
Prevent unauthorized firewall and service changes
Automatically detect and remediate configuration drift
Ensure SELinux policies remain enforced and consistent
Provide compliance visibility through structured reporting

## Architecture
```text
control-node (Ansible Controller)
        |
        |--- node1 (webserver)
        |      ├── HTTP / HTTPS allowed
        |      └── SSH enabled
        |
        |--- node2 (dbserver)
               └── SSH only (restricted access) 
```
## Project Structure
```text
selinux-firewall-compliance/
├── ansible.cfg
├── files
├── group_vars
│   ├── all.yml
│   ├── dbservers.yml
│   └── webservers.yml
├── host_vars
├── inventory
│   └── hosts.yml
├── README.md
├── roles
│   ├── firewall
│   │   ├── defaults
│   │   │   └── main.yml
│   │   ├── handlers
│   │   │   └── main.yml
│   │   ├── tasks
│   │   │   ├── main.yml
│   │   │   └── report.yml
│   │   └── vars
│   └── selinux
│       ├── defaults
│       │   └── main.yml
│       ├── handlers
│       ├── tasks
│       │   └── main.yml
│       └── vars
└── site.yml


```
## Features
### SELinux Automation
Ensures SELinux is in enforcing mode
Configures required SELinux booleans
Restores correct file contexts using restorecon
### Firewall Compliance
Installs and manages firewalld
Allows only approved services per host group
Enforces allowed ports (80, 443 for web servers)
Removes unauthorized services and ports
### Drift Detection
Detects non-compliant firewall services
Identifies unauthorized open ports
Compares actual vs desired state
### Auto-Remediation
Automatically removes unauthorized firewall rules
Ensures continuous compliance state
### Validation & Reporting
SSH connectivity validation
HTTP availability check for web servers
Per-host compliance summary report

## Key Concepts Used
-Ansible Roles (modular design)
-Inventory grouping (webservers / dbservers)
-register and set_fact
-Conditional execution (when)
-Loops and filters (difference)
-Handlers for optimized service reloads
-Idempotent playbook design
-Compliance-as-code approach

## Sample Output
```text
========== COMPLIANCE REPORT ==========
Host: node1
Allowed Services: ['ssh', 'http', 'https']
Allowed Ports: ['80/tcp', '443/tcp']
Unauthorized Services Removed: []
Unauthorized Ports Removed: []
=======================================
```
## Security Design Principles
Least privilege access enforcement
Role-based configuration management
Continuous compliance enforcement
Safe SSH-first firewall strategy (no lockout risk)
Idempotent infrastructure automation

## Safety Considerations
SSH is always preserved to avoid lockouts
Firewall changes are applied in controlled order
Changes are only applied when drift is detected

## How to run
```bash
ansible-playbook -i inventory/hosts.yml site.yml --ask-become-pass
```
## Author
Jithin Joseph John
