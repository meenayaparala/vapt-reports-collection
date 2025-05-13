
# 7. SQL Injection UNION Attack, Determining the Number of Columns Returned by the Query

## 2. CVSS Score

**CVSS v3.1 Base Score: 7.5 (High)**
**CWE-89: SQL Injection**

## 3. OWASP Top 10 Reference

* **A03:2021 – Injection**
* **A01:2021 – Broken Access Control**

## 4. Description of Vulnerability

This vulnerability occurs when user input is improperly handled in an application, allowing attackers to inject SQL code into the query. The **SQL Injection UNION attack** helps attackers manipulate the query and extract data from the database by determining how many columns the query is returning and which columns can be used to display data.

In this lab, we aim to determine the number of columns returned by the query by injecting progressively more NULL values into the `category` parameter.

## 5. Detailed Explanation of What We Performed

We used **Burp Suite** to intercept the request for the product category filter and attempted to inject SQL payloads to determine how many columns the query was returning. We began by injecting a single NULL value and then incremented the number of NULL values until the query executed successfully without returning an error.

### Step 1: Intercept the Request

First, we opened the lab titled "SQL Injection UNION Attack, Determining the Number of Columns Returned by the Query" and started **Burp Suite** to intercept the request. We configured our browser to route traffic through Burp’s proxy, and we identified the request that sets the `category` filter.

### Step 2: Inject the First Payload

Next, we modified the `category` parameter in the intercepted request by injecting a payload with a single NULL value:

```sql
'+UNION+SELECT+NULL--
```

We forwarded the modified request to the server and observed the response. As expected, the application returned an error, indicating that the query was expecting more than one column.

### Step 3: Add More NULL Values

To determine how many columns the query was returning, we continued to modify the `category` parameter by adding additional NULL values. The next payload included two NULL values:

```sql
'+UNION+SELECT+NULL,NULL--
```

We forwarded the request again and observed the response. This time, the error persisted, indicating that the query was still expecting more columns.

### Step 4: Continue Adding NULL Values

We continued adding NULL values incrementally. The next payload had three NULL values:

```sql
'+UNION+SELECT+NULL,NULL,NULL--
```

We forwarded the request and checked the response. Eventually, when we added enough NULL values to match the number of columns expected by the query, the error disappeared, and the response began to include additional content containing the NULL values.

### Step 5: Verify the Number of Columns

Once the error disappeared, we knew that the query was successfully returning the expected number of columns. By continuing to add NULL values until the response was valid, we determined the number of columns that the query returned.

## 6. Impact of the Vulnerability

* **Data Exposure**: By determining the number of columns returned, attackers can craft further payloads to extract sensitive data from the database.
* **Query Manipulation**: Attackers can leverage this knowledge to execute **SQL Injection UNION attacks** and retrieve information from other tables, potentially including usernames, passwords, and other sensitive data.

## 7. Recommendations and Mitigations

* **Use Parameterized Queries**: Always use parameterized queries or prepared statements to safely handle user input and prevent SQL injection.
* **Sanitize Input**: Properly sanitize and validate all user inputs to ensure they cannot be used to inject malicious SQL.
* **Limit Error Messages**: Configure the application to provide generic error messages and avoid disclosing sensitive information such as the structure of database queries.
* **Implement Web Application Firewalls (WAFs)**: Use a WAF to detect and block SQL injection attempts in real-time.

## 8. References

* [OWASP Injection](https://owasp.org/www-project-top-ten/A03_2021-Injection/)
* [PortSwigger SQL Injection](https://portswigger.net/web-security/sql-injection)

## 9. Step-by-Step Procedure

🔹 **Step 1: Intercept the Request**

* Open the lab titled "SQL Injection UNION Attack, Determining the Number of Columns Returned by the Query."
* Start **Burp Suite** and configure your browser to route traffic through Burp’s proxy.
* Visit the application and submit a value for the `category` parameter in the product filter. Intercept the HTTP request using Burp Suite.

🔹 **Step 2: Inject the First Payload**

* Modify the `category` parameter with the following payload to check the number of columns:

  ```sql
  '+UNION+SELECT+NULL--
  ```

* Forward the modified request and observe the response. If an error occurs, it indicates that the query expects more than one column.

🔹 **Step 3: Add More NULL Values**

* Modify the `category` parameter again, this time adding two NULL values:

  ```sql
  '+UNION+SELECT+NULL,NULL--
  ```

* Forward the request and check the response. If the error persists, the query expects even more columns.

🔹 **Step 4: Continue Adding NULL Values**

* Incrementally add NULL values to the `category` parameter, checking the response each time:

  ```sql
  '+UNION+SELECT+NULL,NULL,NULL--
  ```

* Repeat the process until the error disappears and the response includes valid content.

🔹 **Step 5: Verify Lab Completion**

* Once the error disappears, and you receive a valid response, the number of columns in the query is confirmed. This step is essential before continuing with further SQL injection techniques.

🔹 **Step 6: Verify Lab Completion**

* After successfully determining the number of columns, the lab will show a "Congratulations, you solved the lab!" message.

## 10. Proof of Concept

*To be added manually by user.*

---
