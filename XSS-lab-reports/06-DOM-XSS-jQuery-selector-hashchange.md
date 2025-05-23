
### 📄 `06-DOM-XSS-jQuery-selector-hashchange-event.md`

````markdown
# 1. DOM XSS in jQuery Selector Sink Using a Hashchange Event

## 2. CVSS Score
**CVSS v3.1 Base Score: 6.1 (Medium)**  
**CWE-79: Improper Neutralization of Input During Web Page Generation ('Cross-site Scripting')**

## 3. OWASP Top 10 Reference
- **A03:2021 – Injection** – JavaScript is injected and executed due to unsanitized data in DOM manipulation.
- **A05:2021 – Security Misconfiguration** – Unsafe use of `location.hash` in jQuery selectors without validation.

## 4. Description of Vulnerability
This is a **DOM-based XSS** vulnerability where an attacker manipulates the `location.hash` portion of the URL. The vulnerable application uses jQuery to read the hash fragment and dynamically select DOM elements. Since `location.hash` is attacker-controlled, unsafe use of this value within jQuery selectors (e.g., `$(location.hash)`) can lead to unintended script execution.

## 5. Detailed Explanation of What We Performed
The vulnerable application reads the fragment identifier (`location.hash`) from the URL and uses jQuery to select DOM elements. This process happens automatically when the hash changes (`hashchange` event).

By observing the DOM on the homepage, we confirmed the presence of this behavior.

We hosted a malicious iframe on the **exploit server** with the following payload in the `onload` attribute:

```html
<iframe src="https://YOUR-LAB-ID.web-security-academy.net/#" onload="this.src+='<img src=x onerror=print()>'"></iframe>
````

This iframe modifies its `src` attribute to append a malicious payload to the hash fragment. The application processes this new `location.hash` value unsafely, resulting in the execution of `print()` via the `onerror` event.

## 6. Impact of the Vulnerability

* Arbitrary JavaScript execution in the victim's browser
* Theft of sensitive user data (e.g., cookies, tokens)
* Potential for phishing or redirection attacks
* Full compromise of session and user context

## 7. Recommendations and Mitigations

* Avoid using user-controlled input in selectors like `$(location.hash)`
* Sanitize and validate fragment identifiers before using them
* Use strict **Content Security Policies (CSP)** to prevent script injection
* Upgrade jQuery to the latest version and follow secure selector practices

## 8. References

* [OWASP DOM XSS Guide](https://owasp.org/www-community/xss#dom-based-xss)
* [jQuery selector injection](https://portswigger.net/web-security/cross-site-scripting/dom-based)

## 9. Step-by-Step Procedure

### 🔹 Step 1: Open the Lab

* Launch the PortSwigger lab: **"DOM XSS in jQuery selector sink using a hashchange event"**
* Click **"Access the lab"**

### 🔹 Step 2: Observe Vulnerability

* Use browser DevTools or Burp Suite to inspect the homepage
* Notice the application listens for hash changes and uses `location.hash` in jQuery

### 🔹 Step 3: Craft Malicious Exploit

* Open the **Exploit Server** from the lab
* Add the following to the Body section:

```html
<iframe src="https://YOUR-LAB-ID.web-security-academy.net/#" onload="this.src+='<img src=x onerror=print()>'"></iframe>
```

* Replace `YOUR-LAB-ID` with your actual lab ID

### 🔹 Step 4: Host and Deliver Exploit

* Click **"Store"** to save the exploit
* Click **"View exploit"** to confirm the `print()` function is triggered (you'll see it in the console)
* Click **"Deliver to victim"** to solve the lab

### 🔹 Step 5: Lab Success

* After the victim triggers the malicious iframe, the lab should mark as **Solved**

## 10. Proof of Concept

### 🔸 Exploit Code:

```html
<iframe src="https://YOUR-LAB-ID.web-security-academy.net/#" onload="this.src+='<img src=x onerror=print()>'"></iframe>
```

* Confirms execution of attacker-controlled code via unsafe hash usage in jQuery selectors.

