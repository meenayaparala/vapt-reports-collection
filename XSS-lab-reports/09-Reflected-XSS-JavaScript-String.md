
### 📄 `09-Reflected-XSS-JavaScript-string-angle-brackets-encoded.md`

````markdown
# 1. Reflected XSS into a JavaScript String with Angle Brackets HTML-encoded

## 2. CVSS Score
**CVSS v3.1 Base Score: 6.1 (Medium)**  
**CWE-79: Improper Neutralization of Input During Web Page Generation ('Cross-site Scripting')**

## 3. OWASP Top 10 Reference
- **A03:2021 – Injection** – JavaScript context injection.
- **A01:2021 – Broken Access Control** – Reflected input triggering unintended behavior.

## 4. Description of Vulnerability
This is a **Reflected Cross-site Scripting (XSS)** vulnerability that occurs when user input is improperly embedded inside a JavaScript string in the web page. Although angle brackets (`<` and `>`) are HTML-encoded to avoid tag injection, the real vulnerability lies in not escaping **JavaScript string delimiters** like single quotes (`'`). A malicious input can break out of the JavaScript string and execute arbitrary code.

## 5. Detailed Explanation of What We Performed
We entered a random alphanumeric string into the **search box**, intercepted the request using **Burp Suite**, and sent it to **Repeater**. We observed that the input was reflected inside a JavaScript string on the response page, for example:

```html
<script>
  var searchTerm = 'random123';
</script>
````

To exploit the vulnerability, we injected a payload that closed the string and inserted JavaScript code:

```text
'-alert(1)-'
```

The output became:

```html
<script>
  var searchTerm = ''-alert(1)-'';
</script>
```

This breaks the syntax and triggers the `alert(1)` call when the script is parsed and executed by the browser.

## 6. Impact of the Vulnerability

* Arbitrary JavaScript execution in victim's browser
* Possible session hijacking, credential theft, or page defacement
* High severity in phishing and social engineering attacks

## 7. Recommendations and Mitigations

* Escape all user input based on **context** (JavaScript, HTML, URL, etc.)
* Encode characters such as quotes, angle brackets, and slashes within scripts
* Use safe frameworks that automatically handle output encoding
* Implement **Content Security Policy (CSP)** to restrict script sources

## 8. References

* [OWASP Reflected XSS](https://owasp.org/www-community/attacks/xss/#reflected-xss-attacks)
* [PortSwigger: XSS in JavaScript Context](https://portswigger.net/web-security/cross-site-scripting/contexts/javascript)

## 9. Step-by-Step Procedure

### 🔹 Step 1: Access the Lab

* Open the **PortSwigger Reflected XSS** lab titled *"Reflected XSS into a JavaScript string with angle brackets HTML encoded"*
* Click **"Access the lab"**

### 🔹 Step 2: Test with Benign Input

* Enter a search term like `test123`
* Intercept the request using **Burp Suite**
* Forward the request and observe the response in **Repeater**
* Check if the input appears within a JavaScript string in the `<script>` tag

### 🔹 Step 3: Inject the Exploit Payload

* Replace the input with:

```text
'-alert(1)-'
```

* The reflected script should now look like:

```html
<script>
  var searchTerm = ''-alert(1)-'';
</script>
```

* When the page is loaded, the `alert(1)` will execute

### 🔹 Step 4: Verify the Exploit

* Right-click the modified request in **Repeater**
* Select **Copy URL**
* Paste it in the browser and observe the JavaScript `alert` popup

### 🔹 Step 5: Confirm Lab Completion

* Once the alert is triggered, the lab will mark as **"Solved"**

## 10. Proof of Concept

### 🔸 Exploited URL:

```
https://<lab-id>.web-security-academy.net/?search='-alert(1)-'
```

### 🔸 Resulting JavaScript:

```html
<script>
  var searchTerm = ''-alert(1)-'';
</script>
```

### 🔸 Execution:

* When the browser loads this script, `alert(1)` is triggered.

---

