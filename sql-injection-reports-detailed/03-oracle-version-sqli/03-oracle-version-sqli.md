
# 3. SQL Injection UNION Attack, Determining the Number of Columns Returned by the Query

## 2. CVSS Score

**CVSS v3.1 Base Score: 7.5 (High)**
**CWE-89: SQL Injection**

## 3. OWASP Top 10 Reference

* **A03:2021 – Injection**
* **A01:2021 – Broken Access Control**

## 4. Description of Vulnerability

This vulnerability arises from improper handling of user input, which allows attackers to manipulate the SQL query using injection. In this lab, we are exploiting a **SQL injection UNION attack** to determine the number of columns returned by a query and identify which ones contain text data. This could lead to further attacks such as extracting data from other tables or retrieving sensitive information like database versions.

## 5. Detailed Explanation of What We Performed

We used **Burp Suite** to intercept the request and injected a **UNION SELECT** payload into the `category` parameter. This UNION attack allows us to append additional queries to the original query being executed.

We first tested the number of columns being returned by the query using this payload:

```sql
'+UNION+SELECT+'abc','def'+FROM+dual--
```

If the query returns two columns, the application will display the two values `'abc'` and `'def'`.

Then, we retrieved the database version using the following payload:

```sql
'+UNION+SELECT+BANNER,+NULL+FROM+v$version--
```

This query retrieves the database version from the Oracle `v$version` system table.

## 6. Impact of the Vulnerability

* **Exposure of database configuration**: The database version can be exposed, which may allow attackers to identify specific vulnerabilities associated with that version.
* **Risk of further exploitation**: With knowledge of the database version, attackers can craft specific attacks targeting that version.
* **Data leakage**: Depending on the structure of the query, attackers could potentially retrieve sensitive data, leading to significant breaches.

## 7. Recommendations and Mitigations

* **Use parameterized queries** or prepared statements to separate SQL code from user input, ensuring user inputs are treated as data and not executable code.
* **Input sanitization**: Properly sanitize user inputs to prevent them from being used in SQL queries.
* **Restrict database access**: Limit access to system tables, such as `v$version`, for non-administrative users to prevent exposure of database details.
* **Database version management**: Consider disabling or hiding database version information to reduce the likelihood of targeted attacks.

## 8. References

* [OWASP Injection](https://owasp.org/www-project-top-ten/A03_2021-Injection/)
* [PortSwigger SQL Injection](https://portswigger.net/web-security/sql-injection)
* [Oracle v\$version](https://oracle-base.com/articles/misc/v-version-view)

## 9. Step-by-Step Procedure

🔹 **Step 1: Intercept the Request**

* Open the lab titled "SQL Injection UNION Attack, Determining the Number of Columns Returned by the Query."
* Start **Burp Suite** and configure your browser to route traffic through Burp's proxy.
* Visit the application and input a value for the `category` parameter in the filter. Intercept the HTTP request using Burp Suite.

🔹 **Step 2: Determine the Number of Columns**

* In the intercepted request, locate the `category` parameter.

* Modify the `category` parameter to the following payload to determine the number of columns returned:

  ```sql
  '+UNION+SELECT+'abc','def'+FROM+dual--
  ```

* Forward the modified request. If the application returns two columns, this confirms that the query is expecting two columns.

🔹 **Step 3: Display the Database Version**

* Once you confirm the number of columns, modify the `category` parameter again to display the database version:

  ```sql
  '+UNION+SELECT+BANNER,+NULL+FROM+v$version--
  ```

* Forward the request to the server. The database version should now be displayed in the response.

🔹 **Step 4: Verify Lab Completion**

* After completing the steps and successfully retrieving the database version, verify the lab by receiving the confirmation message: "Congratulations, you solved the lab!"

## 10. Proof of Concept

*To be added manually by user.*

---
