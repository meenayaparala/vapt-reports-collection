

### 📄 `04-DOM-XSS-innerHTML-location.search.md`

```markdown
# 1. DOM XSS in `innerHTML` Sink Using `location.search` Source

## 2. CVSS Score
**CVSS v3.1 Base Score: 6.1 (Medium)**  
**CWE-79: Improper Neutralization of Input During Web Page Generation ('Cross-site Scripting')**

## 3. OWASP Top 10 Reference
- **A03:2021 – Injection** – Executing untrusted input within the browser's DOM.
- **A05:2021 – Security Misconfiguration** – Unsafe handling of dynamic content in the DOM.

## 4. Description of Vulnerability
This is a **DOM-based Cross-site Scripting (XSS)** vulnerability where user-controlled input from the URL query string (`location.search`) is directly inserted into the DOM using `element.innerHTML`. When the application uses `innerHTML` to reflect user input without proper sanitization, it allows attackers to inject arbitrary HTML or JavaScript, leading to script execution in the victim’s browser.

## 5. Detailed Explanation of What We Performed
We entered the following payload in the search box:

```

<img src=1 onerror=alert(1)>
```

When we clicked **Search**, the input was appended to the URL and passed into the page via `location.search`. The JavaScript code then used `innerHTML` to render this data on the page. The malformed image tag (`src=1`) triggered the `onerror` event, which executed `alert(1)` in the victim’s browser.

This demonstrated successful DOM XSS since the payload was executed solely on the client side by manipulating the DOM without any server-side reflection.

## 6. Impact of the Vulnerability

* **JavaScript execution in the victim's browser**
* **Account/session hijacking**
* **Redirection to malicious sites**
* **Stealing sensitive data**
* **Full control over affected DOM elements**

## 7. Recommendations and Mitigations

* Avoid using `innerHTML` to insert untrusted input
* Use `textContent` or `innerText` for safe text injection
* Sanitize and validate all user input before rendering
* Use a strong **Content Security Policy (CSP)** to block inline scripts
* Employ DOMPurify or similar libraries for sanitizing HTML safely

## 8. References

* [OWASP DOM XSS Guide](https://owasp.org/www-community/xss#dom-based-xss)
* [PortSwigger DOM-based XSS](https://portswigger.net/web-security/cross-site-scripting/dom-based)

## 9. Step-by-Step Procedure

### 🔹 Step 1: Launch Lab

* Open the PortSwigger lab titled **"DOM XSS in innerHTML sink using source location.search"**
* Click **"Access the lab"**

### 🔹 Step 2: Enter Payload

* Type this payload into the search box:

```
<img src=1 onerror=alert(1)>
```

* Click **Search**

### 🔹 Step 3: Understand the Trigger

* The page renders the search term using `innerHTML`
* Since `src=1` is invalid, the browser triggers the `onerror` event
* The `alert(1)` is executed, confirming successful XSS

### 🔹 Step 4: Lab Marked as Solved

* The lab will show a **"Solved"** message once the payload is executed successfully

## 10. Proof of Concept

### 🔸 Payload:

```
<img src=1 onerror=alert(1)>
```

### 🔸 Explanation:

* The `img` tag fails to load due to the invalid `src`
* The `onerror` attribute triggers JavaScript execution:

```html
<img src=1 onerror=alert(1)>
```

---

