

### 📄 `08-Stored-XSS-anchor-href-double-quotes-encoded.md`

````markdown
# 1. Stored XSS into Anchor href Attribute with Double Quotes HTML-encoded

## 2. CVSS Score
**CVSS v3.1 Base Score: 7.4 (High)**  
**CWE-79: Improper Neutralization of Input During Web Page Generation ('Cross-site Scripting')**

## 3. OWASP Top 10 Reference
- **A03:2021 – Injection** – Malicious data stored and executed as script.
- **A07:2021 – Identification and Authentication Failures** – Persistent attack using stored input that impersonates legitimate links.

## 4. Description of Vulnerability
This is a **Stored XSS vulnerability**, where user input submitted through a form is stored on the server and later reflected unsafely in a web page. Specifically, user input entered into the **Website** field of a comment form is embedded in the `href` attribute of an anchor (`<a>`) tag. Although the attribute value is surrounded by double quotes, insufficient sanitization allows injection of a malicious URL scheme like `javascript:` to execute arbitrary code when the link is clicked.

## 5. Detailed Explanation of What We Performed
We submitted a comment on a blog page, including a random string in the **Website** field. Using **Burp Suite**, we intercepted the submission and confirmed that the value is later reflected inside an anchor tag like:

```html
<a href="random123">User Name</a>
````

We then modified the Website field to inject:

```text
javascript:alert(1)
```

This caused the output to become:

```html
<a href="javascript:alert(1)">User Name</a>
```

When a victim (or we) clicked the username link, the embedded `javascript:` executed and triggered a browser alert.

## 6. Impact of the Vulnerability

* Persistent script execution across all users visiting the affected page
* High risk of phishing, session hijacking, or credential theft
* Reputation damage and potential compliance violations

## 7. Recommendations and Mitigations

* Strictly validate and sanitize all user-supplied URLs (e.g., allow only `http://` or `https://`)
* Encode output appropriately for HTML attribute context
* Disallow `javascript:` schemes via server-side filtering
* Implement **Content Security Policy (CSP)** to block `javascript:` URLs

## 8. References

* [OWASP Stored XSS](https://owasp.org/www-community/attacks/xss/#stored-xss-attacks)
* [PortSwigger: Stored XSS](https://portswigger.net/web-security/cross-site-scripting/stored)

## 9. Step-by-Step Procedure

### 🔹 Step 1: Access the Lab

* Open the **PortSwigger Stored XSS** lab titled *"Stored XSS into anchor href attribute with double quotes HTML-encoded"*
* Click **"Access the lab"**

### 🔹 Step 2: Submit a Benign Comment

* Post a comment with:

  * Name: `TestUser`
  * Email: `test@example.com`
  * Website: `test123`
  * Comment: `Testing`

* Intercept the submission with **Burp Suite** and forward it

### 🔹 Step 3: Intercept the Reflected Output

* After the comment appears, view the blog post again
* Intercept the response in **Burp**, send it to **Repeater**
* Confirm that the `Website` field is reflected inside the `href` attribute of an `<a>` tag

### 🔹 Step 4: Exploit the Vulnerability

* Submit a new comment using:

  * Website: `javascript:alert(1)`
* View the comment page again
* Confirm that the anchor tag now contains:

  ```html
  <a href="javascript:alert(1)">TestUser</a>
  ```

### 🔹 Step 5: Trigger the Payload

* Right-click the user name link
* Copy the URL, paste it into the browser
* Click the link – an alert should be triggered

### 🔹 Step 6: Verify Lab Success

* Alert confirms successful XSS execution
* Lab will display **"Solved"**

## 10. Proof of Concept

### 🔸 Submitted Comment:

```http
POST /post/comment HTTP/1.1
Host: YOUR-LAB-ID.web-security-academy.net
...
name=TestUser&email=test%40example.com&website=javascript:alert(1)&comment=Hello
```

### 🔸 Resulting HTML:

```html
<a href="javascript:alert(1)">TestUser</a>
```

### 🔸 Trigger:

* Clicking the anchor tag labeled `TestUser` causes an alert popup.

---

