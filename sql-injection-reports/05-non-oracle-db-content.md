
# 5. SQL Injection UNION Attack, Listing the Database Contents on Non-Oracle Databases

## 2. CVSS Score

**CVSS v3.1 Base Score: 8.0 (High)**
**CWE-89: SQL Injection**

## 3. OWASP Top 10 Reference

* **A03:2021 – Injection**
* **A01:2021 – Broken Access Control**

## 4. Description of Vulnerability

This vulnerability allows attackers to inject malicious SQL queries into user input fields and retrieve sensitive information from the database. In this case, we used **SQL Injection UNION attack** to retrieve the list of tables in the database, locate the table containing user credentials, and extract sensitive data such as usernames and passwords.

## 5. Detailed Explanation of What We Performed

We used **Burp Suite** to intercept the request setting the product category filter. We then injected a series of **UNION-based SQL injection payloads** to retrieve a list of database tables, the structure of the `users` table, and ultimately the usernames and passwords for all users. The key steps were:

### Step 1: Determine the Number of Columns

First, we determined the number of columns being returned by the query using the following payload:

```sql
'+UNION+SELECT+'abc','def'-- 
```

This payload attempts to retrieve two columns with the values `'abc'` and `'def'`. If the query returns two columns, this confirms that the query expects two columns.

### Step 2: List the Tables in the Database

Once we confirmed the number of columns, we used the following payload to retrieve the list of tables in the database:

```sql
'+UNION+SELECT+table_name,+NULL+FROM+information_schema.tables--
```

This payload targets the `information_schema.tables` table, which contains the names of all tables in the database. By injecting this payload, we could enumerate the tables and look for one that might store user credentials.

### Step 3: Find the Users Table

From the list of tables retrieved in the previous step, we identified the table that stores user credentials. Let's assume the table name was `users_abcdef`.

### Step 4: List the Columns in the Users Table

Next, we listed the columns in the `users_abcdef` table using the following payload:

```sql
'+UNION+SELECT+column_name,+NULL+FROM+information_schema.columns+WHERE+table_name='users_abcdef'--
```

This payload targets the `information_schema.columns` table to find out the column names for the `users_abcdef` table. We were able to identify columns like `username_abcdef` and `password_abcdef`.

### Step 5: Retrieve Usernames and Passwords

Now that we had the names of the relevant columns, we retrieved the usernames and passwords for all users with this payload:

```sql
'+UNION+SELECT+username_abcdef,+password_abcdef+FROM+users_abcdef--
```

This query returned the usernames and passwords for all users in the `users_abcdef` table.

### Step 6: Extract the Administrator's Credentials

After retrieving all user data, we found the credentials for the `administrator` user. Using these credentials, we were able to log in as the administrator.

## 6. Impact of the Vulnerability

* **Unauthorized access**: Attackers can access sensitive user data, including usernames and passwords, potentially leading to account takeovers and privilege escalation.
* **Full compromise of user data**: Extracting user credentials allows attackers to impersonate legitimate users and potentially gain administrative access.
* **Data leakage**: Sensitive information such as usernames and passwords are exposed, which could lead to further attacks against other systems or services.

## 7. Recommendations and Mitigations

* **Use parameterized queries** or prepared statements to prevent user input from being executed as part of a SQL query.
* **Sanitize user input** thoroughly to ensure no malicious SQL code can be injected.
* **Limit database access**: Ensure that users have only the minimum necessary permissions to access sensitive data. Specifically, avoid allowing user input to access system tables like `information_schema`.
* **Hash and salt passwords**: Store passwords securely by hashing them with a strong algorithm and salting them to prevent plain-text password theft.

## 8. References

* [OWASP Injection](https://owasp.org/www-project-top-ten/A03_2021-Injection/)
* [PortSwigger SQL Injection](https://portswigger.net/web-security/sql-injection)
* [MySQL Information Schema](https://dev.mysql.com/doc/refman/8.0/en/information-schema.html)

## 9. Step-by-Step Procedure

🔹 **Step 1: Intercept the Request**

* Open the lab titled "SQL Injection UNION Attack, Listing the Database Contents on Non-Oracle Databases."
* Start **Burp Suite** and configure your browser to route traffic through Burp's proxy.
* Visit the application and input a value for the `category` parameter in the product filter. Intercept the HTTP request using Burp Suite.

🔹 **Step 2: Determine the Number of Columns**

* In the intercepted request, locate the `category` parameter.

* Modify the `category` parameter to the following payload to determine the number of columns returned:

  ```sql
  '+UNION+SELECT+'abc','def'--
  ```

* Forward the modified request. If the application returns two columns, this confirms that the query expects two columns.

🔹 **Step 3: List the Tables in the Database**

* Once you confirm that the query is returning two columns, modify the `category` parameter to list the tables in the database:

  ```sql
  '+UNION+SELECT+table_name,+NULL+FROM+information_schema.tables--
  ```

* Forward the request to the server. The response should contain a list of table names.

🔹 **Step 4: Locate the Users Table**

* Identify the table that likely contains user credentials, such as `users_abcdef`.

* Modify the `category` parameter to list the columns of the identified table:

  ```sql
  '+UNION+SELECT+column_name,+NULL+FROM+information_schema.columns+WHERE+table_name='users_abcdef'--
  ```

* Forward the request and examine the columns for relevant data, such as `username_abcdef` and `password_abcdef`.

🔹 **Step 5: Retrieve Usernames and Passwords**

* Once you have the column names, retrieve the usernames and passwords:

  ```sql
  '+UNION+SELECT+username_abcdef,+password_abcdef+FROM+users_abcdef--
  ```

* Forward the request. The response will contain a list of usernames and passwords for all users.

🔹 **Step 6: Extract the Administrator's Credentials**

* Locate the administrator's credentials from the response. Use these credentials to log in as the administrator.

🔹 **Step 7: Verify Lab Completion**

* After successfully logging in as the administrator, verify that the lab is complete by receiving the message: "Congratulations, you solved the lab!"

## 10. Proof of Concept

*To be added manually by user.*

