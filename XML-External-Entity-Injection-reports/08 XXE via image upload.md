
# 1. Exploiting XXE via Image File Upload

## 2. CVSS Score

**CVSS v3.1 Base Score: 7.5 (High)**
**CWE-611: Improper Restriction of XML External Entity Reference (‘XXE’)**

## 3. OWASP Top 10 Reference

* **A05:2021 – Security Misconfiguration**
* **A01:2021 – Broken Access Control**

---

## 4. Description of Vulnerability

This lab demonstrates a **non-traditional XXE attack vector** in which user-uploaded image files are processed by the **Apache Batik SVG library**, which is vulnerable to XXE when parsing XML-based SVG images.

By injecting an **external entity** within a crafted SVG file, the attacker is able to retrieve the contents of the file `/etc/hostname` from the server, and the text is rendered within the SVG when viewed.

---

## 5. Detailed Explanation of What We Performed

In this scenario, we created a malicious SVG image that embeds an XXE payload designed to read the contents of the `/etc/hostname` file.

We then uploaded this SVG file as an **avatar** when posting a comment on a blog post. When the comment was viewed, the Batik library parsed the SVG and resolved the external entity, rendering the server's hostname inside the SVG image. We then submitted the extracted hostname as the lab solution.

---

## 6. Step-by-Step Procedure

### Step 1: Create a Malicious SVG File

1. Using any text editor, create a file named `avatar.svg` with the following content:

```xml
<?xml version="1.0" standalone="yes"?>
<!DOCTYPE test [ <!ENTITY xxe SYSTEM "file:///etc/hostname"> ]>
<svg width="128px" height="128px" xmlns="http://www.w3.org/2000/svg">
  <text font-size="16" x="0" y="16">&xxe;</text>
</svg>
```

This SVG defines an external entity `xxe` pointing to `/etc/hostname`, and attempts to render its contents using the `<text>` tag.

---

### Step 2: Upload the SVG File

2. Go to any blog post within the lab environment.
3. Post a new **comment** and use the **malicious SVG file** (`avatar.svg`) as the **avatar** image.

---

### Step 3: Trigger the XXE

4. After submitting the comment, **view the comment**.
5. The image will render the contents of `/etc/hostname` inside the SVG due to the vulnerable XML parser (Apache Batik).
6. Note down the **hostname value** displayed in the image.

---

### Step 4: Submit the Solution

7. Use the **"Submit solution"** button and enter the extracted **hostname** to complete the lab.

---

## 7. Impact of the Vulnerability

* **Information Disclosure**: Attackers can retrieve sensitive files from the server.
* **Indirect Exploitation via File Upload**: Highlights the danger of insecure file parsing in server-side libraries like Batik.
* **Chained Exploits Possible**: If used in combination with other flaws, attackers may escalate privileges or gain deeper system access.

---

## 8. Recommendations and Mitigations

* **Disable External Entity Resolution**: Configure XML parsers like Apache Batik to disallow resolving external entities.
* **Sanitize Uploaded Files**: Ensure that uploaded files are validated, sanitized, and rendered in isolation from the server.
* **Content Security Policy (CSP)**: Enforce strict CSP rules to mitigate the rendering of potentially dangerous content.
* **Use Safer Libraries**: Consider libraries that provide secure defaults for parsing XML, or restrict to image formats like PNG or JPEG for avatars.

---

## 9. References

* [OWASP XXE Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/XML_External_Entity_Prevention_Cheat_Sheet.html)
* [Apache Batik XXE Advisory](https://nvd.nist.gov/vuln/detail/CVE-2018-8013)

---

## 10. Proof of Concept

### Malicious SVG File

```xml
<?xml version="1.0" standalone="yes"?>
<!DOCTYPE test [ <!ENTITY xxe SYSTEM "file:///etc/hostname"> ]>
<svg width="128px" height="128px" xmlns="http://www.w3.org/2000/svg">
  <text font-size="16" x="0" y="16">&xxe;</text>
</svg>
```

### Result

When the image is viewed on the blog post comment, the contents of `/etc/hostname` are displayed within the SVG image.

