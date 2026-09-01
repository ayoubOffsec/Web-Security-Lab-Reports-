# Module 02: Computer Fundamentals

## 💡 Module Summary
This module breaks down the core structural components of computer systems, how they execute the boot process, the architectural spectrum of computing devices (from PCs to IoT), client-server communication models, virtualization primitives, and modern cloud deployment paradigms.

---

## 🛠️ Key Concepts & Technical Principles

### 1. Computer System Hardware & Boot Architecture
- **Hardware Building Blocks:** CPU (computation), RAM (volatile execution memory), Storage (non-volatile storage), Motherboard (interconnect bus), and PSU (power delivery).
- **The 5-Stage Boot Process:**
  1. **Power-On:** Signal sent to PSU to initiate power delivery.
  2. **Firmware Execution (UEFI/BIOS):** System low-level code initializes core hardware.
  3. **POST (Power-On Self Test):** Validates hardware presence, integrity, and basic health.
  4. **Boot Device Selection:** Firmware checks prioritized drive order for bootable media.
  5. **Bootloader Initiation:** Bootloader transfers the OS Kernel into RAM and hands over operational control.

### 2. Computer Device Classification
- **Desktops/Laptops:** End-user systems focused on fixed performance or portability.
- **Workstations:** High-reliability machines with specialized hardware designed for error reduction and intense computation.
- **Servers:** Screen-less systems dedicated to providing continuous network services to clients.
- **Embedded Systems vs. IoT:** 
  - **Embedded:** Autonomous, dedicated single-purpose microcontrollers operating without network dependency (e.g., door sensors, appliance control).
  - **IoT (Internet of Things):** Network-connected smart devices capable of remote telemetry, telemetry reporting, and external command execution (e.g., smart thermostats, IP cameras).

### 3. Client-Server Architecture & Communication
- **The Model:** Clients initiate requests; Servers process and deliver responses over network sockets.
- **Ports & Protocols:** Ports route traffic to specific services (e.g., HTTP on Port 80, HTTPS on Port 443). Protocols define formatting rules and syntax.
- **Stateless HTTP Protocol:** HTTP handles requests independently. Application-level statefulness is maintained using session identifiers, cookies, or authorization tokens.
- **Core HTTP Methods:** `GET` (retrieve resources), `POST` (submit data), `PUT` (update resource), `DELETE` (remove resource).

### 4. Virtualization & Container Mechanics
- **Hypervisors (Bare-Metal vs Host-Based):**
  - **Type 1 (Bare-Metal):** Runs directly on physical hardware (e.g., VMware ESXi, Proxmox). High performance, enterprise standard.
  - **Type 2 (Hosted):** Runs on top of an existing host OS (e.g., VirtualBox, VMware Workstation). Ideal for testing and home labs.
- **Virtual Machines (VMs) vs. Containers (Docker):**
  - **VMs:** Virtualize complete hardware resources; each VM runs its own separate Guest OS (high isolation, high resource overhead).
  - **Containers:** Virtualize at the OS kernel level; applications share the host system's kernel (lightweight, rapid startup, identical environment compatibility).

### 5. Cloud Computing Fundamentals
- **Deployment Models:** Public Cloud (shared/scalable), Private Cloud (dedicated/compliant), Hybrid Cloud (mixed operational model).
- **Service Models:**
  - **IaaS (Infrastructure as a Service):** Rent virtual compute, storage, and networking (e.g., AWS EC2). OS and application management remain user responsibility.
  - **PaaS (Platform as a Service):** Cloud provider manages infrastructure and OS; user manages code and runtime deployment.
  - **SaaS (Software as a Service):** Fully managed end-user application delivered over the web (e.g., Gmail, Microsoft 365).
- **Cloud Optimization & Elasticity:** On-demand provisioning, pay-as-you-go billing, and infrastructure scaling based on dynamic demand.

---

## 📌 Summary Takeaway
Understanding physical hardware, boot sequences, virtualized workloads, client-server models, and cloud abstractions is the baseline required for analyzing system attack surfaces, network vectors, and cloud configuration weaknesses.
