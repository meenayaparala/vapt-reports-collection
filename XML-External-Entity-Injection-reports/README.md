# 📦 XML External Entity (XXE) Injection Reports

This folder contains a series of structured reports focused on **XML External Entity (XXE)** vulnerabilities. These include classic XXE, blind XXE, SSRF exploitation, file exfiltration, and variations using external/internal DTDs and XInclude.

Each lab demonstrates the discovery, exploitation, and mitigation of real-world XXE attack vectors.

---

## 📄 Lab Reports

1. **[01 - Exploiting XXE Using External Entities](01-exploiting-xxe-using-external-entities.md)**  
   Leveraging standard XML external entities to access server-side files.

2. **[02 - Exploiting XXE to Perform SSRF Attacks](02-exploiting-xxe-to-perform-ssrf-attacks.md)**  
   Using XXE to initiate server-side requests to internal services (SSRF).

3. **[03 - Blind XXE with Out-of-Band Interaction](03-blind-xxe-with-out-of-band-interaction.md)**  
   Exploiting XXE vulnerabilities without direct responses, relying on OAST interactions.

4. **[04 - Blind XXE with Parameter Entities](04-blind-xxe-with-parameter-entities.md)**  
   Using advanced parameter entities in XML to exploit XXE in silent environments.

5. **[05 - Blind XXE via External DTD](05-blind-xxe-via-external-dtd.md)**  
   Hosting a malicious DTD externally to inject payloads into the XML parser.

6. **[06 - Blind XXE via Error Messages](06-blind-xxe-via-error-messages.md)**  
   Causing parser errors to leak sensitive information in error responses.

7. **[07 - Exploiting XInclude to Retrieve Files](07-exploiting-xinclude-to-retrieve-files.md)**  
   Using the XInclude feature of XML parsers to read local files.

8. **[08 - XXE via Image Upload](08-xxe-via-image-upload.md)**  
   Triggering XXE by embedding malicious XML in image metadata uploads.

9. **[09 - Exploiting XXE via Local DTD](09-exploiting-xxe-via-local-dtd.md)**  
   Utilizing internal DTDs to achieve file read and potentially RCE.

---

## 🔍 Key Topics Covered

- Classic and Blind XXE attacks  
- SSRF via XML injection  
- File exfiltration techniques  
- XInclude abuse  
- External and local DTD payloads

---

## ⚙️ Tools and Techniques

- Burp Suite (w/ Collaborator)
- Custom XML payload crafting
- Out-of-band interaction testing
- HTTP proxies and interceptors

---

## 🎯 Suitable For

- Web security learners and researchers  
- Bug bounty hunters targeting XML parsing endpoints  
- Pentesters evaluating file handling and upload features

---

Stay curious, stay secure! 👾
