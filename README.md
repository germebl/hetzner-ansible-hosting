# Modular Ansible Managed Hosting System

## 📖 Project Overview

An automated hosting system fully managed through Ansible. The goal is to provision and manage various infrastructures (Web, DB, Mail, etc.) with minimal effort. The system supports multi-tier architectures and can scale for both managed hosting and single-user deployments.

This project is specifically developed for **Hetzner Cloud**, leveraging its flexibility and cost-efficiency for scalable infrastructure deployments.

## 🚀 Features

- **Primary Master** – Centralized control for provisioning and management.
- **Project Master** – Manages individual client or project infrastructures.
- **Nodes** – Worker nodes that handle specialized services like web, database, and mail.
- **SSL & DNS Automation** – LEGO acme client handles SSL certificate issuance with DNS validation through Hetzner DNS.
- **Logging** – JSON-based logging for all tasks and operations.

## 📋 Project Status

🔧 **In Development, Not Available – Architecture and Core components actively being built:**

### Phase 1: Conceptualization – Core Infrastructure Planning

**Goal:**  
Establish a clear foundation for the infrastructure and define the architecture. This phase focuses on planning and laying out the structure that will guide development moving forward.

---

### 1. Define Project Goals and Requirements
- **Target Audience:** Agencies, managed hosting providers, individual developers.  
- **Use Cases:**  
  - Multi-project environments (Project Masters managing nodes for different clients).  
  - Single-project environments (Primary Master directly managing nodes).  
  - Automated provisioning of web, database, and mail servers.  
- **Scalability:**  
  - Flexibility from provision simple LEMP stacks to integrated and distributed infrastructure by node-templates
  - Node templates contain the playbooks for one or more nodes including provisioning and basic configuration
  - Detailed configuration (e.g. creating sites or db's) get's managed by the Project Master
- **Security:**  
  - ~~The Primary Master deploys a DNS API Token to each Project Master, which can provision API Tokens for its nodes for pre-selected zones.~~
  - ~~The Project Master deploys the individual API Tokens to the Nodes where the Project Master can decide if it has access to all or just to specific zones of its project~~
  - Tokens currently lack project/zone restrictions (feature request in progress).  
- **Logging:**  
  - Initial systemd logging through Ansible configurations on the Primary Master.  
  - Long-term plan to expand logging to JSON-based solutions.
- **State Management:**  
  - A lightweight MySQL database on the Primary Master will track project states, client data, node information, and configurations.  
  - Each node's role and state will be defined through Hetzner labels and `host_vars`.  
  - e.g. Sites and configurations will be stored in the `host_vars` of each node to allow Ansible to manage existing configurations.  

---

### 2. Architecture Sketch
- **Components:**  
  - **Primary Master** – Oversees project management and provisioning.  
  - **Project Masters** – Manage individual projects (or clients).  
  - **Nodes** – Provisioned to handle specific workloads (web, DB, mail, etc.).  
- **Communication:**  
  - Primary Master connects to Project Masters via SSH (with sudo privileges and SSH keys).  
  - Each project will have its own firewall restricting SSH to Project Masters Public IP.  
- **Node Management:**  
  - Nodes do not self-register. Project Masters configure and manage nodes through Ansible playbooks.  
  - Node states are tracked in `host_vars`, allowing cron jobs or Ansible runs to sync node states.  
  - Changes to sites, configurations, and templates are reflected in `host_vars`.

---

### 3. Technical Decisions
- **Technologies:**  
  - Ansible for orchestration.  
  - LEGO for SSL certificate management.  
- **Hetzner Services:**  
  - cloud server, load balancers, ssh keys, firewalls, networks, and volumes.  
- **Dependencies:**  
  - Ansible, Python.

---
# TODO

### 4. High-Level Design Document (HLD)
- **Diagrams**:  
  - Sketch Primary and Project Master relationships.  
  - Visualize node provisioning workflows.  
  - Map out dependencies and component communication paths.  
- **Workflow:**  
  - How new nodes are provisioned and configured.  
  - SSL and DNS workflows using LEGO and Hetzner APIs.  
- **State Tracking:**  
  - Project and node states managed through MySQL on the Primary Master.  
  - `host_vars` reflect configurations of nodes.  

---

### Next Step – Phase 2: Core Infrastructure Setup
- Set up the basic project structure with roles, playbooks, and inventory files.  
- Begin provisioning the Primary Master and lay the groundwork for Project Masters and nodes.