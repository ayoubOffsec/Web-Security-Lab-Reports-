# 05-Network Fundamentals 

Welcome to the fifth module in the **Pre-Security** learning path. This module establishes a comprehensive foundation in computer networking, traversing physical connections, data encapsulation, theoretical models (OSI & TCP/IP), packet-level structures, routing protocols, and perimeter defense technologies (Firewalls, NAT/Port Forwarding, VPNs).

---

## 📄 Client Vulnerability Briefing (Technical Summary)

> **Security Assessment Context:**  
> Computer networks provide the core infrastructure for data transport. Weaknesses in network architecture, device configurations, or protocol implementations expose organizations to high-impact cyber threats:
> - **Identity Spoofing & Layer 2 Attacks:** Unauthenticated protocols like ARP allow adversaries to execute ARP Poisoning and MAC Spoofing, enabling Man-in-the-Middle (MitM) positioning and traffic interception within Local Area Networks (LANs).
> - **Protocol Weaknesses & Stateless Exposure:** Unencrypted dynamic protocols (e.g., standard HTTP, legacy UDP-based services) allow packet sniffing, injection, and sensitive data exposure over untrusted transit networks.
> - **Perimeter & Segmentation Bypasses:** Improper firewall state management (stateless vs. stateful misconfigurations) or flat network topologies (absence of VLAN isolation) allow lateral movement from compromised low-trust zones to sensitive internal segments.

---

## 🌐 Task 1: What is Networking?

A network consists of two or more interconnected technological devices configured to exchange data and share resources[cite: 1].

### Core Concepts & Identification Mechanics
- **Private vs. Public Networks:** Private networks (LANs) connect localized, trusted devices, whereas public networks (the Internet) interconnect disparate global networks using Internet Service Providers (ISPs)[cite: 1].
- **Device Identification:**
  - **IP Address (Logical Identifier):** Temporarily assigned address used for layer-3 routing across networks[cite: 1].
  - **MAC Address (Physical Identifier):** A permanent, 12-character hexadecimal burn-in address (e.g., `a4:c3:f0:85:ac:2d`) assigned to the Network Interface Card (NIC) at the factory[cite: 1]. The first 6 characters represent the Organizationally Unique Identifier (OUI/Manufacturer), and the last 6 form the unique device serial[cite: 1].
- **IPv4 vs. IPv6:**
  - **IPv4:** 32-bit addressing scheme ($2^{32} \approx 4.29\text{ billion}$ unique addresses), structured in 4 octets[cite: 1]. Facing global address depletion[cite: 1].
  - **IPv6:** 128-bit addressing scheme ($2^{128} \approx 340\text{ trillion trillion}$ unique addresses), written in hexadecimal notation to sustain internet expansion[cite: 1].

---

## 🏗️ Task 2: Intro to LAN (Topologies & Core Protocols)

Local Area Networks (LANs) connect devices in close physical proximity using structured physical layouts and control protocols[cite: 1].

### LAN Network Topologies

| Topology | Structural Design | Key Advantage | Key Vulnerability / Single Point of Failure |
| :--- | :--- | :--- | :--- |
| **Star** | Central device (Switch/Hub) connected to each host[cite: 1]. | Highly scalable and easily isolated upon single host fault[cite: 1]. | Central switch failure collapses the entire network[cite: 1]. |
| **Bus** | Devices branch off a single backbone cable[cite: 1]. | Cheap and easy to deploy[cite: 1]. | Backbone cable fault halts all traffic; heavy collision bottlenecks[cite: 1]. |
| **Ring** | Devices form a circular loop; data travels sequentially[cite: 1]. | Reduced packet collisions compared to bus topologies[cite: 1]. | Any cut in the loop breaks full network connectivity[cite: 1]. |

### Essential Network Services & Resolution Protocols
- **Subnetting:** The process of dividing a broad network into smaller, logical sub-networks (subnets) using a Subnet Mask (e.g., `255.255.255.0`) to optimize traffic and enforce access boundaries[cite: 1].
- **Address Resolution Protocol (ARP):** Maps dynamic Layer 3 IP addresses to physical Layer 2 MAC addresses[cite: 1]. Operates via **ARP Requests** (Layer 2 Broadcasts: *"Who has IP X?"*) and **ARP Replies** (Unicast: *"I have IP X, my MAC is Y"*), caching these pairs in the local ARP Cache[cite: 1].
- **Dynamic Host Configuration Protocol (DHCP):** Automatically assigns IP configurations to joining hosts via the **DORA** process: **D**iscover $\rightarrow$ **O**ffer $\rightarrow$ **R**equest $\rightarrow$ **A**cknowledgement[cite: 1].

---

## 📦 Task 3: The OSI Model

The **Open Systems Interconnection (OSI)** model defines a standardized 7-layer framework dictating how distinct vendors and technologies handle network communications[cite: 1].

+-----------------------------------------------------------------------+|                       OSI Model (7 Layers)                            |+-------+------------------+--------------------------------------------+| Layer | Name             | Core Function & Security Relevance         |+-------+------------------+--------------------------------------------+|   7   | Application      | Human-computer interaction (HTTP, DNS)     ||   6   | Presentation     | Syntax translation, SSL/TLS Encryption     ||   5   | Session          | Port-to-port dialogue state management     ||   4   | Transport        | End-to-end reliability (TCP vs UDP)        ||   3   | Network          | Logical IP routing & packet path selection ||   2   | Data Link        | Physical MAC framing & local switching     ||   1   | Physical         | Binary bitstreams over physical mediums    |+-------+------------------+--------------------------------------------+
### Transport Protocols: TCP vs. UDP
- **Transmission Control Protocol (TCP):** Connection-oriented, stateful protocol that guarantees ordered delivery and integrity via error checking and sequence tracking[cite: 1].
- **User Datagram Protocol (UDP):** Stateless, connectionless protocol prioritizing rapid transmission over reliability; ideal for real-time video streaming, VoIP, and DNS queries[cite: 1].

---

## ✉️ Task 4: Packets & Frames

Data traversing a network is divided into smaller chunks to prevent bandwidth clogging[cite: 1]. As data moves down through the OSI layers, it undergoes **Encapsulation** (adding header metadata), and upon arrival at its target, it undergoes **Decapsulation**[cite: 1].

### TCP Three-Way Handshake & Teardown
Before TCP transmits application payloads, it establishes a synchronous connection[cite: 1]:
1. **SYN:** Client sends an Initial Sequence Number (ISN) to initiate synchronization[cite: 1].
2. **SYN/ACK:** Server acknowledges the client's ISN and responds with its own ISN[cite: 1].
3. **ACK:** Client acknowledges the server's ISN; the session transitions to `ESTABLISHED`[cite: 1].
- **Connection Teardown:** Closed cleanly via **FIN** (Finish) exchange, or abruptly aborted via **RST** (Reset) flags[cite: 1].

### Common Well-Known Network Ports ($0 - 1024$)

| Protocol | Port Number | Service Function | Security Risk / Assessment Focus |
| :--- | :--- | :--- | :--- |
| **FTP** | `21` | Unencrypted File Transfer Protocol | Cleartext credentials & file exposure[cite: 1]. |
| **SSH** | `22` | Secure Shell Remote Management | Key management & brute-force surface[cite: 1]. |
| **HTTP** | `80` | HyperText Transfer Protocol | Unencrypted web traffic interception[cite: 1]. |
| **HTTPS**| `443` | HTTP over TLS/SSL | Encrypted web application traffic[cite: 1]. |
| **SMB** | `445` | Server Message Block | File/printer sharing; frequent target for exploits[cite: 1]. |
| **RDP** | `3389` | Remote Desktop Protocol | Graphical administrative session target[cite: 1]. |

---

## 🛣️ Task 5: Extending Your Network

Connecting private enterprise networks across public infrastructure safely requires routing mechanisms, access boundaries, and encrypted communications[cite: 1].

### Network Perimeter Architecture & Infrastructure
- **Routers vs. Switches:**
  - **Routers (Layer 3):** Inspect IP destinations to route packets between disparate networks[cite: 1].
  - **Layer 2 Switches:** Forward frames within a single network based on MAC destination tables[cite: 1].
  - **Layer 3 Switches:** Combine multi-port Ethernet switching with hardware-accelerated IP routing capabilities[cite: 1].
- **VLANs (Virtual LANs):** Logically segment physical network infrastructure into distinct security domains (e.g., isolating *HR* traffic from *Guest Wi-Fi*) regardless of physical port grouping[cite: 1].
- **Port Forwarding (NAT Mapping):** Maps public router IP address ports to internal private IP address ports, enabling external users to access localized internal resources (e.g., hosting a web server behind a router)[cite: 1].
- **Firewalls:** Network barrier solutions that enforce security policies by inspecting arriving and departing network packets[cite: 1]:
  - **Stateful Firewalls:** Track active session states across whole connection lifetimes; consume higher system resources but prevent unauthorized out-of-sequence responses[cite: 1].
  - **Stateless Firewalls:** Inspect individual packets in isolation against static ACL rules; fast and efficient against high-volume attacks (e.g., DDoS) but vulnerable to state evasion[cite: 1].
- **Virtual Private Networks (VPNs):** Establish encrypted, authenticated point-to-point tunnels (e.g., using IPsec, PPTP, or OpenVPN/WireGuard) over public networks, enabling remote workforce integration and protecting transit data from eavesdropping[cite: 1].

---
## 🚀 Module Verification & Lab Mapping

| Lab Room | Key Tested Concept | Security Assessment Application |
| :--- | :--- | :--- |
| **What is Networking?** | MAC Spoofing & IP Addressing | Bypassing MAC-filtered Wi-Fi access controls[cite: 1]. |
| **Intro to LAN** | Network Topologies, ARP & DHCP | Auditing Layer 2 MitM & ARP Poisoning threats[cite: 1]. |
| **OSI Model** | 7-Layer Protocol Stack & Transport Models | Identifying attack layers (L3 DDoS, L7 Web Attacks)[cite: 1]. |
| **Packets & Frames** | TCP Handshakes, Headers & Ports | Scanning open ports and analyzing traffic PCAP captures[cite: 1]. |
| **Extending Network** | Firewalls, VLANs, Port Forwarding & VPNs | Evaluating network segregation and perimeter breach risks[cite: 1]. |


