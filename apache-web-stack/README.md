# Project 3: Apache Web Stack Deployment (Production Style)

This project demonstrates a production-style **Apache web stack deployment** using **Ansible** with proper role separation, idempotency, and environment support (dev/prod).

---

## Table of Contents

- [Overview](#overview)
- [Project Structure](#project-structure)
- [Inventory & Environment Variables](#inventory--environment-variables)
- [Phases](#phases)
- [Running the Playbook](#running-the-playbook)
- [Author](#author)

---

## Overview

The goal of this project is to deploy a modular web stack using Ansible with:

- Apache web server
- Optional PHP (Phase 3)
- Firewall configuration
- SELinux configuration
- Jinja2 VirtualHost templates
- Idempotent, production-ready design

---
## Project Structure

```text
apache-web-stack/
├── group_vars/
│   ├── dev/
│   │   └── webservers.yml
│   └── prod/
│       └── webservers.yml
├── inventory/
│   ├── dev/
│   │   └── hosts
│   └── prod/
│       └── hosts
├── README.md
├── roles/
│   ├── firewall/
│   │   ├── handlers/
│   │   │   └── main.yml
│   │   └── tasks/
│   │       └── main.yml
│   ├── PHP/
│   │   └── tasks/
│   │       └── main.yml
│   ├── SElinux/  
│   │   └── tasks/
│   │       └── main.yml
│   └── web/
│       ├── files/
│       ├── handlers/
│       │   └── main.yml
│       ├── tasks/
│       │   └── main.yml
│       └── templates/
│           └── vhost.conf.j2
└── site.yml


```
- `roles/web` → Apache installation, document root, test page, VirtualHost templates  
- `roles/firewall` → Firewall setup and handlers  
- `roles/selinux` → SELinux configuration for custom DocumentRoot  
- `group_vars` → Environment-specific variables for dev/prod  
- `inventory` → Hosts for dev and prod 

---
## Inventory & Environment Variables

**Inventory examples:**

`inventory/dev/hosts`:

```ini
[webservers]
node1 ansible_host=<ip> ansible_user=user
```
group_vars/dev/webservers.yml:

http_port: 80
server_admin: devops
document_root: /var/www/devsite
server_name: node1

Change prod values in group_vars/prod/webservers.yml accordingly.
---
## Phases

Phase 1: Apache Installation

Install Apache (httpd)

Create document root directory

Deploy test index page

Start and enable Apache service

Phase 2: VirtualHost & Templates

Deploy Jinja2-based VirtualHost configuration

Remove default Apache welcome page

Use handlers to restart Apache when configuration changes

Phase 3: Firewall, SELinux, PHP

Firewall: Allow HTTP, HTTPS, and SSH ports; reload firewalld

SELinux: Apply correct file context for custom directories

PHP: Install PHP and extensions
---
## Running the Playbook
For dev environment:

```bash
ansible-playbook -i inventory/dev site.yml --ask-become-pass

```
For Prod environment:

```bash
ansible-playbook -i inventory/prod site.yml --ask-become-pass

```
---
## Author
Jithin Jospeh John
