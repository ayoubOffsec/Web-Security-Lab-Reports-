# Networking 

## Introduction

The **Networking** lab series from TryHackMe provides a comprehensive and structured learning path covering the fundamentals of computer networking, from basic concepts to advanced protocols and practical tools. This series forms an essential foundation for anyone entering the field of cybersecurity, as networking is the backbone of all digital communication and security operations.

**This guide covers the following labs:**

1. Networking Concepts
2. Networking Essentials
3. Networking Core Protocols
4. Networking Secure Protocols
5. Wireshark: The Basics
6. Tcpdump: The Basics
7. Nmap: The Basics

---

## Lab 1: Networking Concepts

### Lab Content and Objectives

This lab introduces the fundamental concepts of computer networking, providing a solid foundation for understanding how devices communicate across networks. It covers the OSI model, TCP/IP model, network devices, and basic addressing.

---

### Concepts Covered in This Lab

#### 1. Network Models

**OSI Model (Open Systems Interconnection):**

The OSI model is a conceptual framework that standardizes the functions of a networking system into seven distinct layers:

| Layer | Name | Function | Example Protocols |
|-------|------|----------|-------------------|
| 7 | Application | Provides network services to user applications | HTTP, FTP, SMTP, DNS |
| 6 | Presentation | Formats and encrypts data for application layer | SSL/TLS, JPEG, MPEG |
| 5 | Session | Establishes, manages, and terminates connections | NetBIOS, RPC |
| 4 | Transport | Provides reliable data transfer between hosts | TCP, UDP |
| 3 | Network | Handles routing and addressing of data packets | IP, ICMP, ARP |
| 2 | Data Link | Transfers data between adjacent network nodes | Ethernet, PPP, MAC |
| 1 | Physical | Transmits raw bit stream over physical medium | Ethernet cables, Fiber optics |

**TCP/IP Model:**

The TCP/IP model is the practical implementation used in modern networks and consists of four layers:

| Layer | Function | Example Protocols |
|-------|----------|-------------------|
| Application | Provides high-level protocols for user services | HTTP, FTP, DNS, SMTP |
| Transport | End-to-end communication and data flow control | TCP, UDP |
| Internet | Routing and addressing of packets | IP, ICMP, ARP |
| Network Access | Physical transmission of data | Ethernet, Wi-Fi, PPP |

**Key Differences Between Models:**

- OSI has 7 layers, TCP/IP has 4 layers
- OSI is a theoretical model, TCP/IP is the practical implementation
- OSI separates presentation and session layers, TCP/IP combines them into application layer

---

#### 2. Network Devices

| Device | Function | Layer |
|--------|----------|-------|
| **Router** | Connects different networks and routes traffic between them | Network Layer (Layer 3) |
| **Switch** | Connects devices within the same network and forwards data based on MAC addresses | Data Link Layer (Layer 2) |
| **Hub** | Broadcasts data to all connected devices (obsolete) | Physical Layer (Layer 1) |
| **Modem** | Converts digital signals to analog for transmission over phone lines | Physical Layer |
| **Firewall** | Filters network traffic based on security rules | Network/Application Layer |
| **Access Point** | Allows wireless devices to connect to a wired network | Data Link Layer |

---

#### 3. IP Addressing

**IPv4 Addresses:**

- 32-bit addresses expressed as four decimal numbers (octets) separated by dots
- Example: `192.168.1.1`
- Each octet ranges from 0 to 255
- Total possible addresses: 4.3 billion

**IPv4 Address Classes:**

| Class | Range | Network Bits | Host Bits | Purpose |
|-------|-------|--------------|-----------|---------|
| A | 0.0.0.0 – 127.255.255.255 | 8 | 24 | Large networks |
| B | 128.0.0.0 – 191.255.255.255 | 16 | 16 | Medium networks |
| C | 192.0.0.0 – 223.255.255.255 | 24 | 8 | Small networks |
| D | 224.0.0.0 – 239.255.255.255 | - | - | Multicast |
| E | 240.0.0.0 – 255.255.255.255 | - | - | Experimental |

**Private IP Addresses:**

| Class | Range |
|-------|-------|
| A | 10.0.0.0 – 10.255.255.255 |
| B | 172.16.0.0 – 172.31.255.255 |
| C | 192.168.0.0 – 192.168.255.255 |

**Subnet Masks:**

- Defines which portion of an IP address belongs to the network
- Example: `255.255.255.0` or `/24`
- `192.168.1.0/24` means first 24 bits are network portion, last 8 bits are host portion

---

#### 4. MAC Addresses

- **Media Access Control address**
- 48-bit hardware address assigned to network interfaces
- Expressed in hexadecimal format (e.g., `00:1A:2B:3C:4D:5E`)
- Unique to each network interface globally
- Used for communication within the same local network (Layer 2)

---

## Lab 2: Networking Essentials

### Lab Content and Objectives

This lab builds on basic networking concepts by exploring more essential topics including subnets, routing, and common services. It focuses on practical knowledge needed for real-world networking scenarios.

---

### Concepts Covered in This Lab

#### 1. IP Subnetting

**Why Subnetting?**

- Efficient use of IP addresses
- Improves network security and performance
- Reduces broadcast traffic
- Helps with network organization and management

**Calculating Subnets:**

| CIDR Notation | Subnet Mask | Usable Hosts |
|---------------|-------------|--------------|
| /24 | 255.255.255.0 | 254 |
| /25 | 255.255.255.128 | 126 |
| /26 | 255.255.255.192 | 62 |
| /27 | 255.255.255.224 | 30 |
| /28 | 255.255.255.240 | 14 |
| /29 | 255.255.255.248 | 6 |
| /30 | 255.255.255.252 | 2 |

**Subnetting Steps:**

1. Determine the network address by performing AND operation (IP address AND subnet mask)
2. Identify the broadcast address (all host bits set to 1)
3. Find the usable host range (first usable = network + 1, last usable = broadcast - 1)

---

#### 2. Network Address Translation (NAT)

**What is NAT?**

- Translates private IP addresses to public IP addresses
- Allows multiple devices on a private network to share a single public IP
- Provides additional security by hiding internal IP addresses

**Types of NAT:**

| Type | Description |
|------|-------------|
| **Static NAT** | One-to-one mapping between private and public IP |
| **Dynamic NAT** | Maps private IPs to a pool of public IPs |
| **PAT (Port Address Translation)** | Maps multiple private IPs to a single public IP using different ports |

---

#### 3. Network Services

**Common Network Services and Their Ports:**

| Service | Port | Protocol | Function |
|---------|------|----------|----------|
| HTTP | 80 | TCP | Web traffic |
| HTTPS | 443 | TCP | Secure web traffic |
| FTP | 20, 21 | TCP | File transfer |
| SSH | 22 | TCP | Secure remote access |
| Telnet | 23 | TCP | Unencrypted remote access |
| SMTP | 25 | TCP | Email sending |
| DNS | 53 | UDP/TCP | Domain name resolution |
| DHCP | 67, 68 | UDP | Automatic IP assignment |
| SNMP | 161 | UDP | Network device monitoring |
| MySQL | 3306 | TCP | Database management |
| RDP | 3389 | TCP | Remote desktop protocol |

---

#### 4. DHCP (Dynamic Host Configuration Protocol)

**What is DHCP?**

- Automatically assigns IP addresses to devices on a network
- Reduces manual configuration overhead
- Ensures unique IP address assignment

**DHCP Process (DORA):**

| Step | Name | Description |
|------|------|-------------|
| 1 | Discover | Client broadcasts DHCP Discover message |
| 2 | Offer | Server responds with DHCP Offer (offers IP) |
| 3 | Request | Client requests the offered IP with DHCP Request |
| 4 | Acknowledge | Server confirms with DHCP Acknowledgment |

---

#### 5. DNS (Domain Name System)

**What is DNS?**

- Translates human-readable domain names to IP addresses
- Acts as the "phonebook of the internet"
- Hierarchical and distributed database system

**DNS Record Types:**

| Record | Function |
|--------|----------|
| **A** | Maps domain to IPv4 address |
| **AAAA** | Maps domain to IPv6 address |
| **CNAME** | Canonical name (alias for another domain) |
| **MX** | Mail exchanger (email server address) |
| **NS** | Name server (authoritative DNS server) |
| **TXT** | Text records (SPF, DKIM, verification) |
| **PTR** | Reverse DNS lookup (IP to domain) |

**DNS Resolution Process:**

1. Browser checks local cache
2. Checks operating system cache
3. Contacts local DNS resolver (ISP)
4. Recursive query through DNS hierarchy (Root → TLD → Authoritative)
5. Returns IP address to client

---

## Lab 3: Networking Core Protocols

### Lab Content and Objectives

This lab dives deep into the essential protocols that power internet communication, including TCP, UDP, IP, ICMP, and ARP. It examines how these protocols work and how they can be observed in network traffic.

---

### Concepts Covered in This Lab

#### 1. TCP (Transmission Control Protocol)

**Characteristics:**

- **Connection-oriented** – establishes a connection before data transfer
- **Reliable** – guarantees delivery of data
- **Ordered** – ensures data arrives in the correct order
- **Flow control** – manages data rate to prevent congestion
- **Error checking** – detects and retransmits corrupted packets

**TCP Three-Way Handshake:**

| Step | Name | Description |
|------|------|-------------|
| 1 | SYN | Client sends SYN packet (Synchronize) |
| 2 | SYN-ACK | Server responds with SYN-ACK |
| 3 | ACK | Client responds with ACK (Acknowledgment) |

**TCP Flags:**

| Flag | Name | Function |
|------|------|----------|
| SYN | Synchronize | Initiates connection |
| ACK | Acknowledgment | Confirms receipt of data |
| FIN | Finish | Terminates connection |
| RST | Reset | Forces connection reset |
| PSH | Push | Forces immediate data delivery |
| URG | Urgent | Marks data as urgent |

---

#### 2. UDP (User Datagram Protocol)

**Characteristics:**

- **Connectionless** – no connection establishment required
- **Unreliable** – no guarantee of delivery
- **Unordered** – packets may arrive out of order
- **No flow control** – no congestion management
- **Lighter** – less overhead than TCP

**Comparison of TCP vs UDP:**

| Feature | TCP | UDP |
|---------|-----|-----|
| Connection | Connection-oriented | Connectionless |
| Reliability | Reliable | Unreliable |
| Ordering | Ordered | Unordered |
| Flow Control | Yes | No |
| Error Correction | Yes | No (basic checksum only) |
| Overhead | High (20-60 bytes header) | Low (8 bytes header) |
| Speed | Slower | Faster |
| Use Cases | HTTP, HTTPS, FTP, SMTP, SSH | DNS, DHCP, VoIP, Streaming, Gaming |

---

#### 3. IP (Internet Protocol)

**What is IP?**

- Core protocol of the internet layer
- Responsible for addressing and routing packets
- Provides logical addressing (IP addresses)
- Handles packet fragmentation and reassembly

**IPv6:**

- 128-bit addresses (compared to 32-bit in IPv4)
- Hexadecimal notation (e.g., `2001:0db8:85a3:0000:0000:8a2e:0370:7334`)
- Much larger address space
- Built-in security features (IPsec)
- Simplified header structure

---

#### 4. ICMP (Internet Control Message Protocol)

**What is ICMP?**

- Used for error reporting and diagnostic functions
- Utilized by tools like `ping` and `traceroute`
- Operates at the network layer

**Common ICMP Types:**

| Type | Name | Function |
|------|------|----------|
| 0 | Echo Reply | Response to ping request |
| 3 | Destination Unreachable | Host/network unreachable |
| 8 | Echo Request | Ping request |
| 11 | Time Exceeded | TTL expired (traceroute) |
| 5 | Redirect | Better route available |

**Popular ICMP Tools:**

| Tool | Function |
|------|----------|
| `ping` | Tests reachability using ICMP Echo Request/Reply |
| `traceroute` | Traces route to a destination using Time Exceeded messages |
| `pathping` | Combines ping and traceroute with statistics |

---

#### 5. ARP (Address Resolution Protocol)

**What is ARP?**

- Maps IP addresses to MAC addresses
- Operates at the data link layer (Layer 2)
- Essential for communication within a local network

**ARP Process:**

1. Device wants to send data to an IP address
2. Checks ARP cache for MAC address
3. If not found, broadcasts ARP Request (Who has this IP?)
4. Target device responds with ARP Reply (Here is my MAC)
5. MAC address is stored in ARP cache

---

## Lab 4: Networking Secure Protocols

### Lab Content and Objectives

This lab focuses on secure network protocols that provide encryption, authentication, and data integrity. It covers SSL/TLS, SSH, IPsec, and other security protocols essential for protecting data in transit.

---

### Concepts Covered in This Lab

#### 1. SSL/TLS (Secure Sockets Layer / Transport Layer Security)


3. IPsec (Internet Protocol Security)

What is IPsec?

    Framework of protocols for securing IP communications

    Provides encryption, authentication, and data integrity

    Operates at the network layer (Layer 3)

IPsec Modes:
Mode	Description
Transport Mode	Only encrypts the payload (not IP header)
Tunnel Mode	Encrypts entire packet and adds new IP header (used in VPNs)

IPsec Components:
Protocol	Function
AH (Authentication Header)	Provides integrity and authentication (no encryption)
ESP (Encapsulating Security Payload)	Provides encryption, authentication, and integrity
IKE (Internet Key Exchange)	Establishes security associations and exchanges keys
4. VPN (Virtual Private Network)

What is VPN?

    Extends a private network across a public network

    Provides secure, encrypted tunnel for data

    Used for remote access and site-to-site connectivity

VPN Protocols:
Protocol	Description
OpenVPN	Open-source, uses SSL/TLS, highly configurable
IPsec/L2TP	Common, uses IPsec for encryption, L2TP for tunneling
WireGuard	Modern, lightweight, fast, uses ChaCha20
PPTP	Outdated and insecure (not recommended)
SSTP	Microsoft protocol, uses SSL/TLS
5. DNSSEC (DNS Security Extensions)

What is DNSSEC?

    Security extension for DNS

    Adds digital signatures to DNS records

    Protects against DNS spoofing and cache poisoning

Key Functions:

    Data Authentication – Verifies DNS data comes from its source

    Data Integrity – Ensures DNS data hasn't been modified

    Authenticated Denial of Existence – Proves that a domain doesn't exist

Lab 5: Wireshark: The Basics
Lab Content and Objectives

This lab introduces Wireshark, the world's most popular network protocol analyzer. It covers how to capture, filter, and analyze network traffic to identify issues and understand protocol behavior.
Concepts Covered in This Lab
1. Introduction to Wireshark

What is Wireshark?

    Network protocol analyzer and packet sniffer

    Captures and displays network traffic in real-time

    Available for Windows, Linux, and macOS

    Free and open-source (GPL license)

Use Cases:
Purpose	Description
Network Troubleshooting	Identify connectivity and performance issues
Security Analysis	Detect attacks, malware, and suspicious traffic
Protocol Analysis	Understand how protocols work in practice
Network Monitoring	Track network usage and performance
Forensics	Investigate incidents and collect evidence
2. Basic Capture Techniques

Starting a Capture:

    Select the network interface to capture from (e.g., eth0, Wi-Fi)

    Click the "Start Capturing" button (shark fin icon)

    Stop capture when enough data has been collected

Capture Filters:

    Applied BEFORE capture to reduce captured data

    Captures only matching traffic

    Uses Berkeley Packet Filter (BPF) syntax

Common Capture Filters:
Filter	Description
host 192.168.1.100	Capture traffic to/from a specific host
port 80	Capture HTTP traffic
port 443	Capture HTTPS traffic
tcp	Capture TCP traffic only
udp	Capture UDP traffic only
not arp	Exclude ARP traffic
3. Display Filters

What are Display Filters?

    Applied AFTER capture to filter displayed packets

    Do not delete captured data, just hide non-matching packets

    More powerful and specific than capture filters

Common Display Filters:
Filter	Description
ip.addr == 192.168.1.100	Traffic to/from this IP
ip.src == 192.168.1.100	Traffic from this source IP
ip.dst == 192.168.1.100	Traffic to this destination IP
tcp.port == 80	TCP traffic on port 80
http	All HTTP traffic
https	All HTTPS traffic
dns	All DNS traffic
arp	All ARP traffic
tcp.flags.syn == 1	SYN packets only
tcp.flags.reset == 1	RST packets only
icmp	All ICMP traffic
4. Analyzing Packets

The Wireshark Interface:
Section	Description
Packet List Pane	Displays all captured packets (summary)
Packet Details Pane	Shows detailed information about selected packet (layers)
Packet Bytes Pane	Shows raw packet data in hexadecimal and ASCII

TCP Stream Analysis:
Feature	Description
Follow TCP Stream	Reconstructs entire TCP conversation
Show in HEX/ASCII	View data in both formats
Filter Stream	Filter packets from this stream
Extract Data	Save stream data to file

HTTP Analysis:
Feature	Description
HTTP Request/Response	View full HTTP conversation
Headers	Examine request and response headers
Status Codes	Check response status (200, 404, 500, etc.)
Body Data	View the actual web content

DNS Analysis:
Feature	Description
Query Type	A, AAAA, CNAME, MX, etc.
Query Name	Domain being resolved
Response	IP addresses returned
TTL	Time-to-live for cached records
5. Color Coding

Default Colors in Wireshark:
Color	Protocol
Green	TCP
Blue	UDP
Yellow/Red	HTTP
Purple	ICMP
Orange	ARP
Lab 6: Tcpdump: The Basics
Lab Content and Objectives

This lab introduces Tcpdump, a powerful command-line packet analyzer. It covers capturing and analyzing network traffic from the terminal, which is essential for system administrators and security professionals working with headless servers.
Concepts Covered in This Lab
1. Introduction to Tcpdump

What is Tcpdump?

    Command-line packet capture tool

    Uses libpcap to capture network packets

    Available on most Linux/Unix systems

    Lightweight and efficient

Key Benefits:
Benefit	Description
Low Resource Usage	Minimal CPU and memory overhead
Remote Operation	Can capture traffic on remote servers via SSH
Scriptable	Can be used in automated scripts
Powerful Filters	Uses the same BPF syntax as Wireshark
Portable	Available on almost all Unix-like systems
2. Basic Tcpdump Usage

Basic Command Structure:
bash

tcpdump [options] [filter]

Common Options:
Option	Description	Example
-i	Specify network interface	tcpdump -i eth0
-c	Limit number of packets to capture	tcpdump -c 100
-n	Don't resolve hostnames (faster)	tcpdump -n
-nn	Don't resolve hostnames or port names	tcpdump -nn
-v	Verbose output	tcpdump -v
-w	Write to file (PCAP format)	tcpdump -w file.pcap
-r	Read from file	tcpdump -r file.pcap

Basic Examples:
bash

# Capture all traffic on eth0
tcpdump -i eth0

# Capture only 10 packets
tcpdump -c 10

# Capture and write to file
tcpdump -w capture.pcap

# Read from file
tcpdump -r capture.pcap

# Capture with no name resolution
tcpdump -nn -i eth0

3. Tcpdump Filters

Host Filters:
Filter	Description
host 192.168.1.100	Traffic to/from host
src 192.168.1.100	Traffic from host
dst 192.168.1.100	Traffic to host

Port Filters:
Filter	Description
port 80	Traffic on port 80
src port 80	Traffic from port 80
dst port 80	Traffic to port 80
portrange 1-1024	Traffic on ports in range

Protocol Filters:
Filter	Description
tcp	TCP traffic only
udp	UDP traffic only
icmp	ICMP traffic only
arp	ARP traffic only

Combining Filters:
Operator	Example	Description
and or &&	host 192.168.1.100 and port 80	Both conditions
or or ||	port 80 or port 443	Either condition
not or !	not arp	Exclude ARP

Advanced Examples:
bash

# Capture HTTP traffic to/from a specific IP
tcpdump -i eth0 -nn 'host 192.168.1.100 and port 80'

# Capture traffic except SSH
tcpdump -i eth0 'not port 22'

# Capture SYN packets
tcpdump -i eth0 'tcp[tcpflags] & tcp-syn != 0'

# Detect ARP spoofing
tcpdump -i eth0 arp

# Detect port scanning
tcpdump -i eth0 'tcp[tcpflags] & tcp-syn != 0 and tcp[tcpflags] & tcp-ack == 0'

4. Reading PCAP Files
bash

# Basic reading
tcpdump -r capture.pcap

# With filters
tcpdump -r capture.pcap 'host 192.168.1.100'

# With time stamp
tcpdump -r capture.pcap -tttt

# With packet details
tcpdump -r capture.pcap -v

5. Tcpdump Tips and Best Practices
Tip	Description
Use -n flag	Disables DNS lookups, speeds up capture
Use -s 0	Captures full packets (not truncated)
Limit with -c	Use to limit capture count for testing
Use -w	Write to file first, analyze later
Buffer size	Use -B for larger buffer (reduces drops)
Filter early	Apply filters to reduce captured data
Check privileges	Tcpdump often requires root/sudo
Lab 7: Nmap: The Basics
Lab Content and Objectives

This lab introduces Nmap (Network Mapper), the most widely used port scanner. It covers scanning techniques, host discovery, port scanning, and service detection.
Concepts Covered in This Lab
1. Introduction to Nmap

What is Nmap?

    Network mapping and port scanning tool

    Finds live hosts, open ports, and running services

    Available on Windows, Linux, and macOS

    Included in most penetration testing distributions

Key Use Cases:
Purpose	Description
Host Discovery	Find live hosts on a network
Port Scanning	Identify open ports and services
Service Detection	Determine version of running services
OS Detection	Identify operating system of target
Security Auditing	Assess network security posture
Network Inventory	Map network assets
2. Basic Nmap Commands

Basic Command Structure:
bash

nmap [scan type] [options] [target]

Common Targets:
Target Format	Example	Description
Single IP	192.168.1.1	Scan a single host
Multiple IPs	192.168.1.1 192.168.1.2	Scan multiple hosts
IP Range	192.168.1.1-10	Scan a range of IPs
Subnet (CIDR)	192.168.1.0/24	Scan a whole subnet
Domain	scanme.nmap.org	Resolve domain to IP

Common Scanning Options:
Option	Description	Example
-sn	Ping scan (host discovery only)	nmap -sn 192.168.1.0/24
-sS	TCP SYN scan (stealth)	nmap -sS 192.168.1.1
-sT	TCP connect scan	nmap -sT 192.168.1.1
-sU	UDP scan	nmap -sU 192.168.1.1
-sV	Version detection	nmap -sV 192.168.1.1
-O	OS detection	nmap -O 192.168.1.1
-p	Specific ports	nmap -p 22,80,443 192.168.1.1
-p-	All ports (1-65535)	nmap -p- 192.168.1.1

Basic Examples:
bash

# Simple port scan
nmap 192.168.1.1

# Host discovery on a subnet
nmap -sn 192.168.1.0/24

# Service version detection
nmap -sV 192.168.1.1

# Scan specific ports
nmap -p 22,80,443 192.168.1.1

# OS detection
nmap -O 192.168.1.1

# Aggressive scan (includes OS, version, traceroute, scripts)
nmap -A 192.168.1.1

Conclusion

The Networking lab series from TryHackMe provides comprehensive and structured knowledge of computer networking, covering:
Level	Skills Covered
Basics	OSI model, TCP/IP model, IP addressing, network devices
Essentials	Subnetting, NAT, DHCP, DNS, network services
Core Protocols	TCP, UDP, IP, ICMP, ARP
Secure Protocols	SSL/TLS, SSH, IPsec, VPN, DNSSEC
Tools	Wireshark, Tcpdump, Nmap

This knowledge forms the foundation upon which any cybersecurity specialization is built, whether offensive or defensive, as networking is the backbone of all digital communication and security operations.

**What is SSL/TLS?**

- Cryptographic protocols that provide secure communication over a network
- Used to establish encrypted connections between clients and servers
- Most commonly used for HTTPS

**Key Functions:**

- **Encryption** – Protects data from eavesdropping
- **Authentication** – Verifies identity of communicating parties (using certificates)
- **Integrity** – Ensures data hasn't been tampered with

**SSL/TLS Handshake Process:**

| Step | Name | Description |
|------|------|-------------|
| 1 | Client Hello | Client sends supported cipher suites and TLS version |
| 2 | Server Hello | Server selects cipher suite and sends certificate |
| 3 | Certificate Verification | Client verifies server's certificate |
| 4 | Key Exchange | Client generates pre-master secret, encrypted with server's public key |
| 5 | Session Keys | Both parties generate session keys |
| 6 | Change Cipher Spec | Both parties switch to encrypted communication |
| 7 | Finished | Handshake complete, secure channel established |

**SSL/TLS Versions:**

| Version | Status |
|---------|--------|
| SSL 2.0 | Obsolete/Insecure |
| SSL 3.0 | Obsolete/Insecure |
| TLS 1.0 | Deprecated (vulnerable to attacks) |
| TLS 1.1 | Deprecated |
| TLS 1.2 | Widely used and secure |
| TLS 1.3 | Most recent, improved performance and security |

---

#### 2. SSH (Secure Shell)

**What is SSH?**

- Cryptographic network protocol for secure remote access
- Replaces insecure protocols like Telnet and rlogin
- Provides authentication and encryption

**SSH Components:**

| Component | Function |
|-----------|----------|
| **SSH Client** | Initiates connection (e.g., `ssh`, PuTTY) |
| **SSH Server** | Accepts connections (daemon: `sshd`) |
| **SSH Key Pair** | Public/private keys for authentication |
| **SSH Agent** | Manages private keys for SSO |

**SSH Authentication Methods:**

| Method | Description |
|--------|-------------|
| **Password** | User enters password (less secure) |
| **Public Key** | Uses SSH key pair (more secure) |
| **Host-based** | Authentication based on trusted hosts |
| **Kerberos** | Uses Kerberos ticket system |

**SSH Command Examples:**

```bash
ssh user@hostname          # Connect to remote host
ssh -p 2222 user@hostname  # Connect on non-standard port
ssh -i key.pem user@host   # Use private key authentication
scp file.txt user@host:/path/  # Secure copy
sftp user@host             # Secure FTP session
