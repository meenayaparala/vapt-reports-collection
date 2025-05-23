
### 📄 `10-DOM-XSS-document-write-location-search.md`

```markdown
# 1. DOM XSS Using document.write with Data from location.search

## 2. CVSS Score
**CVSS v3.1 Base Score: 6.1 (Medium)**  
**CWE-79: Improper Neutralization of Input During Web Page Generation ('Cross-site Scripting')**

## 3. OWASP Top 10 Reference
- **A03:2021 – Injection** – DOM-based JavaScript injection.
- **A05:2021 – Security Misconfiguration** – Unvalidated and unsanitized client-side logic.

## 4. Description of Vulnerability
This is a **DOM-based Cross-site Scripting (XSS)** vulnerability. It occurs because the application includes unsanitized input from the URL’s query string (`location.search`) in a call to `document.write()`. The vulnerable script dynamically writes an option element into a `select` tag based on the `storeId` parameter value.

By injecting HTML and JavaScript content into the `storeId` parameter, an attacker can manipulate the DOM and execute arbitrary scripts.

## 5. Detailed Explanation of What We Performed
The vulnerable JavaScript logic on the product page reads the `storeId` value from the URL and writes it directly into a `select` element using `document.write()`.

### Benign Example:

URL:
```

product?productId=1\&storeId=abc123

````

Reflected in the page as:
```html
<select>
  <option>abc123</option>
</select>
````

We changed the `storeId` parameter to a malicious payload:

```text
"></select><img src=1 onerror=alert(1)>
```

Final URL:

```
product?productId=1&storeId="></select><img%20src=1%20onerror=alert(1)>
```

This closed the `<select>` element and injected a new `<img>` tag. The invalid `src=1` causes an error, triggering the `onerror` handler which runs `alert(1)`.

## 6. Impact of the Vulnerability

* Arbitrary JavaScript execution in user browsers
* Theft of sensitive session data or tokens
* Browser-based exploits like redirection or clickjacking
* Bypass of input validation since payload is passed via DOM

## 7. Recommendations and Mitigations

* Avoid using `document.write()` altogether
* Sanitize and validate input using trusted DOM APIs (e.g., `textContent`, `setAttribute`)
* Implement **Content Security Policy (CSP)** to restrict inline scripts
* Use JavaScript frameworks that automatically escape dynamic content

## 8. References

* [OWASP DOM XSS](https://owasp.org/www-community/attacks/DOM_Based_XSS)
* [PortSwigger: DOM-based XSS](https://portswigger.net/web-security/cross-site-scripting/dom-based)

## 9. Step-by-Step Procedure

### 🔹 Step 1: Access the Lab

* Open the **PortSwigger DOM-based XSS** lab titled *"DOM XSS using document.write with data from location.search"*
* Click **"Access the lab"**

### 🔹 Step 2: Understand the Functionality

* Visit a product page like:

```
https://<lab-id>.web-security-academy.net/product?productId=1
```

* Observe the use of a `select` drop-down for **store locations**
* Use browser DevTools or Burp to inspect how `storeId` is processed

### 🔹 Step 3: Test Benign Input

* Add a random value to the URL:

```
?productId=1&storeId=test123
```

* Confirm the string `test123` appears as an option in the drop-down

### 🔹 Step 4: Exploit the XSS Vulnerability

* Change the URL to:

```
?productId=1&storeId="></select><img%20src=1%20onerror=alert(1)>
```

* The malicious payload breaks the structure of the `select` tag and injects a new image with an error handler

### 🔹 Step 5: Confirm Execution

* The browser triggers `alert(1)` upon rendering the page
* Lab is marked **"Solved"**

## 10. Proof of Concept

### 🔸 Exploited URL:

```
https://<lab-id>.web-security-academy.net/product?productId=1&storeId="></select><img%20src=1%20onerror=alert(1)>
```

### 🔸 Resulting DOM Injection:

```html
<select>
  <option value=""></select><img src=1 onerror=alert(1)></option>
</select>
```

### 🔸 Execution:

* The alert box is triggered due to the `onerror` event

---

