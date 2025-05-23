
### 📄 `02-Stored-XSS-HTML-Context.md`

````markdown
# 1. Stored XSS in HTML Context with Nothing Encoded

## 2. CVSS Score
**CVSS v3.1 Base Score: 7.4 (High)**  
**CWE-79: Improper Neutralization of Input During Web Page Generation ('Cross-site Scripting')**

## 3. OWASP Top 10 Reference
- **A03:2021 – Injection** – The app stores and reflects untrusted input without sanitization.
- **A01:2021 – Broken Access Control** – May lead to user session takeover if combined with poor auth.

## 4. Description of Vulnerability
Stored XSS (also known as persistent XSS) occurs when user input is stored by the application (e.g., in a database or file) and later rendered in web pages without proper encoding. This type of XSS is particularly dangerous because it is automatically triggered whenever a victim visits the affected page.

## 5. Detailed Explanation of What We Performed
In this lab, a blog comment feature accepts user input (name, email, website, and comment text) and stores it. We submitted a comment containing a script tag as part of the comment text. The application failed to sanitize or encode the input, and the script was later executed when visiting the blog post, confirming a stored XSS vulnerability.

Vulnerable behavior example:

```html
<p>Great article! <script>alert(1)</script></p>
````

This gets executed when anyone reads the blog.

## 6. Impact of the Vulnerability

* Arbitrary JavaScript execution in every visitor's browser
* Session hijacking
* Credential theft
* Malware delivery
* Defacement or redirection to malicious sites

## 7. Recommendations and Mitigations

* Perform **context-aware output encoding** before rendering user input in HTML
* Apply **input validation and sanitization**
* Use **Content Security Policy (CSP)** to restrict script execution
* Escape input on both storage and rendering stages if possible
* Disable or strictly sanitize any HTML in user-generated content

## 8. References

* [OWASP XSS Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html)
* [PortSwigger Stored XSS](https://portswigger.net/web-security/cross-site-scripting/stored)

## 9. Step-by-Step Procedure

### 🔹 Step 1: Lab Setup

* Open the PortSwigger lab titled **"Stored XSS in HTML context with nothing encoded"**
* Click **"Access the Lab"**

### 🔹 Step 2: Locate the Comment Box

* Navigate to the blog post and scroll to the comment form.

### 🔹 Step 3: Inject the Payload

* In the comment box, paste:

```html
<script>alert(1)</script>
```

* Fill in other required fields:

  * Name: `XSS Tester`
  * Email: `test@example.com`
  * Website: `https://example.com`

* Click **Post Comment**

### 🔹 Step 4: Trigger the XSS

* Return to the blog post where the comment was posted.
* The script executes as soon as the comment is rendered.

### 🔹 Step 5: Lab Solved

* A JavaScript alert pops up (`alert(1)`).
* PortSwigger shows the “Solved” banner when the payload is executed.

## 10. Proof of Concept

### 🔸 Payload Used in Comment:

```html
<script>alert(1)</script>
```

### 🔸 Trigger Point:

* Visiting the blog post that contains the malicious comment.

---
