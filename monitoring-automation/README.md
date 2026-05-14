# Monitoring Setup Automation
---
## Project Overview
This project automates the deployment of a complete Linux monitoring and observability stack using Ansible.

The monitoring platform includes:

Prometheus for metrics collection
Node Exporter for Linux system metrics
Grafana for visualization dashboards
Prometheus alert rules for infrastructure monitoring

The project follows production-style automation practices using Ansible roles, templates, handlers, reusable variables, and inventory-driven configuration

---
## Technologies Used

Ansible
Prometheus
Node Exporter
Grafana
PromQL
Linux (RHEL/Rocky/AlmaLinux)
Firewalld
Systemd

---
## Infrastructure

Control Node (CN)	Ansible + Prometheus + Grafana
Managed Host 1	Node Exporter
Managed Host 2	Node Exporter

---
## Project Structure
```text
monitoring-automation/
├── group_vars
│   └── all.yml
├── inventory
│   └── hosts.yml
├── roles
│   ├── grafana
│   │   ├── defaults
│   │   │   └── main.yml
│   │   ├── handlers
│   │   │   └── main.yml
│   │   ├── tasks
│   │   │   └── main.yml
│   │   ├── templates
│   │   └── vars
│   │       └── main.yml
│   ├── node_exporter
│   │   ├── defaults
│   │   │   └── main.yml
│   │   ├── handlers
│   │   │   └── main.yml
│   │   ├── tasks
│   │   │   └── main.yml
│   │   ├── templates
│   │   │   └── node_exporter.service.j2
│   │   └── vars
│   │       └── main.yml
│   └── prometheus
│       ├── defaults
│       │   └── main.yml
│       ├── handlers
│       │   └── main.yml
│       ├── tasks
│       │   └── main.yml
│       ├── templates
│       │   ├── alerts.yml.j2
│       │   ├── prometheus.service.j2
│       │   └── prometheus.yml.j2
│       └── vars
│           └── main.yml
└── site.yml
└── README.md
```
---
## Features Implemented
### Node Exporter Automation

-Creates dedicated service user
-Deploys Node Exporter binary
-Configures systemd service
-Opens firewall port
-Enables and starts service

### Prometheus Automation

-Deploys Prometheus server
-Dynamically generates scrape targets
-Uses Jinja2 templates
-Configures Prometheus service
-Opens firewall port
-Enables and starts service

### Grafana Automation

-Installs Grafana
-Configures Grafana service
-Opens firewall access
-Connects Grafana with Prometheus datasource

Prometheus### Alerting System

Configured production-style alert rules:

-NodeDown
-HighCPUUsage
-HighMemoryUsage
-DiskAlmostFull

---
## Monitoring Architecture

```text
Managed Hosts
(node1 / node2)
        │
        ▼
Node Exporter (9100)
        │
        ▼
Prometheus Server (9090)
        │
        ▼
Grafana Dashboards (3000)
        │
        ▼
Alert Rules & Monitoring
```
---
## Ansible Concepts Practiced

-Role-based architecture
-Inventory grouping
-Templates (Jinja2)
-Handlers
-Variable separation
-Dynamic configuration generation
-Service management
-Idempotent playbooks
-Cross-host orchestration
-Firewall automation

---
## Deployment Steps

### Clone Repository

```bash
git clone <your-repo-url>
cd monitoring-automation
```
### Configure Inventory
edit
```bash
inventory/hosts.yml
```
### Run Playbook
```bash
ansible-playbook -i inventory/hosts.yml site.yml --ask-become-pass
````
---
## Access URLs

### Prometheus
```text
http://<CONTROL_NODE_IP>:9090
```
### Grafana
```text
http://<CONTROL_NODE_IP>:3000 
```
### Default credentials:
```text
Username: admin
Password: admin
```
---
## Screenshots

### Service Installation Proof- Prometheus

<img width="967" height="577" alt="prometheusinstall" src="https://github.com/user-attachments/assets/07082c5a-c521-42c4-a241-262d22bc920d" />

 ### Service Installation Proof- Grafana

<img width="967" height="617" alt="grafanaserver" src="https://github.com/user-attachments/assets/f679a556-e777-4f04-b33a-989979569d3f" />

### Prometheus UI

<img width="952" height="722" alt="prometheus" src="https://github.com/user-attachments/assets/a73cb0ce-c098-44f5-9b9e-cbc6ba173e97" />

### Grafana Dashboard

<img width="1892" height="837" alt="Grafana- Dashboard" src="https://github.com/user-attachments/assets/3162a27e-4597-42ac-80a9-1b0509be18f5" />

---
## Metrics Visualized

CPU Usage
Memory Usage
Disk Usage
System Load
---
## Author
Jithin Joseph John
