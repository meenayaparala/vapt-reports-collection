
# 8. SQL Injection UNION Attack, Determining the Number of Columns Returned by the Query

## 2. CVSS Score

**CVSS v3.1 Base Score: 7.5 (High)**
**CWE-89: SQL Injection**

## 3. OWASP Top 10 Reference

* **A03:2021 – Injection**
* **A01:2021 – Broken Access Control**

## 4. Description of Vulnerability

This vulnerability arises when an application improperly handles user input in SQL queries. An attacker can leverage this flaw by injecting malicious SQL code into the query, potentially leading to unauthorized access to the database. The **SQL Injection UNION attack** allows an attacker to manipulate a query to return additional data from other tables or to determine the structure of the database.

In this lab, we are tasked with determining the number of columns returned by a query and verifying that the query returns three columns using SQL injection.

## 5. Detailed Explanation of What We Performed

We used **Burp Suite** to intercept the request and modified the `category` parameter in the product filter to perform a **SQL Injection UNION attack**. We started by attempting to inject three NULL values to verify if the query returned three columns. If this led to an error, we replaced the NULL values with random data until the error disappeared, indicating that the query was correctly returning three columns.

### Step 1: Intercept the Request

* Open the lab titled **"SQL Injection UNION Attack, Determining the Number of Columns Returned by the Query"** and start **Burp Suite** to intercept the HTTP request.
* Configure your browser to route traffic through Burp's proxy.
* Submit a product category filter, and Burp Suite will intercept the request containing the `category` parameter.

### Step 2: Inject the Initial Payload

* Modify the `category` parameter in the intercepted request by injecting the following SQL payload with three NULL values:

  ```sql
  '+UNION+SELECT+NULL,NULL,NULL--
  ```

* Forward the modified request and observe the response. If the error persists, it indicates that the query does not return exactly three columns.

### Step 3: Replace NULL Values with Random Data

* If the query still returns an error after injecting three NULL values, replace each NULL value with random data provided by the lab. For example:

  ```sql
  '+UNION+SELECT+'abcdef',NULL,NULL--
  ```

* Forward the modified request and check the response. If the error persists, replace the next NULL value with random data:

  ```sql
  '+UNION+SELECT+'abcdef','12345',NULL--
  ```

* Continue this process until the error disappears and the response includes valid content. This confirms that the query returns three columns.

### Step 4: Verify the Number of Columns

* Once the error disappears, the response should include the data corresponding to the three columns. This confirms that the query is returning three columns.

## 6. Impact of the Vulnerability

* **Database Information Disclosure**: By determining the number of columns returned, attackers can craft further payloads to extract sensitive data from the database.
* **Further Exploitation**: This information can be used to launch more advanced attacks such as extracting usernames, passwords, or other sensitive data using SQL injection techniques.

## 7. Recommendations and Mitigations

* **Use Parameterized Queries**: Always use parameterized queries or prepared statements to avoid SQL injection vulnerabilities.
* **Validate and Sanitize Input**: Ensure that all user inputs are validated and sanitized to prevent malicious input from being executed as part of an SQL query.
* **Limit Error Information**: Avoid exposing detailed error messages that can reveal the structure of your database or the queries being executed.
* **Implement Web Application Firewalls (WAFs)**: Use a WAF to help detect and block SQL injection attempts in real time.

## 8. References

* [OWASP Injection](https://owasp.org/www-project-top-ten/A03_2021-Injection/)
* [PortSwigger SQL Injection](https://portswigger.net/web-security/sql-injection)

## 9. Step-by-Step Procedure

🔹 **Step 1: Intercept the Request**

* Open the lab titled **"SQL Injection UNION Attack, Determining the Number of Columns Returned by the Query."**
* Start **Burp Suite** and configure your browser to route traffic through Burp’s proxy.
* Submit a request for the `category` filter in the product search and intercept the HTTP request using Burp Suite.

🔹 **Step 2: Inject the Initial Payload**

* Modify the `category` parameter in the intercepted request with the following SQL payload to check the number of columns:

  ```sql
  '+UNION+SELECT+NULL,NULL,NULL--
  ```

* Forward the modified request and check if an error occurs. If the error persists, the query likely returns a different number of columns.

🔹 **Step 3: Replace NULL Values with Random Data**

* Replace the first NULL value with random data (e.g., `'abcdef'`):

  ```sql
  '+UNION+SELECT+'abcdef',NULL,NULL--
  ```

* If the error persists, replace the next NULL value with random data (e.g., `'12345'`):

  ```sql
  '+UNION+SELECT+'abcdef','12345',NULL--
  ```

* Repeat this process until the error disappears, indicating that the query is returning three columns.

🔹 **Step 4: Verify Lab Completion**

* Once the error disappears and valid content is returned, the query is confirmed to return three columns. The lab will indicate successful completion with a message such as "Congratulations, you solved the lab!"

🔹 **Step 5: Verify Lab Completion**

* Confirm the lab completion and proceed to further steps for advanced SQL injection techniques.

## 10. Proof of Concept

*To be added manually by user.*

---
