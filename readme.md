# 🔐 VAPT Reports Collection

Welcome to the **Vulnerability Assessment and Penetration Testing (VAPT)** Reports Repository. This collection contains well-documented lab-based exploitation walkthroughs categorized by vulnerability type. Each report demonstrates a realistic scenario involving the discovery, exploitation, and remediation of specific web application vulnerabilities.

---

## 📁 Folder Structure

### 1. [`Authentication-vulnerability-reports`](./Authentication-vulnerability-reports)

A collection of reports focused on breaking authentication mechanisms, including 2FA bypasses, brute-force, and password reset flaws.

> **Highlights:**
- Username enumeration via multiple techniques
- 2FA logic flaws and brute-force bypass
- Password reset poisoning and middleware bypass
- Offline and multi-credential brute-forcing

### 2. [`sql-injection-reports`](./sql-injection-reports)

This section deals with classic and advanced **SQL Injection (SQLi)** attacks, including login bypasses, data exfiltration, and version enumeration.

> **Highlights:**
- UNION-based SQLi techniques
- Database version extraction (MySQL, MSSQL, Oracle)
- Hidden data retrieval
- Multi-field extraction using SQLi

### 3. [`SSTI-vulnerability-reports`](./SSTI-vulnerability-reports)

Reports on **Server-Side Template Injection (SSTI)** vulnerabilities involving various template engines and exploitation scenarios.

> **Highlights:**
- Basic SSTI detection and injection
- Code context exploitation
- Blind SSTI and sandbox bypass
- Custom exploits with documented/undocumented behaviors

### 4. [`XML-External-Entity-Injection-reports`](./XML-External-Entity-Injection-reports)

Covers **XML External Entity (XXE)** vulnerabilities including classic, blind, and file retrieval via local/remote DTDs and XInclude.

> **Highlights:**
- SSRF via XXE
- Blind XXE using OAST, DTDs, and error messages
- File exfiltration via image uploads and XInclude
- XEE attack chains using internal/external resources

---

## 🎯 Purpose

This repository is intended to:

- Help learners and bug bounty hunters understand real-world attack vectors
- Document repeatable lab scenarios for study or demonstration
- Provide step-by-step analysis and mitigation for each vulnerability class

---

## 🛠 Tools & Skills Required

- Burp Suite / HTTP Interception Tools  
- Browser DevTools and Proxy Settings  
- Basic to Intermediate Knowledge of:
  - HTTP Protocols
  - HTML/XML Parsing
  - SQL Syntax
  - Authentication Workflows

---

## 📚 How to Use

1. Navigate into a folder of your choice.
2. Open the relevant `.md` file to view the report.
3. Follow along with payloads and tips provided.
4. Learn exploitation and remediation strategies hands-on.

---


> Stay curious. Stay secure. 🔍💻

