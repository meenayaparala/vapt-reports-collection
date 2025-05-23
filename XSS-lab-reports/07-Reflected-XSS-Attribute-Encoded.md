
### 📄 `07-Reflected-XSS-attribute-angle-brackets-encoded.md`

````markdown
# 1. Reflected XSS into Attribute with Angle Brackets HTML-encoded

## 2. CVSS Score
**CVSS v3.1 Base Score: 6.1 (Medium)**  
**CWE-79: Improper Neutralization of Input During Web Page Generation ('Cross-site Scripting')**

## 3. OWASP Top 10 Reference
- **A03:2021 – Injection** – Input is reflected directly into HTML attributes.
- **A05:2021 – Security Misconfiguration** – Misuse of HTML escaping, allowing input to escape attribute context.

## 4. Description of Vulnerability
This **reflected XSS** vulnerability occurs when user input is included within an HTML attribute without sufficient context-aware encoding. Though angle brackets are HTML-encoded, the input is reflected inside a **quoted attribute**, making it vulnerable to breaking out of the attribute and injecting an event handler like `onmouseover`.

## 5. Detailed Explanation of What We Performed
The vulnerable application reflects user input from the search box into a double-quoted HTML attribute. While the `<` and `>` characters are encoded (`&lt;` and `&gt;`), quotes (`"`) and event handlers are not sufficiently sanitized.

We submitted a random string in the search box, intercepted the request using **Burp Suite**, and observed in the response that our input was reflected inside a quoted attribute, like:

```html
<input value="random123">
````

We then used Burp Repeater to replace our input with the payload:

```html
" onmouseover="alert(1)
```

This effectively broke out of the attribute and introduced a new `onmouseover` handler. When the page loaded, hovering the mouse over the injected element triggered the alert.

## 6. Impact of the Vulnerability

* Arbitrary JavaScript execution in a victim's browser
* Potential for session hijacking, cookie theft, or phishing
* May allow attackers to escalate to more dangerous attacks like stored XSS if combined with other vulnerabilities

## 7. Recommendations and Mitigations

* Use **context-aware output encoding** (e.g., HTML attribute encoding) based on location
* Sanitize user inputs thoroughly on both client and server
* Implement **Content Security Policy (CSP)** to block inline scripts
* Avoid reflecting user input directly into sensitive attributes

## 8. References

* [OWASP Cross-site Scripting (XSS)](https://owasp.org/www-community/attacks/xss/)
* [PortSwigger: Reflected XSS](https://portswigger.net/web-security/cross-site-scripting/reflected)

## 9. Step-by-Step Procedure

### 🔹 Step 1: Access the Lab

* Open the **PortSwigger Reflected XSS lab** titled *"Reflected XSS into attribute with angle brackets HTML-encoded"*
* Click **"Access the lab"**

### 🔹 Step 2: Test for Reflection

* Submit a random string in the **search box**
* Use **Burp Suite** to intercept the request
* Send the request to **Burp Repeater** and observe the response
* Confirm the input appears inside a quoted HTML attribute like:

```html
<input value="randomString">
```

### 🔹 Step 3: Inject Payload

* Replace the search query in Burp Repeater with:

```html
" onmouseover="alert(1)
```

* Forward the modified request

### 🔹 Step 4: Trigger the Vulnerability

* Copy the full request URL from Burp
* Paste it in a new browser tab
* Hover your mouse over the injected element to trigger the `onmouseover` alert

### 🔹 Step 5: Verify Lab Success

* The alert confirms successful XSS execution
* A **"Solved"** message should appear

## 10. Proof of Concept

### 🔸 Injected Request:

```http
GET /?search=" onmouseover="alert(1) HTTP/1.1
Host: YOUR-LAB-ID.web-security-academy.net
```

### 🔸 Outcome:

* Input reflected as:

```html
<input value="" onmouseover="alert(1)">
```

* Hovering over the field triggers the alert.

---

