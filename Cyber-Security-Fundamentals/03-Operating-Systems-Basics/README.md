# 03 - Operating Systems & CLI Fundamentals (Linux & Windows)

Welcome to the third section of the **Cyber Security Fundamentals** path. This guide provides a hands-on exploration of operating system architecture, command-line operations across Linux and Windows, and core OS security concepts.

---

## 📄 Client Vulnerability Briefing (Technical Summary)

> **Note for Clients & Stakeholders:**  
> An **Operating System (OS)** acts as the central software environment managing computer hardware and application permissions. Weak security configurations—such as predictable user credentials, overly permissive file permissions, or unpatched OS components—expose an organization to unauthorized system access, internal data leaks, and ransomware deployment. Security auditing ensures that operating systems are properly hardened using the **Principle of Least Privilege** and continuous monitoring.

---

## 🖥️ 1. OS Architecture & Hardware Interactions

An Operating System sits directly between physical hardware components and application software:

- **Hardware Layer:** CPU, RAM (Memory), System Board, Storage Devices (SSD/HDD), Network Interfaces.
- **OS Kernel & Core Services:** Controls execution threads, allocates memory, manages file systems, and enforces security boundaries.
- **Application Layer:** Programs such as Web Browsers, Databases, and Command-Line Interfaces (CLI).

### The CIA Triad in OS Security
1. **Confidentiality:** Ensuring system configuration files, tokens, and user data are accessible only by authorized users.
2. **Integrity:** Protecting system binaries, logs, and configuration files from unauthorized alteration.
3. **Availability:** Ensuring the system remains operational and resilient against system-crashing malware (e.g., Ransomware).

---

## 🐧 2. Linux CLI Navigation & System Reconnaissance

The Linux terminal provides low-level control over the operating system. Terminal commands are essential for executing security assessment tools and auditing system configurations.

### Directory Navigation & File Operations
- **`pwd`** — Print Working Directory (displays current absolute path).
- **`ls`** — List files and directories in the current folder.
- **`ls -l`** — Detailed file listing (permissions, owner, group, file size, timestamp).
- **`ls -al`** — Detailed listing including **hidden files** (files starting with `.`).
- **`cd <directory>`** — Change current working directory (`cd ..` moves back one directory level).
- **`cat <filename>`** — Display the full text contents of a file.

### Advanced Searching & File Discovery
- **`find <start_directory> -name <filename>`**  
  Locates specific files across the system directory tree.  
  *Example:* `find ~ -name mission_brief.txt` (Searches the user home directory).

### System Information & Reconnaissance Commands
- **`whoami`** — Displays the current logged-in user account.
- **`uname -a`** — Displays full OS kernel parameters, system hostname, and CPU architecture (`x86_64`).
- **`df -h`** — Displays disk space usage in human-readable format (e.g., GB, MB).
- **`cat /etc/os-release`** — Displays specific Linux distribution details (e.g., Ubuntu release version).

---

## 🪟 3. Windows Command Prompt (CMD) Operations

Windows CLI (Command Prompt) is a critical environment for performing security audits, system diagnostics, and forensic investigations on workstation environments.

### Navigation & File Commands
- **`cd`** — Displays current working directory path (or changes directory when target path is provided).
- **`dir`** — Lists contents of the current directory.
- **`dir /a`** — Displays all files, including **hidden system directories**.
- **`dir /s <filename>`** — Searches for a file recursively across all subfolders.
- **`type <filename>`** — Reads and outputs the contents of a text file.

### System Inspection Commands
- **`whoami`** — Identifies active domain/local user credentials.
- **`hostname`** — Displays the netBIOS machine name.
- **`systeminfo`** — Outputs extensive OS data (OS Name, Version, Build Number, Architecture, System Model).
- **`ipconfig`** — Displays active network interfaces, IPv4 addresses, subnet masks, and Default Gateways.

---

## 🛡️ 4. Key Operating System Security Weaknesses

Modern threat actors target three primary OS vulnerabilities to compromise target networks:

+-------------------------------------------------------------------------+
|                    Common OS Attack Vectors                             |
+--------------------------+--------------------+-------------------------+
| Weak Authentication      | File Permissions   | Malicious Software      |
| (Dictionary Passwords)   | (Over-Permissive)  | (Trojans & Ransomware)  |
+--------------------------+--------------------+-------------------------+


1. **Authentication & Weak Credentials:**
   - Use of common dictionary words (e.g., `123456`, `password`, `qwerty`) makes systems vulnerable to automated brute-force attacks.
   - Proper authentication requires strong passwords or multi-factor authentication (MFA).

2. **Weak File Permissions (Violation of Least Privilege):**
   - Misconfigured permissions allow low-privilege users or guest accounts to read sensitive configuration files or execute administrative applications.

3. **Malicious Programs (Malware):**
   - **Trojans:** Disguised software giving remote access to malicious actors.
   - **Ransomware:** Encrypts local files and storage drives to destroy system availability until decrypted.

---

## 🚀 Lab Verification Summary

| Task Platform | Target Activity | Command Used | Outcome |
| :--- | :--- | :--- | :--- |
| **Linux CLI** | File Search & Inspection | `find ~ -name mission_brief.txt` | Located path & extracted flag using `cat` |
| **Linux CLI** | System Auditing | `uname -a` & `cat /etc/os-release` | Identified Linux kernel and Ubuntu release |
| **Windows CLI** | Deep Search & Inspection | `dir /s task_brief.txt` | Discovered path & read contents using `type` |
| **Windows CLI** | Network & Host Recon | `whoami`, `hostname`, `ipconfig` | Extracted user account, host, and IPv4 data |

---
*Created by AyoubOffSec — Web Security & Cyber Security Fundamentals Roadmap.*
