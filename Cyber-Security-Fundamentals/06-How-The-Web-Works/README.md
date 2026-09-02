# 06 - How The Web Works 

Welcome to the sixth module in the **Pre-Security** learning path. This module explores the core mechanics of the World Wide Web, covering Domain Name System (DNS) resolution, HTTP protocol operations, header architecture, session management via cookies, web application structures (Front-End vs. Back-End), and perimeter web infrastructure components (Load Balancers, CDNs, Databases, WAFs).

---

## 📄 Executive Technical Summary

> **Web Infrastructure Security Context:**  
> Modern web application delivery relies on a multi-tiered architecture spanning name resolution, HTTP transit, application logic, and database persistence. Security weaknesses at any tier introduce critical attack vectors:
> - **DNS Impersonation & Cache Poisoning:** Insecure or unvalidated DNS resolution flows can allow adversaries to redirect legitimate traffic to malicious endpoints via cache poisoning or unauthorized record modifications (e.g., modifying A/CNAME records).
> - **Client-Side Vulnerabilities & Input Sanitization Bypasses:** Inadequate input filtering on the front-end allows malicious actors to execute **HTML Injection** or Cross-Site Scripting (XSS), altering application appearance, hijacking session tokens, or phishing end-users.
> - **Information Exposure & Misconfigurations:** Failure to sanitize source code or obscure backend parameters leads to **Sensitive Data Exposure** (e.g., lingering credentials, API endpoints in comments, or unhandled internal application errors).

---

## 🌐 Task 1: DNS in Detail

The **Domain Name System (DNS)** serves as the internet's directory service, translating human-readable domain names (e.g., `example.com`) into machine-routable IP addresses (e.g., `104.26.10.229` for IPv4 or `2606:4700:20::681a:be5` for IPv6).

### Domain Hierarchy Architecture
DNS follows an inverted tree hierarchy structured as follows:
1. **Root Domain (`.`):** The top of the DNS hierarchy, maintained by ICANN and managed via root servers.
2. **Top-Level Domain (TLD):** The rightmost portion of a domain name:
   - **gTLD (Generic TLD):** Denotes functional domains (e.g., `.com`, `.org`, `.edu`, `.gov`).
   - **ccTLD (Country Code TLD):** Denotes geographical locations (e.g., `.ca`, `.uk`, `.ma`).
3. **Second-Level Domain (SLD):** The unique identifier registered directly below the TLD (e.g., `tryhackme` in `tryhackme.com`). Limited to 63 characters.
4. **Subdomain:** Optional prefix structures separated by periods (e.g., `admin.tryhackme.com` or `jupiter.servers.tryhackme.com`). The entire domain name string cannot exceed 253 characters.

### Essential DNS Record Types

| Record Type | Function | Security & Operational Context |
| :--- | :--- | :--- |
| **A** | Maps a domain to an IPv4 address. | Primary IPv4 destination resolution. |
| **AAAA** | Maps a domain to an IPv6 address. | Primary IPv6 destination resolution. |
| **CNAME** | Aliases one domain to another domain name. | Often points to third-party services (e.g., Shopify, AWS CloudFront). |
| **MX** | Specifies mail servers for the domain with priority flags. | Target selection for email spoofing and phishing vector evaluations. |
| **TXT** | Stores arbitrary text data. | Used for **SPF**, **DKIM**, **DMARC** email authentication, and domain ownership validation. |

### The DNS Request Lookup Flow
When a host queries a domain name:
1. **Local Cache Check:** The host OS checks local DNS cache and hosts files.
2. **Recursive DNS Server:** If uncached, the query goes to the Recursive Resolver (ISP or custom DNS like `1.1.1.1` or `8.8.8.8`).
3. **Root Nameserver Referral:** The Recursive Server queries Root Servers to locate the correct TLD Nameserver.
4. **TLD Nameserver Referral:** The TLD server responds with the Authoritative Nameserver (Nameserver responsible for the specific domain).
5. **Authoritative Response:** The Authoritative Nameserver returns the requested record (A/AAAA/CNAME), which is cached locally according to its **TTL (Time To Live)** value.

---

## 📡 Task 2: HTTP in Detail

The **HyperText Transfer Protocol (HTTP)** is a stateless, client-server application-layer protocol created by Tim Berners-Lee for transmitting web assets (HTML, images, JSON). **HTTPS** encrypts this communication using TLS/SSL to preserve confidentiality and data integrity.

### Uniform Resource Locator (URL) Breakdown
A complete URL instructs the browser on how to locate and retrieve a specific web asset:

$$\text{http://user:pass@example.com:80/view-room?id=1\#task3}$$

- **Scheme (`http://`):** Declares the protocol used.
- **User Info (`user:pass@`):** Credentials embedded for basic authentication.
- **Host (`example.com`):** Domain name or IP address of the target server.
- **Port (`:80`):** Target service port (Default: `80` for HTTP, `443` for HTTPS).
- **Path (`/view-room`):** Specific directory or file location on the server.
- **Query String (`?id=1`):** Parameters passed to the backend server logic.
- **Fragment (`#task3`):** Internal document reference processed strictly client-side.

### Request & Response Structure
- **HTTP Request:** Contains a Request Line (Method, Path, HTTP Version), Request Headers, and an optional Body.
- **HTTP Response:** Contains a Status Line (HTTP Version, Status Code), Response Headers, and the Response Body (HTML/JSON data).

### Core HTTP Methods

| Method | Primary Purpose | State Impact |
| :--- | :--- | :--- |
| **GET** | Retrieves data from the server without modifying state. | Read-only / Safe. |
| **POST** | Submits data to the server to create resources or process inputs. | Modifies state. |
| **PUT** | Uploads or replaces target resources on the server. | Modifies / Overwrites state. |
| **DELETE** | Removes specified resources from the web server. | Destructive. |

### HTTP Status Codes Overview

- **`100–199` (Informational):** Request received, continuing process.
- **`200–299` (Success):** Request successfully processed (e.g., `200 OK`, `201 Created`).
- **`300–399` (Redirection):** Further action needed (e.g., `301 Moved Permanently`, `302 Found`).
- **`400–499` (Client Error):** Fault lies in client request (e.g., `400 Bad Request`, `401 Unauthorized`, `403 Forbidden`, `404 Not Found`, `405 Method Not Allowed`).
- **`500–599` (Server Error):** Server failed to fulfill a valid request (e.g., `500 Internal Server Error`, `503 Service Unavailable`).

---

## 🍪 Task 3: HTTP Headers & Cookies

Headers are key-value pairs exchanged between client and server to control metadata, caching, encoding, and identity state.

### Common HTTP Headers
- **Request Headers:**
  - `Host`: Identifies the target domain (essential for virtual hosting).
  - `User-Agent`: Supplies browser, OS, and rendering engine details.
  - `Cookie`: Transmits stored session tokens back to the server.
  - `Accept-Encoding`: Informs the server of supported compression schemes (e.g., `gzip`, `deflate`).
- **Response Headers:**
  - `Set-Cookie`: Instructs the client browser to store a cookie token.
  - `Content-Type`: Identifies the MIME type of returned data (`text/html`, `application/json`).
  - `Cache-Control`: Specifies caching directives for the browser.

### Cookies & State Management
Because HTTP is **stateless**, servers issue cookies via `Set-Cookie` response headers to track active sessions, maintain user states, and preserve personal preferences across requests.
- **Session Tokens:** Unique string identifiers issued to authenticated users. Attackers target unencrypted or weakly generated session cookies to execute session hijacking.

---

## 💻 Task 4: How Websites Work & Front-End Security

Web applications consist of two fundamental layers:
1. **Front-End (Client-Side):** The user interface rendered directly inside the browser (HTML, CSS, JavaScript).
2. **Back-End (Server-Side):** Server applications, databases, and business logic executing remote processing.

### Web Page Construction Technologies
- **HTML (HyperText Markup Language):** Defines structure using tags (`<h1>`, `<p>`, `<img>`, `<button>`). Utilizes `class` (grouping) and `id` (unique element identification) attributes.
- **CSS (Cascading Style Sheets):** Controls visual styling, layout, and presentation.
- **JavaScript (JS):** Implements dynamic logic and event triggers (e.g., `onclick`, `onhover`). Added via `<script>` tags or external `.js` files.

### Basic Web Application Vulnerabilities

#### 1. Sensitive Data Exposure
Occurs when developers leave unencrypted operational details, test API keys, cleartext credentials, or internal administrative URLs within client-side code or HTML comments (`<!-- comment -->`).
- **Assessment Approach:** Inspecting page source code (`CTRL+U` / View Source) and auditing client-side JS files.

#### 2. HTML Injection
Occurs when user input is accepted by the application and rendered into the Document Object Model (DOM) without proper sanitization or encoding.
- **Mechanism:** An attacker submits raw HTML tags (e.g., `<h1>Hacked</h1>` or malicious `<a>` links). If unencoded, the browser interprets the input as legitimate code, altering page structure or misleading users.
- **Mitigation:** Strict input sanitization and context-aware output encoding.

---

## 🏗️ Task 5: Putting It All Together (Web Infrastructure)

Complex modern web applications rely on multi-tier supporting architecture to sustain high traffic, deliver assets rapidly, and protect server infrastructure:

[ Client Browser ]
│
▼
[ Web Application Firewall (WAF) ] ── (Filters Malicious Payloads / Rate Limits)
│
▼
[ Load Balancer / CDN ] ────────── (Distributes Traffic / Serves Static Assets)
│
▼
[ Web Server (Nginx / Apache) ] ──── (Processes Virtual Hosts / Dynamic Scripts)
│
▼
[ Database (MySQL / PostgreSQL) ] ── (Stores Persistent Data)


### Supporting Infrastructure Components
- **Load Balancers:** Distribute incoming requests across multiple backend servers using algorithms such as *Round-Robin* or *Least Connections*. Conduct periodic **Health Checks** to route traffic away from offline nodes.
- **Content Delivery Networks (CDNs):** A global network of edge servers that cache static assets (images, CSS, JS) close to users geographically to minimize latency and bandwidth consumption.
- **Web Application Firewalls (WAFs):** Perimeter inspection systems positioned in front of web servers to inspect HTTP requests for signature patterns (SQLi, XSS, Command Injection) and enforce rate limiting.
- **Databases:** Relational (e.g., MySQL, PostgreSQL, MSSQL) or non-relational (e.g., MongoDB) datastores queried by backend scripts to store user data persistently.

### Web Server Operations & Dynamic Content
- **Web Servers:** Software (Apache, Nginx, IIS, Node.js) configured to serve files from a designated **Root Directory** (e.g., `/var/www/html` or `C:\inetpub\wwwroot`).
- **Virtual Hosts:** Allows a single web server instance to host multiple distinct domain names by matching the `Host` header to localized configuration files.
- **Static vs. Dynamic Content:**
  - **Static:** Files served directly as stored on disk without runtime modifications (e.g., raw HTML, PNG).
  - **Dynamic:** Web pages generated on the fly by backend scripting languages (PHP, Python, Node.js) interacting with databases before returning pure HTML to the client browser.

---

## 🚀 Module Verification & Lab Mapping

| Lab Room | Core Tested Concept | Practical Assessment Focus |
| :--- | :--- | :--- |
| **DNS in Detail** | Domain Hierarchy, Record Types, DNS Resolution Flow | Auditing DNS configurations, SPF/TXT records & subdomains. |
| **HTTP in Detail** | URLs, Methods, Status Codes & Request Formatting | Analyzing intercepted web traffic using proxy tools. |
| **Headers & Cookies** | Session Tracking, Cookies & Metadata Headers | Evaluating session security & cookie security flags. |
| **How Websites Work** | HTML/JS Fundamentals, Data Exposure & Injection | Auditing source code and testing input sanitization. |
| **Putting It All Together** | WAF, CDNs, Load Balancers & Web Architecture | Mapping modern web app infrastructure and perimeters. |

