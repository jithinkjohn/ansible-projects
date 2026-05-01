# Ansible Backup Automation
---
## Overview

This project automates backups of Linux servers using Ansible.
---
##  Features
- Backup important directories (/etc, /var/log)
- Store backups on control node
- Automatic daily backups using cron
- Backup verification
- Retention policy (auto delete old backups)
---
##  Project Structure
```text
backup-system
├── group_vars
│   └── all.yml
├── inventory
│   ├── hosts
│   └── hosts.yml
├── README.md
├── roles
│   └── backup
│       ├── defaults
│       │   └── main.yml
│       ├── files
│       ├── tasks
│       │   ├── backup.yml
│       │   ├── cron.yml
│       │   ├── install.yml
│       │   └── main.yml
│       ├── templates
│       └── vars
└── site.yml


```
---
##  How it works
1. Creates archive on remote nodes
2. Fetches backup to control node
3. Verifies backup
4. Deletes old backups
5. Schedules cron job
---

## How to run 
```bash
ansible-playbook -i inventory site.yml --ask-become-pass
```
---

## Author 
Jithin Joseph John
