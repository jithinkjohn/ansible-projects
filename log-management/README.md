# Automated Log Management & Rotation

##  Project Overview

This project demonstrates a **production-style centralized logging system** using **Ansible**.

It automates:

- Log server setup
- Log forwarding from clients
- Log rotation
- Disk usage monitoring and alerts

The project is structured using **Ansible roles** and follows production best practices.

---
## Project Strucure

```text
log-management
├── group_vars
│   └── log_clients.yml
├── inventory
│   └── hosts.yml
├── roles
│   ├── log_management
│   │   ├── handlers
│   │   ├── tasks
│   │   │   └── main.yml
│   │   └── templates
│   │       ├── disk_alert.sh.j2
│   │       └── logrotate_remote.j2
│   ├── rsyslog_client
│   │   ├── handlers
│   │   │   └── main.yml
│   │   ├── tasks
│   │   │   └── main.yml
│   │   └── templates
│   │       └── client.conf.j2
│   └── rsyslog_server
│       ├── handlers
│       │   └── main.yml
│       ├── tasks
│       │   └── main.yml
│       └── templates
│           └── server.conf.j2
└── site.yml
```
---

## Architecture

```text
Control Node (Ansible)
|
| SSH
v
Managed Nodes:
├── node1 (Log Server)
└── node2 (Log Client)
```


- **Control Node** → Runs Ansible playbooks  
- **node1** → Collects logs centrally  
- **node2** → Sends logs to node1  

---

## Project Phases

---

###   Phase 1 – Environment Setup

- Configured Ansible control node
- Set up SSH connectivity
- Created `devopsuser`
- Verified inventory and host connectivity

---

### Phase 2 – Centralized Logging

- Installed **rsyslog** on all nodes
- Configured node1 as a **log server**
- Configured node2 to **forward logs**
- Opened firewall port `514/tcp`
- Verified logs from node2 appear on node1

node2 → sends logs → node1 stores them

This makes monitoring and troubleshooting much easier.

---

### Phase 3 – Log Rotation & Disk Monitoring

- Configured **logrotate** for `/var/log/remote/*.log`
- Logs rotate daily
- Keep 7 days of history
- Old logs are compressed
- Created disk usage alert script
- Configured cron job to check disk automatically

---

### What is Log Rotation?

Log files grow continuously.

If we do nothing:
- Logs grow forever
- Disk becomes full
- System may crash

Log rotation:
- Archives old logs
- Compresses them
- Deletes very old logs automatically
---

```text 
   node2 (Log Client)
   ┌───────────────┐
   │   Applications│
   │   & Services  │
   └───────┬───────┘
           │  Logs forwarded via rsyslog
           ▼
   node1 (Log Server / Central Collector)
   ┌─────────────────────────────┐
   │ /var/log/remote/            │
   │ ┌───────────────┐           │
   │ │ node2.log     │ Current log
   │ └───────────────┘           │
   │ ┌───────────────┐           │
   │ │ node2.log.1.gz│ Yesterday, compressed
   │ └───────────────┘           │
   │ ┌───────────────┐           │
   │ │ node2.log.2.gz│ 2 days ago
   │ └───────────────┘           │
   └─────────┬───────────────────┘
             │
             │ Cron Job (/usr/local/bin/disk_alert.sh)
             │ runs hourly → checks disk usage
             ▼
   Disk Usage > 80% ?  ──► Email Alert to root
```
---
## Features

- Centralized logging with **rsyslog**
- Automated **log rotation** (daily, keep 7 days, compressed)
- Disk usage monitoring and alerts via **cron + bash script**
- Role-based **Ansible structure** for reusability
- Production-ready automation with **idempotent playbooks**
- Uses **group_vars, templates, handlers, and inventory** separation
---
## How to run
```bash
ansible-playbook -i inventory/hosts.yml site.yml --ask-become-pass
```
---
## Author
Jithin Joseph John
---
