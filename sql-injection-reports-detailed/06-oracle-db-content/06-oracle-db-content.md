
# 6. SQL Injection UNION Attack, Listing the Database Contents on Oracle

## 2. CVSS Score

**CVSS v3.1 Base Score: 8.0 (High)**
**CWE-89: SQL Injection**

## 3. OWASP Top 10 Reference

* **A03:2021 – Injection**
* **A01:2021 – Broken Access Control**

## 4. Description of Vulnerability

This vulnerability allows attackers to inject SQL queries into user input fields to manipulate the underlying database. In this case, the **SQL Injection UNION attack** is used to retrieve the list of database tables, identify the users table, and extract sensitive information such as usernames and passwords. This type of attack is particularly applicable to Oracle databases.

## 5. Detailed Explanation of What We Performed

We used **Burp Suite** to intercept and modify the request for the product category filter. We then performed a **SQL Injection UNION attack** by injecting payloads to enumerate the database structure, locate the table storing user credentials, and extract sensitive data such as usernames and passwords. Below are the key steps:

### Step 1: Determine the Number of Columns

First, we tested the query to determine how many columns it was returning. We used the following payload in the `category` parameter:

```sql
'+UNION+SELECT+'abc','def'+FROM+dual--
```

This payload attempts to retrieve two columns with values `'abc'` and `'def'`. If the query returns two columns, this confirms that the query expects two columns, and the attack can proceed.

### Step 2: List the Tables in the Database

Once we confirmed that the query was returning two columns, we used the following payload to retrieve the list of all tables in the database:

```sql
'+UNION+SELECT+table_name,NULL+FROM+all_tables--
```

This query retrieves the table names from the `all_tables` table in the Oracle database. The results showed the tables available in the database, and we searched for the table that stored user credentials.

### Step 3: Locate the Users Table

From the list of tables, we identified the table that contained user credentials. Let's assume the table name was `USERS_ABCDEF`.

### Step 4: List the Columns in the Users Table

Next, we retrieved the names of the columns in the `USERS_ABCDEF` table with the following payload:

```sql
'+UNION+SELECT+column_name,NULL+FROM+all_tab_columns+WHERE+table_name='USERS_ABCDEF'--
```

This query targets the `all_tab_columns` table, which contains metadata about all columns in the database. We were able to identify the columns related to usernames and passwords, such as `USERNAME_ABCDEF` and `PASSWORD_ABCDEF`.

### Step 5: Retrieve Usernames and Passwords

Now that we had the names of the relevant columns, we injected the following payload to retrieve the usernames and passwords for all users:

```sql
'+UNION+SELECT+USERNAME_ABCDEF,+PASSWORD_ABCDEF+FROM+USERS_ABCDEF--
```

This query returned the usernames and passwords stored in the `USERS_ABCDEF` table.

### Step 6: Extract the Administrator's Credentials

We then examined the response to find the administrator’s credentials. After retrieving the password for the `administrator` user, we used it to log in to the application as an administrator.

## 6. Impact of the Vulnerability

* **Unauthorized access**: Attackers can extract usernames and passwords from the database, potentially leading to account takeovers and privilege escalation.
* **Data leakage**: Sensitive information such as passwords and usernames is exposed, allowing attackers to impersonate legitimate users and perform actions on their behalf.
* **Full compromise of user accounts**: If attacker gains access to an administrator account, they can control the application and access other sensitive data.

## 7. Recommendations and Mitigations

* **Implement parameterized queries** or **prepared statements** to prevent SQL injection.
* **Sanitize user input** properly to ensure that user-supplied data cannot be executed as part of SQL queries.
* **Use stored procedures**: In addition to parameterized queries, stored procedures can also help mitigate injection attacks.
* **Restrict database user privileges**: Ensure that user accounts accessing the database only have the necessary permissions and do not have access to sensitive system tables.
* **Implement least privilege**: Only allow database access to users who absolutely need it. Ensure that database users have the minimal access required to perform their duties.

## 8. References

* [OWASP Injection](https://owasp.org/www-project-top-ten/A03_2021-Injection/)
* [PortSwigger SQL Injection](https://portswigger.net/web-security/sql-injection)
* [Oracle All Tab Columns](https://docs.oracle.com/cd/E11882_01/server.112/e41084/statviews_4007.htm)

## 9. Step-by-Step Procedure

🔹 **Step 1: Intercept the Request**

* Open the lab titled "SQL Injection UNION Attack, Listing the Database Contents on Oracle."
* Start **Burp Suite** and configure your browser to route traffic through Burp’s proxy.
* Visit the application and submit a value for the `category` parameter in the product filter. Intercept the HTTP request using Burp Suite.

🔹 **Step 2: Determine the Number of Columns**

* In the intercepted request, locate the `category` parameter.

* Modify the `category` parameter with the following payload to determine the number of columns:

  ```sql
  '+UNION+SELECT+'abc','def'+FROM+dual--
  ```

* Forward the modified request. If the application returns two columns, this confirms that the query expects two columns.

🔹 **Step 3: List the Tables in the Database**

* Once the number of columns is confirmed, modify the `category` parameter to list the tables:

  ```sql
  '+UNION+SELECT+table_name,NULL+FROM+all_tables--
  ```

* Forward the request and observe the response to retrieve the list of tables.

🔹 **Step 4: Locate the Users Table**

* From the list of tables, identify the table containing user credentials, such as `USERS_ABCDEF`.

* Modify the `category` parameter to list the columns of the `USERS_ABCDEF` table:

  ```sql
  '+UNION+SELECT+column_name,NULL+FROM+all_tab_columns+WHERE+table_name='USERS_ABCDEF'--
  ```

* Forward the request and review the column names for the table.

🔹 **Step 5: Retrieve Usernames and Passwords**

* Now that the relevant columns are identified, use this payload to retrieve the usernames and passwords:

  ```sql
  '+UNION+SELECT+USERNAME_ABCDEF,+PASSWORD_ABCDEF+FROM+USERS_ABCDEF--
  ```

* Forward the request to retrieve the usernames and passwords.

🔹 **Step 6: Extract the Administrator's Credentials**

* Look for the administrator’s credentials in the response and use them to log in as the administrator.

🔹 **Step 7: Verify Lab Completion**

* After successfully logging in as the administrator, verify that the lab is completed by receiving the message: "Congratulations, you solved the lab!"

## 10. Proof of Concept

*To be added manually by user.*

---
