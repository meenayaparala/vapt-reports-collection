# 🛠️ SQL Injection Reports

This folder contains detailed walkthroughs and vulnerability analysis for multiple **SQL Injection (SQLi)** labs. Each report outlines:

- The attack scenario
- Methodology and payloads
- Step-by-step exploitation
- Key takeaways and defenses

---

## 📄 Lab Reports

1. **[01 - Hidden Data SQLi](01-hidden-data-sqli.md)**  
   Accessing hidden or unauthorized data using basic SQL injection.

2. **[02 - Login Bypass via SQLi](02-login-bypass-sqli.md)**  
   Exploiting SQL injection to bypass login authentication mechanisms.

3. **[03 - Oracle Version Enumeration via SQLi](03-oracle-version-sqli.md)**  
   Retrieving the Oracle database version through SQL injection techniques.

4. **[04 - MySQL/MSSQL Version Disclosure](04-mysql-mssql-version-sqli.md)**  
   Extracting version details from MySQL or MSSQL servers using SQLi.

5. **[05 - Extracting Data from Non-Oracle Databases](05-non-oracle-db-content.md)**  
   Reading data from generic databases excluding Oracle, using SQL injection.

6. **[06 - Reading Oracle DB Table Content](06-oracle-db-content.md)**  
   Querying Oracle-specific structures to read table content via SQLi.

7. **[07 - UNION-Based SQLi: Finding Column Count](07-union-find-columns.md)**  
   Using `UNION SELECT` to identify the number of columns in a vulnerable query.

8. **[08 - UNION-Based SQLi: Finding Text-Compatible Column](08-union-find-text-column.md)**  
   Determining which column accepts string output to extract useful data.

9. **[09 - Extracting Data from Other Tables](09-union-other-table-data.md)**  
   Accessing different database tables using UNION-based SQL injection.

10. **[10 - Retrieving Multiple Values with UNION SELECT](10-union-retrieve-multiple-values.md)**  
    Extracting multiple fields from a database via advanced UNION techniques.

---

## 📘 Usage

These reports are ideal for:

- Learning SQL injection from beginner to advanced level
- Preparing for web app penetration testing or CTF challenges
- Understanding real-world exploitation and defensive mechanisms

---

## 🛡️ Prerequisites

- Basic understanding of SQL and relational databases
- Familiarity with HTTP requests and parameters
- Use of tools like Burp Suite, SQLMap, and manual payload crafting

---

Happy Hacking! 🐱‍💻
