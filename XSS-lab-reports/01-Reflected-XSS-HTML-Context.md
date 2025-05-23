
### 📄 `01-Reflected-XSS-HTML-Context.md`

````markdown
# 1. Reflected XSS in HTML Context with Nothing Encoded

## 2. CVSS Score
**CVSS v3.1 Base Score: 6.1 (Medium)**  
**CWE-79: Improper Neutralization of Input During Web Page Generation ('Cross-site Scripting')**

## 3. OWASP Top 10 Reference
- **A03:2021 – Injection** – The app includes untrusted input without proper escaping or validation.
- **A07:2021 – Identification and Authentication Failures** – Potential secondary impact due to session hijacking.

## 4. Description of Vulnerability
Reflected XSS vulnerabilities occur when an application includes user-supplied input directly in the response HTML without proper encoding. In this lab, the user input from a search form is reflected into the HTML document in a way that allows JavaScript to execute.

## 5. Detailed Explanation of What We Performed
The lab provides a search form that sends the search term as a GET parameter in the URL. We tested this by injecting a simple script payload into the input field. The vulnerable application failed to sanitize the input and reflected it back directly into the HTML response, causing our JavaScript code to execute.

Example vulnerable behavior:

```html
<p>You searched for: <script>alert(1)</script></p>
````

This line is rendered as-is, leading to code execution.

## 6. Impact of the Vulnerability

* Execution of arbitrary JavaScript in victim's browser
* Session hijacking
* Defacement or phishing
* Exfiltration of sensitive data

## 7. Recommendations and Mitigations

* Use **context-aware output encoding** (e.g., HTML escape `<`, `>`, `&`, `'`, `"`)
* Use **Content Security Policy (CSP)** to limit inline scripts
* Validate and sanitize all user input on the server and client sides
* Use frameworks/libraries that auto-escape output (e.g., React, Django templates)

## 8. References

* [OWASP XSS Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html)
* [PortSwigger XSS Guide](https://portswigger.net/web-security/cross-site-scripting)

## 9. Step-by-Step Procedure

### 🔹 Step 1: Lab Setup

* Open the lab titled **"Reflected XSS in HTML context with nothing encoded"** on PortSwigger Web Security Academy.
* Click **"Access the Lab"**.

### 🔹 Step 2: Find the Input

* Locate the search bar on the website.
* It sends input via the query string, for example:

```
https://<lab-id>.web-security-academy.net/?search=test
```

### 🔹 Step 3: Inject Payload

* Enter the following into the search box:

```html
<script>alert(1)</script>
```

* Then click the **Search** button.

### 🔹 Step 4: Observe Behavior

* The payload is reflected in the HTML response and executed in the browser.
* You should see a JavaScript `alert` box pop up.

### 🔹 Step 5: Lab Solved

* Once the alert is triggered, PortSwigger confirms the lab is solved.

## 10. Proof of Concept

### 🔸 URL with Payload:

```
https://<lab-id>.web-security-academy.net/?search=<script>alert(1)</script>
```

### 🔸 Behavior:

* Executes JavaScript via direct reflection in HTML.

