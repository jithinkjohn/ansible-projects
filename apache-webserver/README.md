# Apache Web Server Automation (Ansible)

## Project Overview
This project automates the installation and configuration of the Apache HTTP Server on RHEL 9 managed nodes using Ansible.

## What This Playbook Does
- Installs Apache (`httpd`)
- Starts and enables the Apache service
- Opens HTTP service in the firewall
- Deploys a custom `index.html` page

## Environment
- Control Node: RHEL 9
- Managed Nodes: node1, node2
- Package Source: Local DVD ISO repository (offline setup)

## How to Run
```bash
ansible-playbook -i inventory apache.yml

