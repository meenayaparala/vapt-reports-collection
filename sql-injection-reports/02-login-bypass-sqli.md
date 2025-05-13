
# 2. SQL Injection Vulnerability Allowing Login Bypass

## 2. CVSS Score

**CVSS v3.1 Base Score: 9.1 (Critical)**
**CWE-89: SQL Injection**

## 3. OWASP Top 10 Reference

* **A01:2021 – Broken Access Control**
* **A03:2021 – Injection**

## 4. Description of Vulnerability

This vulnerability arises when a login form directly incorporates user input into a SQL query without sanitization. It allows an attacker to manipulate the query and bypass the authentication process by injecting malicious SQL code into the input fields.

## 5. Detailed Explanation of What We Performed

In this lab, we leveraged a **SQL injection** vulnerability in the login form to bypass authentication. By intercepting the login request using **Burp Suite** and modifying the `username` parameter, we injected a payload that caused the SQL query to ignore the password check, allowing unauthorized login.

The original SQL query would likely look like:

```sql
SELECT * FROM users WHERE username = '<username>' AND password = '<password>';
```

We modified the `username` parameter by injecting the following payload:

```sql
administrator'-- 
```

This injected query looked like this after modification:

```sql
SELECT * FROM users WHERE username = 'administrator'--' AND password = '<password>';
```

In SQL, `--` indicates a comment, causing the remainder of the query to be ignored. The query then becomes:

```sql
SELECT * FROM users WHERE username = 'administrator' AND password = '';--';
```

Since the comment `--` makes the password check irrelevant, the query always evaluates to true, and the user is logged in as `administrator`.

## 6. Impact of the Vulnerability

* **Unauthorized access to user accounts** with the potential for accessing sensitive areas of the application.
* **Privilege escalation**, particularly for users with higher access levels such as administrators.
* **Bypassing authentication mechanisms**, allowing an attacker to perform actions normally restricted to authenticated users.

## 7. Recommendations and Mitigations

* **Use parameterized queries** or prepared statements to separate SQL code from user input, ensuring that user inputs are treated as data, not executable code.
* **Input validation**: Implement thorough validation and sanitization of all user inputs to prevent malicious data from entering the system.
* **Multi-factor authentication (MFA)**: Add an extra layer of protection beyond the basic username and password to prevent unauthorized login bypass.
* **Account lockouts**: Introduce mechanisms to lock accounts after multiple failed login attempts, making it harder for attackers to exploit the vulnerability.

## 8. References

* [OWASP Login Bypass](https://owasp.org/www-community/attacks/Authentication_Bypass)
* [PortSwigger SQL Injection](https://portswigger.net/web-security/sql-injection)

## 9. Step-by-Step Procedure

🔹 **Step 1: Intercept the Login Request**

* Open the lab titled "SQL Injection Vulnerability Allowing Login Bypass."
* Launch **Burp Suite** and configure your browser to route traffic through Burp's proxy.
* Visit the login page of the application and enter any username and password in the login form.
* In Burp Suite, go to the **Proxy** tab and intercept the HTTP request when you submit the login form.

🔹 **Step 2: Modify the Request**

* In the intercepted request, locate the `username` parameter. It should look something like this:

  ```plaintext
  username=admin&password=secret
  ```

* Modify the `username` parameter by injecting the SQL payload `'administrator'--`, so the request now looks like:

  ```plaintext
  username=administrator'--&password=secret
  ```

  The `--` is a SQL comment, which causes the SQL query to disregard the rest of the query after it, including the password.

🔹 **Step 3: Forward the Modified Request**

* After modifying the `username` parameter, forward the request from Burp Suite to the application server by clicking **Forward**.
* The application should process the request and authenticate the user as `administrator`, completely bypassing the password check.

🔹 **Step 4: Observe the Application Behavior**

* Once the request is forwarded, observe the behavior of the application. You should notice that you are logged in as the `administrator` without entering a valid password.
* This confirms that the login bypass was successful and that the SQL injection vulnerability has been exploited.

🔹 **Step 5: Verify Lab Completion**

* The lab will display a success message, such as "Congratulations, you solved the lab!" This confirms that the exploitation of the vulnerability has been completed successfully.

## 10. Proof of Concept

*To be added manually by user.*

---
