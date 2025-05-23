
### 📄 `03-DOM-XSS-document.write-location.search.md`

````markdown
# 1. DOM XSS in `document.write` Sink Using `location.search` Source

## 2. CVSS Score
**CVSS v3.1 Base Score: 6.1 (Medium)**  
**CWE-79: Improper Neutralization of Input During Web Page Generation ('Cross-site Scripting')**

## 3. OWASP Top 10 Reference
- **A03:2021 – Injection** – JavaScript-based injection due to insecure DOM manipulation.
- **A05:2021 – Security Misconfiguration** – Improper handling of client-side scripts and data.

## 4. Description of Vulnerability
This vulnerability is a **DOM-based XSS**, where the client-side JavaScript dynamically writes to the page using `document.write` with unsanitized input from `location.search` (i.e., the URL query string). The unsafe data is inserted inside an HTML `img` tag, and a malicious payload can be crafted to break out of the `src` attribute and inject a new element.

## 5. Detailed Explanation of What We Performed
Initially, we entered a random alphanumeric string into the search box, which was reflected in the HTML inside an `<img src="">` tag.

Example output:

```html
<img src="abc123">
````

We confirmed that the page was dynamically generating this element using `document.write` and inserting user input from `location.search`.

We then crafted a payload to **break out of the `src` attribute** and insert a new SVG tag with an `onload` event that runs JavaScript:

```
"><svg onload=alert(1)>
```

This caused the browser to interpret the payload as HTML and execute the `onload` event, displaying an alert box.

## 6. Impact of the Vulnerability

* Arbitrary JavaScript execution in the browser
* Session hijacking
* Redirection to malicious sites
* Data exfiltration
* Full compromise of user interaction with the vulnerable page

## 7. Recommendations and Mitigations

* Avoid using `document.write()` to inject untrusted data
* Sanitize and encode all user input before insertion into the DOM
* Use secure DOM APIs (e.g., `textContent`, `setAttribute`) instead of string concatenation
* Implement Content Security Policy (CSP) to restrict script execution

## 8. References

* [OWASP DOM XSS Guide](https://owasp.org/www-community/xss#dom-based-xss)
* [PortSwigger DOM-based XSS](https://portswigger.net/web-security/cross-site-scripting/dom-based)

## 9. Step-by-Step Procedure

### 🔹 Step 1: Lab Setup

* Launch the lab titled **"DOM XSS in document.write sink using source location.search"**
* Click **"Access the Lab"**

### 🔹 Step 2: Test Initial Behavior

* Enter a random alphanumeric string into the search box, e.g., `abc123`
* Click **Search**
* Open Developer Tools → Inspect the element in the response
* Observe the string being reflected inside an `img` tag like:

```html
<img src="abc123">
```

### 🔹 Step 3: Inject Payload

* Modify the query string directly in the URL with the payload:

```
?search="><svg onload=alert(1)>
```

* Full example URL:

```
https://<lab-id>.web-security-academy.net/?search="><svg onload=alert(1)>
```

### 🔹 Step 4: Observe Execution

* The script executes immediately via the `onload` attribute of the SVG element.
* A JavaScript `alert(1)` box appears.

### 🔹 Step 5: Lab Solved

* PortSwigger displays a **"Solved"** message upon successful execution.

## 10. Proof of Concept

### 🔸 Payload:

```
"><svg onload=alert(1)>
```

### 🔸 Resulting HTML:

```html
<img src=""><svg onload=alert(1)>
```

