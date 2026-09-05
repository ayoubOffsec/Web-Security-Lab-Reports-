# Vulnerability Report: Unrestricted File Upload via Content-Type Restriction Bypass

## 1. Executive Summary
During the security assessment of the web application's user profile forms, a critical Unrestricted File Upload vulnerability was discovered on the avatar upload endpoint (POST /my-account/avatar). The application attempts to validate uploaded files based solely on the user-controlled Content-Type HTTP header without enforcing strict server-side file extension and magic byte inspection.

An attacker can bypass this restriction by spoofing the Content-Type header (changing application/x-php to image/png) to upload an arbitrary PHP execution payload (.php). This enables Remote Code Execution (RCE), allowing an unauthorized attacker to read sensitive files, execute system commands, and achieve Full Server Compromise.
## 2. Vulnerability Details

    Vulnerability Class: File Upload Vulnerability / Insecure Input Validation

    Vulnerability Type: Arbitrary File Upload leading to Remote Code Execution (RCE)

    Severity: Critical (10.0 CVSS - Full System Access)

    Target Endpoint: POST /my-account/avatar

    Target Storage Directory: /files/avatars/

## Root Cause Analysis:

    Weak Client-Controlled Validation: The server relies on the client-supplied Content-Type request header to verify if the uploaded file is a valid image.

    Execution Permissions: The /files/avatars/ directory has script execution permissions enabled, allowing the web server parser (PHP) to process .php files directly upon access.

    Business & Security Impact: An attacker can upload a web shell, execute arbitrary system commands, steal application secrets, manipulate database contents, or use the compromised host as a pivot for lateral network movement.

 ## 3. Proof of Concept & Step-by-Step Exploitation

 Step 1: Endpoint Reconnaissance & Upload Location Identification

The user profile interface was inspected to locate avatar upload functionality. Uploading a standard avatar image revealed that user images are publicly accessible inside the /files/avatars/ directory.
Step 2: Payload Crafting (Safe PoC)

A controlled, non-malicious PHP script was created to demonstrate arbitrary file read capabilities by reading the contents of /home/carlos/secret:

<?php echo file_get_contents('/home/carlos/secret'); ?>

<img width="1488" height="678" alt="1" src="https://github.com/user-attachments/assets/0e36014e-e4f4-487d-be10-b74db4b31c7e" />

Step 3: Bypassing Content-Type Validation

Attempting to upload the .php file directly resulted in a server rejection, indicating an active file type check.

<img width="1563" height="678" alt="2" src="https://github.com/user-attachments/assets/95a816af-e4ed-41ee-ac62-3e93dda0b5ec" />

Using an interception proxy (Burp Suite), the request was intercepted. The Content-Type header associated with the file payload was modified from application/x-php to image/png:

The server accepted the modified request and successfully stored exploit.php in the public directory.

<img width="1563" height="678" alt="3" src="https://github.com/user-attachments/assets/37974376-824e-44e5-8d81-827de321c6c2" />

Step 4: Remote Code Execution (RCE) Verification

Navigating to the newly uploaded file's URL (GET /files/avatars/exploit.php) triggered the server-side PHP engine to execute the script. The sensitive contents of /home/carlos/secret were returned directly in the HTTP response.

<img width="1563" height="678" alt="4" src="https://github.com/user-attachments/assets/ce68ff4e-c6d5-42e5-aa88-1ebc752a3109" />

Client Note: "Unrestricted file upload vulnerabilities occur when a web server allows users to upload files without properly validating their name, extension, content, or size. When the server trusts client-provided headers like 'Content-Type' and stores uploaded scripts in a web-accessible directory with execution rights, attackers can upload malicious scripts (Web Shells) to run commands and gain total control over the host server."

## 5. Remediation & Hardening Recommendations

Strict File Extension & Magic Bytes Validation:

    Enforce a strict server-side whitelist of allowed extensions (e.g., .png, .jpg, .jpeg only).

    Do NOT rely on the Content-Type header sent by the browser. Inspect the actual file header bytes (Magic Bytes) on the server to verify file type integrity.

Disable Script Execution in Upload Directories: Configure the web server (Apache/Nginx) to explicitly disable script execution (e.g., PHP parsing) within the /files/avatars/ directory.

Randomize Filenames & Store Externally: Rename all uploaded files to randomly generated hashes (e.g., a8f9c2d1.png) and store them outside the public web root or in isolated cloud storage (e.g., AWS S3).

Implement Spam & CSRF Protection: Integrate reCAPTCHA v3 and anti-CSRF tokens across all public forms and contact inputs to prevent automated submission spam and unauthorized actions.
