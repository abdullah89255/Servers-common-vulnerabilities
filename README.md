# Servers-common-vulnerabilities
Alright — let’s break down **each one** clearly, so you know what they are, what they do, and the common vulnerabilities they can have.

---

## **1. Apache HTTP Server**

**Type:** Web Server Software
**Purpose:** Serves websites over HTTP/HTTPS. Very popular and open-source.
**Features:**

* Can host multiple sites on one server
* Supports PHP, Perl, Python, etc.
* Highly configurable with modules

**Common Vulnerabilities:**

* **Directory traversal** (accessing files outside web root)
* **Misconfigurations** (exposing server status, directories)
* **Outdated versions** (RCE – Remote Code Execution flaws)
* **Weak SSL/TLS settings** (allowing insecure encryption)
* **Module vulnerabilities** (e.g., mod\_php, mod\_ssl)

---

## **2. Microsoft IIS (Internet Information Services)**

**Type:** Web Server Software (Microsoft Windows)
**Purpose:** Hosts websites, web apps, and services.
**Features:**

* Integrates with ASP.NET
* Windows authentication
* GUI-based management

**Common Vulnerabilities:**

* **WebDAV exploits** (uploading malicious files)
* **Buffer overflows** in old versions
* **Default sample files** (attackers can test them)
* **Misconfigurations** exposing sensitive info
* **Weak authentication settings**

---

## **3. Nginx**

**Type:** Web Server / Reverse Proxy
**Purpose:** Serves websites, handles load balancing, and proxies requests.
**Features:**

* High performance, low memory use
* Can serve static files very fast
* Often used in front of Apache

**Common Vulnerabilities:**

* **HTTP request smuggling**
* **Path traversal attacks**
* **Misconfigured proxy settings** (bypassing security)
* **Information disclosure** through error pages
* **Outdated versions** with known CVEs

---

## **4. MySQL**

**Type:** Relational Database Management System (RDBMS)
**Purpose:** Stores and manages website/app data.
**Features:**

* SQL-based queries
* Open-source, widely used with PHP apps
* Supports replication, transactions

**Common Vulnerabilities:**

* **SQL Injection** (reading/changing DB via malicious queries)
* **Weak authentication** (easy-to-guess passwords)
* **Privilege escalation** from misconfigured permissions
* **Outdated server** with exploitable flaws
* **Information disclosure** (error messages leaking schema info)

---

## **5. FTP (File Transfer Protocol)**

**Type:** File Transfer Service
**Purpose:** Upload/download files to a server.
**Features:**

* Simple and widely supported
* Can use plain FTP or secure versions (FTPS/SFTP)

**Common Vulnerabilities:**

* **Cleartext passwords** (in plain FTP, easily sniffed)
* **Anonymous login** (if enabled)
* **Brute-force attacks** on weak passwords
* **Directory traversal**
* **Unpatched server software** (RCE vulnerabilities)

---

## **6. SSH (Secure Shell)**

**Type:** Remote Login Protocol
**Purpose:** Securely access and manage servers from a terminal.
**Features:**

* Encrypted communication
* Supports tunneling and file transfers (SCP/SFTP)
* Public-key authentication

**Common Vulnerabilities:**

* **Brute-force login attempts**
* **Weak or reused passwords**
* **Outdated SSH version** with crypto flaws
* **Misconfigured key-based authentication**
* **User privilege escalation** if the system is poorly configured

---

💡 **Security Tip:**
Most of these vulnerabilities are not because the software is “bad,” but because:

* The version is outdated
* It’s misconfigured
* Passwords are weak
* Unnecessary services are running

---

If you want, I can also give you **a complete table with the services, vulnerabilities, and example CVEs** so you have a ready-made reference for security work. That would make it much easier for bug bounty or pentesting use.
