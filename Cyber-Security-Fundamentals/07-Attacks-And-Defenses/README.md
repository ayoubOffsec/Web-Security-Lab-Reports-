# Attacks and Defenses 

Welcome to the seventh and final module in the **Pre-Security** learning path. This module covers core security principles, cryptographic mechanisms, offensive security tactics (Red Teaming / Pentesting), and defensive security strategies (Blue Teaming / Incident Response).

---

## 📄 Executive Technical Summary

> **Security Mindset & Operational Alignment:**  
> Cyber security aims to protect digital assets through two complementary operations: **Offensive Security** (finding vulnerabilities before malicious actors do) and **Defensive Security** (maintaining visibility, preventing breaches, and mitigating incidents). 
> - **Triad Violations:** Attacks target data **Confidentiality** (unauthorized access), **Integrity** (unauthorized modification), or **Availability** (denial of service).
> - **Cryptographic Assurance:** Confidentiality and Integrity rely heavily on cryptographic implementations (Symmetric/Asymmetric encryption and TLS/SSL certificates).
> - **Chained Exploitation:** System compromise often results from combining minor issues (e.g., directory enumeration leading to credential brute-forcing).

---

## 🛡️ Task 1: The CIA Triad

The **CIA Triad** is the foundational framework driving all security controls, assessments, and incident response evaluations.

### Core Pillars

1. **Confidentiality:** Ensures sensitive data is accessible strictly to authorized individuals.
   - *Threats:* Interception, credential exposure, unauthorized data access.
   - *Controls:* Access Control Lists (ACLs), Encryption, Multi-Factor Authentication (MFA).
2. **Integrity:** Guarantees that data remains accurate, complete, and un-tampered with by unauthorized entities.
   - *Threats:* Unauthorized parameter modification, database tampering, price/order manipulation.
   - *Controls:* Hashing, digital signatures, strict database permissions.
3. **Availability:** Guarantees that systems, networks, and applications remain accessible to authorized users when needed.
   - *Threats:* Denial of Service (DoS/DDoS), power outages, resource exhaustion.
   - *Controls:* Load balancing, redundancy, traffic rate limiting.

---

## 🔐 Task 2: Cryptography Concepts

Cryptography provides the mathematical controls required to enforce Confidentiality and Integrity over insecure networks.

### Fundamental Terminology
- **Plaintext:** Unencrypted readable data.
- **Ciphertext:** Encrypted/scrambled data requiring a key to decrypt.
- **Key:** A secret parameter used by encryption/decryption algorithms.
- **Algorithm:** The public mathematical procedure used for scrambling/unscrambling data[cite: 3].

### Symmetric vs. Asymmetric Encryption

| Feature | Symmetric Encryption | Asymmetric Encryption |
| :--- | :--- | :--- |
| **Key Architecture** | Single shared key for Encryption & Decryption[cite: 3]. | Key Pair: Public Key (encrypt) & Private Key (decrypt)[cite: 3]. |
| **Speed & Efficiency** | Extremely fast; ideal for bulk data processing[cite: 3]. | Computationally slower; used for small payloads/handshakes[cite: 3]. |
| **Key Management** | Subject to the **Key Distribution Problem**[cite: 3]. | Solves key distribution (Public key shared freely)[cite: 3]. |
| **Real-World Examples**| AES (Advanced Encryption Standard)[cite: 3]. | RSA, ECC (Elliptic Curve Cryptography)[cite: 3]. |

### Hybrid Cryptography (HTTPS Implementation)
Modern web security (TLS/SSL) combines both schemes:
1. **Key Exchange (Asymmetric):** The browser uses the site's **Public Key** (validated via Certificate Authorities) to safely establish a shared secret[cite: 3].
2. **Data Transit (Symmetric):** The browser and server switch to fast **Symmetric Encryption** using the shared secret for active session traffic[cite: 3].

---

## ⚔️ Task 3: Offensive Security (Become a Hacker)

Offensive security involves authorized, proactive testing to identify and exploit vulnerabilities before attackers do[cite: 3].

### Key Terminology
- **Scope:** Strict boundaries defining allowed target systems and actions[cite: 3].
- **Vulnerability:** Flaw or weakness in software logic or system configuration[cite: 3].
- **Exploit:** Technique or code used to take advantage of a vulnerability[cite: 3].
- **Enumeration:** Gathering information about targets (directories, services, users)[cite: 3].

### Practical Exploitation Flow (Chaining Weaknesses)

In security assessments, minor vulnerabilities are often chained together to achieve high-impact compromise[cite: 3]:

1. **Content Discovery (Directory Enumeration):**
   Using automated discovery tools like **Gobuster** to uncover unlinked directories/files[cite: 3]:
   ```bash
   gobuster dir --url [http://www.onlineshop.thm/](http://www.onlineshop.thm/) -w /usr/share/wordlists/dirbuster/directory-list.txt

Result: Discovery of exposed administrative endpoints (e.g., /login)[cite: 3].

    Automated Credential Testing (Dictionary Attack):
    Leveraging Hydra to test login interfaces against wordlists[cite: 3]:
    Bash

    hydra -l admin -P passlist.txt www.onlineshop.thm http-post-form "/login:username=^USER^&password=^PASS^:F=incorrect" -V

    Result: Gaining authenticated administrative access to sensitive internal functions[cite: 3].

🛡️ Task 4: Defensive Security (Become a Defender)

Defensive security focuses on maintaining system visibility, preventing attacks, detecting anomalous behavior, and responding to incidents[cite: 3].
The Defensive Framework (P-D-M-A-R)

    Prevention: Implementing security controls (Firewalls, Antivirus, Patch Management) to block threats[cite: 3].

    Detection: Monitoring network logs and host activity to identify anomalies[cite: 3].

    Mitigation: Isolating compromised assets and blocking malicious sources[cite: 3].

    Analysis: Investigating root causes and impact through log forensics[cite: 3].

    Response & Improvement: Recovering operations and strengthening security policies[cite: 3].

Multi-Layered Perimeter Defense Architecture
System Component	Security Risk	Applied Defense Mechanisms
Employee Devices	Malware execution, Phishing links	

Endpoint Detection & Response (EDR), Antivirus, Patching[cite: 3].
Web Server	Web app exploitation, DDoS	

Web Application Firewall (WAF), TLS, Least Privilege[cite: 3].
Mail Server	Email spoofing, Malicious attachments	

Spam Filters, SPF/DKIM/DMARC records, Attachment Sandboxing[cite: 3].
Network Perimeter	Unauthorized inbound/outbound access	

Stateful Firewall rules, IP Reputational Filtering[cite: 3].
🚀 Module Verification & Lab Mapping
Lab Room	Core Tested Concept	Practical Assessment Focus
The CIA Triad	Security Core Pillars	

Categorizing real-world security incidents by impact type[cite: 3].
Cryptography Concepts	Encryption Schemes & Ciphers	

Caesar cipher analysis, key exchange mechanics & HTTPS certificates[cite: 3].
Become a Hacker	Offensive Methodologies	

Directory brute-forcing (Gobuster) and credential attacks (Hydra)[cite: 3].
Become a Defender	Infrastructure Defense	

Mapping client infrastructure assets and applying layered controls[cite: 3].
