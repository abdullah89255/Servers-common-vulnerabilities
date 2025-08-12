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

Alright, I’ll walk you through **how bug bounty hunters typically approach exploiting Apache, IIS, Nginx, MySQL, FTP, and SSH** — but we’ll keep it **ethical** so you can apply it only on authorized targets (your own lab or approved bug bounty programs).

This will be like a **pentester’s roadmap** rather than a “copy-paste attack” guide, so you understand the process from recon to exploitation.

---

## **1. Apache HTTP Server**

**Recon:**

* Find version:

  ```bash
  curl -I http://target.com
  ```

  or

  ```bash
  whatweb target.com
  ```
* Look for `/server-status` (if mod\_status enabled)

  ```bash
  curl http://target.com/server-status
  ```

**Exploitation (common in BB programs):**

* Directory traversal → `http://target.com/../../etc/passwd`
* Misconfigured modules → Test with **Nmap NSE scripts**:

  ```bash
  nmap --script http-apache-server-status target.com
  ```
* CVE search for version:

  ```bash
  searchsploit apache <version>
  ```

---

## **2. Microsoft IIS**

**Recon:**

* Detect version:

  ```bash
  whatweb target.com
  ```
* Check HTTP methods:

  ```bash
  nmap --script http-methods target.com
  ```

**Exploitation:**

* WebDAV enabled → Upload shell via `cadaver` or `davtest`
* Known CVEs for outdated versions (e.g., buffer overflow, CVE-2017-7269)
* Default files: `/iisstart.htm`, `/scripts/`

---

## **3. Nginx**

**Recon:**

* Detect version:

  ```bash
  whatweb target.com
  ```
* Check for reverse proxy misconfig:

  ```bash
  curl -I http://target.com
  ```

**Exploitation:**

* HTTP request smuggling → test with **smuggler.py**
* Path traversal if `alias` misconfigured:
  `http://target.com/static../etc/passwd`
* CVEs for Nginx modules

---

## **4. MySQL**

**Recon:**

* Test open port 3306:

  ```bash
  nmap -p 3306 --script mysql* target.com
  ```
* Try weak creds: `root:root`
  **Exploitation:**
* SQL Injection in web apps → Use **sqlmap**:

  ```bash
  sqlmap -u "http://target.com/page?id=1" --dbs
  ```
* Privilege escalation with `LOAD_FILE()` and `SELECT INTO OUTFILE` if writable.

---

## **5. FTP**

**Recon:**

* Check anonymous login:

  ```bash
  ftp target.com
  ```
* Banner grab for version:

  ```bash
  nc target.com 21
  ```

**Exploitation:**

* Anonymous login → Upload web shell if web root accessible
* Directory traversal → `GET ../../../../etc/passwd`
* Brute-force with **hydra**:

  ```bash
  hydra -l admin -P passwords.txt ftp://target.com
  ```

---

## **6. SSH**

**Recon:**

* Check port 22:

  ```bash
  nmap -p 22 target.com
  ```

**Exploitation:**

* Brute-force with **hydra** or **medusa** (only if allowed in scope):

  ```bash
  hydra -l root -P passwords.txt ssh://target.com
  ```
* CVEs for specific OpenSSH versions
* Weak key reuse (search `.ssh/authorized_keys` via other vulnerabilities)

---

💡 **Bug Bounty Tips:**

* Always start with **recon → version detection → CVE research**
* Use tools like `nmap`, `whatweb`, `gobuster`, `sqlmap`, `hydra`, `metasploit`
* Double-check **program scope** before testing — brute-forcing or denial-of-service is often *out of scope*

---

If you want, I can make you **a full exploitation flowchart** for all 6 services, so you can follow it step-by-step in a bug bounty or lab environment.
It would be like a personal **pentesting playbook** for each service.
