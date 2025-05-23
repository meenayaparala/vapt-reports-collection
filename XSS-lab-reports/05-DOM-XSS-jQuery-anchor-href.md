

### 📄 `05-DOM-XSS-jQuery-anchor-href-location.search.md`

```markdown
# 1. DOM XSS in jQuery Anchor `href` Attribute Sink Using `location.search` Source

## 2. CVSS Score
**CVSS v3.1 Base Score: 6.1 (Medium)**  
**CWE-79: Improper Neutralization of Input During Web Page Generation ('Cross-site Scripting')**

## 3. OWASP Top 10 Reference
- **A03:2021 – Injection** – Untrusted input is dynamically inserted into the DOM.
- **A05:2021 – Security Misconfiguration** – Insecure handling of `href` attributes with unsanitized input.

## 4. Description of Vulnerability
This is a **DOM-based Cross-site Scripting (XSS)** vulnerability caused by jQuery inserting unsanitized data from `location.search` into the `href` attribute of an anchor (`<a>`) element. If the value of `href` is controlled by the attacker, and no proper sanitization is applied, JavaScript URIs such as `javascript:alert(1)` can be executed when the link is clicked.

## 5. Detailed Explanation of What We Performed
In the **Submit feedback** page, we located a URL with a query parameter `returnPath`:

```

/feedback?returnPath=/abc123

```

By inspecting the page, we observed that the value of `returnPath` was being injected into the `href` attribute of a link using jQuery.

We modified the URL to:

```

/feedback?returnPath=javascript\:alert(document.cookie)

```

Then, we pressed **Enter** and clicked the "back" link rendered on the page. Since the `href` now contained a `javascript:` URI, the browser executed `alert(document.cookie)` when the link was clicked. This confirmed a **DOM XSS** vulnerability in an anchor tag’s `href` attribute.

## 6. Impact of the Vulnerability
- **JavaScript execution in the context of the application**
- **Access to sensitive information such as cookies**
- **Potential for session hijacking**
- **Redirection to malicious sites**

## 7. Recommendations and Mitigations
- Never allow untrusted input to set the `href` attribute directly
- Use `textContent` or safe link building mechanisms
- Sanitize all URL parameters before inserting into the DOM
- Block JavaScript URIs in `href` using validation or CSP
- Implement a strong **Content Security Policy (CSP)** to prevent execution of unsafe inline scripts

## 8. References
- [OWASP DOM XSS Guide](https://owasp.org/www-community/xss#dom-based-xss)
- [jQuery and DOM XSS](https://portswigger.net/web-security/cross-site-scripting/dom-based)

## 9. Step-by-Step Procedure

### 🔹 Step 1: Open the Lab
- Launch the PortSwigger lab: **"DOM XSS in jQuery anchor href attribute sink using location.search source"**
- Click **"Access the lab"**

### 🔹 Step 2: Modify URL
- Navigate to the feedback page with a random `returnPath` value to test rendering:

```

/feedback?returnPath=/test123

```

- Inspect the "back" link and note that `/test123` appears in the `href`.

### 🔹 Step 3: Inject JavaScript Payload
- Change the URL to:

```

/feedback?returnPath=javascript\:alert(document.cookie)

```

- Press Enter to reload the page

### 🔹 Step 4: Trigger the Vulnerability
- Click the "back" link with the injected `href` value
- The JavaScript code `alert(document.cookie)` will be executed, confirming successful DOM XSS

### 🔹 Step 5: Lab Confirmation
- If the lab is solved correctly, a success message will appear.

## 10. Proof of Concept

### 🔸 Payload URL:

```

/feedback?returnPath=javascript\:alert(document.cookie)

````

### 🔸 Code Injection:

```html
<a href="javascript:alert(document.cookie)">Back</a>
````

* When the link is clicked, the script is executed

---

