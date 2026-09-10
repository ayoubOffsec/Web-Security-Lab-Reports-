# Full Web Application Penetration Test

## Overview

A web application security assessment was conducted to identify security vulnerabilities affecting authentication, authorization, input validation, and general application security.

The assessment identified the following vulnerabilities:

* SQL Injection – Authentication Bypass
* Missing Brute-Force Protection
* Insecure Direct Object Reference (IDOR)
* Cross-Site Scripting (XSS)
* Missing Security Headers

---

# 1. SQL Injection – Authentication Bypass

**Severity:** Critical
**Category:** SQL Injection / Authentication

## Description

The application's login functionality was found to be vulnerable to SQL Injection.

A login request was first intercepted using a web proxy. The email parameter was then modified using the following payload:

```text
' or 1=1--
```

The payload was encoded where necessary to ensure it was correctly processed by the application.

After submitting the modified request, the application successfully authenticated the request without valid credentials, demonstrating an authentication bypass.

## Proof of Concept

The original authentication request was intercepted after submitting arbitrary test credentials.

> **📸 Screenshot 1 — Insert `1.png` here**

<img width="1217" height="507" alt="1" src="https://github.com/user-attachments/assets/65dd973e-3838-43bb-a48f-086981458ed0" />


The email parameter was then modified with the SQL Injection payload and the request was submitted.

<img width="1217" height="667" alt="2" src="https://github.com/user-attachments/assets/f504ec78-00c4-4efa-b78c-4037660227ca" />


The application successfully authenticated the request, confirming the vulnerability.

## Impact

An attacker could potentially:

* Bypass the application's authentication mechanism.
* Access unauthorized user accounts.
* Access sensitive information.
* Potentially compromise privileged accounts.

## Recommendation

* Use prepared statements and parameterized queries.
* Never concatenate user-controlled input directly into SQL queries.
* Implement proper server-side input validation.
* Apply least-privilege database permissions.
* Use WAF protections only as an additional security layer.

---

# 2. Missing Brute-Force Protection

**Severity:** High
**Category:** Authentication

## Description

The application's login functionality did not appear to implement sufficient protection against repeated password attempts.

A login request was intercepted using a web proxy and then sent to an automated request-testing tool.

The password parameter was configured for controlled testing against a commonly used password wordlist.

No effective rate limiting or account lockout mechanism was observed during the assessment.

## Proof of Concept

A login request containing a known user email and an arbitrary password was intercepted.

The captured request was then transferred to the Intruder functionality for controlled testing.

<img width="1217" height="667" alt="4" src="https://github.com/user-attachments/assets/7f00e951-d854-42e1-b3b8-d2bc63745519" />


A password wordlist was configured and the password parameter was selected as the testing position.

<img width="1600" height="768" alt="5" src="https://github.com/user-attachments/assets/f27dd280-8abf-4492-b21e-baa117caea18" />


After starting the controlled attack, a valid password was identified based on the application's response.

<img width="1600" height="768" alt="6" src="https://github.com/user-attachments/assets/a5aa5945-97b7-486e-97b5-1a04c4eab28f" />


## Impact

An attacker could automate authentication attempts against user accounts.

This could lead to:

* Account compromise.
* Password spraying.
* Credential stuffing.
* Abuse of weak or reused passwords.

## Recommendation

* Implement server-side rate limiting.
* Add progressive delays after failed login attempts.
* Implement account protection mechanisms.
* Consider CAPTCHA after repeated failures.
* Enable Multi-Factor Authentication (MFA).
* Monitor repeated authentication failures.
* Detect and block automated authentication attempts.

---

# 3. Insecure Direct Object Reference (IDOR)

**Severity:** High
**Category:** Broken Access Control

## Description

The application was found to expose user-specific shopping cart resources through a user-controlled identifier.

The authenticated user's own cart was accessed first. The identifier was then modified to another value.

The application returned another user's shopping cart without properly verifying whether the authenticated user was authorized to access the requested resource.

## Proof of Concept

The authenticated user's shopping cart was accessed using the assigned identifier.

<img width="1214" height="768" alt="7" src="https://github.com/user-attachments/assets/9247ee4d-5b72-4f4e-874b-806c8c4f7d8d" />


The identifier was then modified to another user's identifier.

For example:

```text
User ID: 2
```

was changed to:

```text
User ID: 5
```

The application subsequently returned another user's shopping cart.

<img width="1214" height="768" alt="8" src="https://github.com/user-attachments/assets/36bca825-937f-42b2-bffe-d2e31b71cead" />


## Impact

An attacker could potentially:

* Access another user's resources.
* View unauthorized information.
* Enumerate valid identifiers.
* Potentially modify or delete another user's resources if similar authorization flaws exist in other endpoints.

## Recommendation

* Implement server-side authorization checks for every object.
* Verify resource ownership before returning data.
* Do not rely on predictable identifiers for access control.
* Test both read and write operations.
* Implement centralized access-control mechanisms.

---

# 4. Cross-Site Scripting (XSS)

**Severity:** High
**Category:** Cross-Site Scripting / Injection

## Description

The application's search functionality was found to process user-controlled input without sufficient output encoding.

A normal search value was first submitted and processed by the application.

A controlled XSS payload was then supplied:

```html
<iframe src="javascript:alert(`XSS`)">
```

The payload was executed by the browser, confirming that attacker-controlled input could reach an executable context.

## Proof of Concept

A normal value was entered into the search functionality.

<img width="1600" height="811" alt="9" src="https://github.com/user-attachments/assets/41c783cb-59ee-4802-8ff3-e20c6982c3c6" />


The XSS payload was then submitted:

```html
<iframe src="javascript:alert(`XSS`)">
```

The JavaScript payload executed successfully in the browser.

<img width="1600" height="811" alt="10" src="https://github.com/user-attachments/assets/15812127-7513-4e7e-8563-99632f0b6085" />


## Impact

Successful exploitation could allow an attacker to execute JavaScript in a victim's browser under the application's origin.

Potential consequences include:

* Manipulation of application content.
* Unauthorized actions within the victim's session.
* Access to data available to client-side scripts.
* Phishing attacks.
* Other client-side attacks depending on the application's security controls.

## Recommendation

* Implement context-aware output encoding.
* Validate and sanitize untrusted input where appropriate.
* Avoid unsafe HTML and JavaScript sinks.
* Implement a strong Content Security Policy (CSP).
* Use secure cookie attributes such as `HttpOnly`, `Secure`, and `SameSite`.

---

# 5. Missing Security Headers

**Severity:** Low
**Category:** Security Misconfiguration

## Description

A basic review of the application's HTTP response headers identified opportunities for additional security hardening.

The following security headers should be reviewed and configured where appropriate:

```http
Content-Security-Policy
X-Content-Type-Options: nosniff
Referrer-Policy: strict-origin-when-cross-origin
Strict-Transport-Security
Permissions-Policy
```

## Impact

Missing security headers can reduce browser-side security protections and may increase the impact of certain client-side vulnerabilities.

## Recommendation

Configure appropriate security headers according to the application's functionality and deployment environment.

Security headers should be considered defense-in-depth controls and should complement proper authentication, authorization, input validation, and secure coding practices.

---

# Risk Summary

| # | Vulnerability                           | Severity |
| - | --------------------------------------- | -------- |
| 1 | SQL Injection – Authentication Bypass   | Critical |
| 2 | Missing Brute-Force Protection          | High     |
| 3 | Insecure Direct Object Reference (IDOR) | High     |
| 4 | Cross-Site Scripting (XSS)              | High     |
| 5 | Missing Security Headers                | Low      |

---

# Tools Used

* Burp Suite
* Web Browser
* HTTP Request Analysis
* Intruder
* Manual Web Application Testing

---

# Skills Demonstrated

* Web Application Penetration Testing
* Authentication Testing
* SQL Injection
* Authentication Bypass
* Brute-Force Testing
* Broken Access Control
* IDOR Testing
* Cross-Site Scripting
* HTTP Security Header Analysis
* Vulnerability Assessment
* Security Reporting

---

# Conclusion

The security assessment identified multiple vulnerabilities affecting authentication, authorization, input validation, and application hardening.

The most critical finding was the **SQL Injection authentication bypass**, while additional high-severity issues included **missing brute-force protection, IDOR, and XSS**.

The identified vulnerabilities should be remediated according to their severity, followed by a security retest to verify that the implemented fixes are effective.
