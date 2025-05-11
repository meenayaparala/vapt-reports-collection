
# 10. SQL Injection UNION Attack, Retrieving Usernames and Passwords

## 2. CVSS Score

**CVSS v3.1 Base Score: 8.0 (High)**
**CWE-89: SQL Injection**

## 3. OWASP Top 10 Reference

* **A03:2021 – Injection**
* **A01:2021 – Broken Access Control**

## 4. Description of Vulnerability

The vulnerability exploited in this lab is a **SQL Injection UNION attack**, where an attacker manipulates an SQL query by injecting arbitrary SQL statements. In this case, the attacker is able to retrieve sensitive information, such as usernames and passwords, by exploiting the application's failure to properly sanitize input. This attack uses the UNION operator to combine results from multiple SELECT statements, enabling the attacker to extract data from other tables in the database.

## 5. Detailed Explanation of What We Performed

In this lab, we used **Burp Suite** to intercept and modify the request that sets the product category filter. The aim was to determine the number of columns being returned by the query and to identify which columns contain text data. Once the structure of the query was understood, we used a **UNION** SQL injection attack to retrieve the usernames and passwords from the `users` table. We specifically verified that only one of the columns returned text data.

### Step 1: Intercept the Request

* Open the lab titled **"SQL Injection UNION Attack, Retrieving Usernames and Passwords"** and start **Burp Suite** to intercept the HTTP request.
* Configure the browser to route traffic through Burp’s proxy.
* Submit a request for a product category filter, and Burp Suite will intercept the request containing the `category` parameter.

### Step 2: Inject the Initial Payload

* Modify the `category` parameter in the intercepted request with the following SQL payload to verify the number of columns returned by the query:

  ```sql
  '+UNION+SELECT+NULL,'abc'--
  ```

* Forward the request and check the response. If the response includes valid content with one column containing text (e.g., "abc") and the other being NULL, it confirms that the query is returning two columns, only one of which contains text.

### Step 3: Inject Payload to Retrieve Data from the Users Table

* After confirming the number of columns, we inject the following payload to retrieve usernames and passwords from the `users` table:

  ```sql
  '+UNION+SELECT+NULL,username||'~'||password+FROM+users--
  ```

* This payload concatenates the `username` and `password` fields, separated by a tilde (`~`), and retrieves the data from the `users` table. Forward the modified request and observe the response.

### Step 4: Verify Data Retrieval

* If successful, the response will contain a list of usernames and passwords, formatted as `username~password`. This indicates that the **SQL injection attack** has successfully exposed sensitive user information.

## 6. Impact of the Vulnerability

* **Exposure of Sensitive Data**: Attackers can retrieve sensitive user information such as usernames and passwords.
* **Security Breach**: The attacker can gain unauthorized access to user accounts, including admin accounts, which can lead to further exploitation.
* **Privilege Escalation**: Once an attacker has access to user credentials, they can attempt privilege escalation, potentially compromising the entire system.

## 7. Recommendations and Mitigations

* **Use Parameterized Queries**: Always use parameterized queries or prepared statements to ensure that user input is not directly incorporated into SQL queries.
* **Sanitize User Input**: Ensure all user inputs are validated and sanitized before being used in SQL queries.
* **Use Web Application Firewalls (WAFs)**: A WAF can help block SQL injection attacks by identifying and preventing suspicious payloads.
* **Least Privilege Principle**: Ensure that the application and database operate with the least privilege principle, limiting what can be queried and modified.

## 8. References

* [OWASP SQL Injection](https://owasp.org/www-project-top-ten/A03_2021-Injection/)
* [PortSwigger SQL Injection](https://portswigger.net/web-security/sql-injection)

## 9. Step-by-Step Procedure

🔹 **Step 1: Intercept the Request**

* Open the lab titled **"SQL Injection UNION Attack, Retrieving Usernames and Passwords"** and start **Burp Suite** to intercept the HTTP request.
* Configure the browser to route traffic through Burp’s proxy.
* Submit a request for the `category` filter and intercept the request using Burp Suite.

🔹 **Step 2: Inject the Initial Payload**

* Modify the `category` parameter with the following SQL payload to check if the query is returning two columns:

  ```sql
  '+UNION+SELECT+NULL,'abc'--
  ```

* Forward the request and check the response. If no error occurs, and the response includes "abc" while the other column is NULL, it confirms that the query is returning two columns, one containing text.

🔹 **Step 3: Inject Payload to Retrieve Usernames and Passwords**

* Once you have confirmed the number of columns, inject a new SQL payload to retrieve usernames and passwords from the `users` table:

  ```sql
  '+UNION+SELECT+NULL,username||'~'||password+FROM+users--
  ```

* Forward the modified request and observe the response. The usernames and passwords should be displayed, separated by a tilde (\~).

🔹 **Step 4: Verify Lab Completion**

* Verify that the response contains the usernames and passwords from the `users` table. This confirms successful exploitation of the vulnerability.

🔹 **Step 5: Complete the Lab**

* Once the usernames and passwords are retrieved, you will have successfully completed the lab. A message like "Congratulations, you solved the lab!" will appear to indicate successful exploitation.

## 10. Proof of Concept

*To be added manually by user.*

---

