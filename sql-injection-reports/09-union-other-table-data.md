
# 9. SQL Injection UNION Attack, Retrieving Data from the Users Table

## 2. CVSS Score

**CVSS v3.1 Base Score: 8.0 (High)**
**CWE-89: SQL Injection**

## 3. OWASP Top 10 Reference

* **A03:2021 – Injection**
* **A01:2021 – Broken Access Control**

## 4. Description of Vulnerability

This vulnerability occurs when an attacker exploits a flaw in an application's SQL query by injecting malicious SQL code into an input parameter. The **SQL Injection UNION attack** allows an attacker to retrieve data from other tables in the database, bypassing the intended functionality of the application. This lab demonstrates how to use the UNION operator to retrieve sensitive data from the `users` table, specifically usernames and passwords.

## 5. Detailed Explanation of What We Performed

In this exercise, we used **Burp Suite** to intercept and modify the request that sets the product category filter. The goal was to determine the number of columns returned by the query and identify which columns contained text data. Once this was confirmed, we used a **UNION** payload to retrieve the contents of the `users` table, specifically the usernames and passwords.

### Step 1: Intercept the Request

* Open the lab titled **"SQL Injection UNION Attack, Retrieving Data from the Users Table"** and start **Burp Suite** to intercept the HTTP request.
* Configure the browser to route traffic through Burp’s proxy.
* Submit a request with a product category filter, and Burp Suite will intercept the request containing the `category` parameter.

### Step 2: Inject the Initial Payload

* Modify the `category` parameter by injecting the following SQL payload to verify the number of columns returned by the query:

  ```sql
  '+UNION+SELECT+'abc','def'--
  ```

* Forward the modified request and observe the response. If no error occurs and the response contains valid content, it indicates that the query is returning two columns, both of which contain text.

### Step 3: Retrieve Data from the Users Table

* Once the number of columns has been verified, we use a UNION attack to retrieve data from the `users` table. The payload for this attack is as follows:

  ```sql
  '+UNION+SELECT+username,+password+FROM+users--
  ```

* Forward the request with the modified payload and observe the application’s response. If successful, the response will contain a list of usernames and passwords from the `users` table.

### Step 4: Verify the Data

* The application response should display usernames and passwords from the `users` table. This confirms that SQL injection has been successfully exploited, and sensitive data has been leaked.

## 6. Impact of the Vulnerability

* **Exposure of Sensitive Data**: Attackers can gain access to sensitive data, including usernames and passwords, leading to unauthorized access to user accounts or system privilege escalation.
* **Security Breach**: Full compromise of user accounts or the entire system is possible if an attacker can obtain administrative credentials.
* **Data Loss and Privacy Violations**: Personal or financial information stored in the database could be exposed.

## 7. Recommendations and Mitigations

* **Use Parameterized Queries**: Always use parameterized queries or prepared statements to prevent SQL injection.
* **Sanitize User Input**: Ensure all user input is validated and sanitized before being included in SQL queries.
* **Use Web Application Firewalls (WAFs)**: A WAF can help detect and block SQL injection attempts in real time.
* **Limit Data Exposure**: Avoid returning sensitive information like passwords in query results. If necessary, hash passwords and store them securely.

## 8. References

* [OWASP SQL Injection](https://owasp.org/www-project-top-ten/A03_2021-Injection/)
* [PortSwigger SQL Injection](https://portswigger.net/web-security/sql-injection)

## 9. Step-by-Step Procedure

🔹 **Step 1: Intercept the Request**

* Open the lab titled **"SQL Injection UNION Attack, Retrieving Data from the Users Table"** and start **Burp Suite** to intercept the HTTP request.
* Configure the browser to route traffic through Burp's proxy.
* Submit a request for the `category` filter and intercept the request using Burp Suite.

🔹 **Step 2: Inject the Initial Payload**

* Modify the `category` parameter in the intercepted request with the following SQL payload to check if the query is returning two columns:

  ```sql
  '+UNION+SELECT+'abc','def'--
  ```

* Forward the request and check the response. If no error occurs and the response includes valid content, it indicates the query is returning two columns containing text.

🔹 **Step 3: Inject Payload to Retrieve Data from Users Table**

* Once we’ve confirmed the number of columns, we inject a new SQL payload to retrieve data from the `users` table:

  ```sql
  '+UNION+SELECT+username,+password+FROM+users--
  ```

* Forward the modified request and observe the response. If successful, the response will contain usernames and passwords from the `users` table.

🔹 **Step 4: Verify Lab Completion**

* Verify that the response contains usernames and passwords from the `users` table. This confirms successful exploitation of the vulnerability.

🔹 **Step 5: Complete the Lab**

* Once the usernames and passwords are retrieved, you will have successfully completed the lab. A message like "Congratulations, you solved the lab!" will appear to indicate successful exploitation.

## 10. Proof of Concept

*To be added manually by user.*

---
