
# 4. SQL Injection UNION Attack, Querying the Database Type and Version on MySQL and Microsoft

## 2. CVSS Score

**CVSS v3.1 Base Score: 7.5 (High)**
**CWE-89: SQL Injection**

## 3. OWASP Top 10 Reference

* **A03:2021 – Injection**
* **A01:2021 – Broken Access Control**

## 4. Description of Vulnerability

This vulnerability occurs when user input is not properly sanitized, allowing an attacker to manipulate the SQL query using **UNION-based SQL injection**. In this lab, the attacker uses the UNION statement to retrieve information about the database type and version. This could expose critical information that may allow attackers to exploit other vulnerabilities in the system.

## 5. Detailed Explanation of What We Performed

We used **Burp Suite** to intercept and modify the HTTP request for filtering products by category. The goal was to identify the number of columns returned by the query and determine which columns contain text data. After that, we used **SQL injection** to retrieve the database version by manipulating the SQL query.

### Step 1: Determine the Number of Columns

First, we tested the number of columns returned by the query using the following payload:

```sql
'+UNION+SELECT+'abc','def'#
```

This payload attempts to retrieve two columns, `'abc'` and `'def'`, from a special table. The `#` at the end comments out the rest of the query, ensuring that the query does not produce errors due to extra clauses. If the query returns two columns, this means the query expects two columns to be returned.

The original query might look something like this:

```sql
SELECT * FROM products WHERE category = '<user_input>';
```

After injecting the payload, it would become:

```sql
SELECT * FROM products WHERE category = '' UNION SELECT 'abc','def'--';
```

### Step 2: Retrieve the Database Version

Once we determined that the query was returning two columns, we modified the payload to query the database version:

```sql
'+UNION+SELECT+@@version,+NULL#
```

This payload is designed to display the database version (using the `@@version` global variable in MySQL or a similar mechanism in Microsoft SQL). The query was modified to:

```sql
SELECT * FROM products WHERE category = '' UNION SELECT @@version, NULL#';
```

In this case, `@@version` retrieves the database version information, and `NULL` is used to fill the second column.

## 6. Impact of the Vulnerability

* **Exposure of database version**: Exposing the database version could provide an attacker with critical information that may help them identify known vulnerabilities specific to that version.
* **Risk of further exploitation**: Once the database version is known, attackers can craft specific payloads targeting known weaknesses in the system or database.
* **Data leakage**: Further exploitation of this vulnerability could lead to leakage of sensitive data or unauthorized access to internal systems.

## 7. Recommendations and Mitigations

* **Use parameterized queries** or prepared statements to prevent user input from being executed as part of a SQL query.
* **Sanitize user input** thoroughly to ensure that no dangerous SQL code can be injected.
* **Limit database access**: Ensure that users have the minimum required permissions to access only necessary data. Limit access to system variables like `@@version`.
* **Hide database version**: Disable or restrict the retrieval of version information through queries to mitigate the risk of exposing the database version to attackers.

## 8. References

* [OWASP Injection](https://owasp.org/www-project-top-ten/A03_2021-Injection/)
* [PortSwigger SQL Injection](https://portswigger.net/web-security/sql-injection)
* [MySQL @@version](https://dev.mysql.com/doc/refman/8.0/en/information-functions.html#function_version)

## 9. Step-by-Step Procedure

🔹 **Step 1: Intercept the Request**

* Open the lab titled "SQL Injection UNION Attack, Querying the Database Type and Version on MySQL and Microsoft."
* Start **Burp Suite** and configure your browser to route traffic through Burp's proxy.
* Visit the application and input a value for the `category` parameter in the product filter. Intercept the HTTP request using Burp Suite.

🔹 **Step 2: Determine the Number of Columns**

* In the intercepted request, locate the `category` parameter.

* Modify the `category` parameter to the following payload to determine the number of columns returned:

  ```sql
  '+UNION+SELECT+'abc','def'#
  ```

* Forward the modified request. If the application returns two columns, this confirms that the query expects two columns.

🔹 **Step 3: Retrieve the Database Version**

* Once you confirm that the query is returning the correct number of columns, modify the `category` parameter again to retrieve the database version:

  ```sql
  '+UNION+SELECT+@@version,+NULL#
  ```

* Forward the request to the server. The database version should now be displayed in the response.

🔹 **Step 4: Verify Lab Completion**

* After successfully retrieving the database version, verify that the lab is complete by receiving the message: "Congratulations, you solved the lab!"

## 10. Proof of Concept

*To be added manually by user.*

