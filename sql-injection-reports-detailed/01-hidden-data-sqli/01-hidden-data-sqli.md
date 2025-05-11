Got it — thanks for the clear feedback. I’ll rewrite the report for **Lab 1** with a **more detailed step-by-step procedure**, better **technical depth**, and clear structure in every section. Let's refine this properly.

---

### 📄 Revised: `01-hidden-data-sqli.md`

````markdown
# 1. SQL Injection Vulnerability in WHERE Clause Allowing Retrieval of Hidden Data

## 2. CVSS Score
**CVSS v3.1 Base Score: 6.4 (Medium)**  
**CWE-89: Improper Neutralization of Special Elements used in an SQL Command ('SQL Injection')**

## 3. OWASP Top 10 Reference
- **A01:2021 – Broken Access Control** – Attacker accesses data they shouldn't.
- **A03:2021 – Injection** – Classic SQL injection vulnerability.

## 4. Description of Vulnerability
This vulnerability arises when user-supplied input is inserted directly into a SQL query string without proper validation or escaping. In this lab, the SQL query used to retrieve product data from the database includes unsanitized input from the URL's query string. By manipulating this input, an attacker can alter the logic of the SQL `WHERE` clause and retrieve unintended or hidden records from the database.

## 5. Detailed Explanation of What We Performed
The web application displays a product catalog where each product is accessed via a parameter in the URL (e.g., `productId=1`). We tested the behavior of this parameter by injecting SQL special characters and payloads. Upon injecting `' OR 1=1--`, the server responded with **all products**, including those that were previously not visible or meant to be hidden. This confirmed a SQL injection vulnerability, where the condition in the SQL `WHERE` clause was overridden to always return `true`.

The likely backend query:
```sql
SELECT * FROM products WHERE id = '<user_input>';
````

After injection:

```sql
SELECT * FROM products WHERE id = '' OR 1=1--';
```

This makes the condition always true (`1=1`) and bypasses any access restrictions or filters.

## 6. Impact of the Vulnerability

* Unauthorized access to sensitive or hidden records
* Potential exposure of private business data or user information
* Acts as a foundation for further attacks (e.g., enumeration, data exfiltration)

## 7. Recommendations and Mitigations

* **Use parameterized queries** (e.g., with `?` or named placeholders)
* **Input validation**: Only accept numeric input where applicable
* Implement **Web Application Firewall (WAF)** rules to block malicious patterns
* **Least privilege principle** for database accounts

## 8. References

* [OWASP SQL Injection](https://owasp.org/www-community/attacks/SQL_Injection)
* [PortSwigger: SQL injection vulnerabilities](https://portswigger.net/web-security/sql-injection)

## 9. Step-by-Step Procedure

### 🔹 Step 1: Lab Setup

* Navigate to the PortSwigger Academy SQL Injection lab titled *"SQL injection vulnerability in WHERE clause allowing retrieval of hidden data."*
* Click **"Access the lab"** to open the live instance.

### 🔹 Step 2: Explore the Application

* You'll see a product catalog or items list on the homepage.
* Observe the URL query string. Example:

  ```
  https://<lab-id>.web-security-academy.net/filter?category=Gifts
  ```

### 🔹 Step 3: Test for SQL Injection

* Append a single quote `'` to the parameter value to test how the application handles input:

  ```
  ?category=Gifts'
  ```
* The page may throw a **SQL syntax error** or behave differently — this is a strong sign of unsanitized input.

### 🔹 Step 4: Exploit the Vulnerability

* Use a payload to modify the WHERE clause logic:

  ```
  ?category=Gifts' OR 1=1--
  ```
* The `--` sequence is a SQL comment, which causes the rest of the query to be ignored.
* As a result, the backend executes:

  ```sql
  SELECT * FROM products WHERE category = 'Gifts' OR 1=1--';
  ```
* Since `1=1` is always true, the query returns all records from the database.

### 🔹 Step 5: Verify the Impact

* Observe that hidden or unrelated items now appear in the results.
* If the lab has a success criteria (e.g., “Solved” message or flag), it will appear once the injection successfully bypasses the query logic.

## 10. Proof of Concept

*To be added manually by user.*

```

---

Let me know if this level of detail is good. If yes, I’ll proceed with **Lab 2 (Login Bypass)** and so on in the same comprehensive manner.
```
