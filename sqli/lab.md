# SOLVED THIS Visible error-based SQL injection

The vulneralibity of the web application was that it uses a tracking cookie for analytics, and performs a SQL query containing the value of the submitted cookie,
even though the SQL query are not returned, the application reacts different to errors and spill alot than it has to.

---

## Goal
- Was to capture the Administrator password and login access

## Approach
1. captured a GET request to the page filters
```html
GET /filter?category=Pets HTTP/1.1
Host: 0ac5003903c80e158084c1a8004f00e0.web-security-academy.net
Cookie: TrackingId=ibqb6MmdWlsIshNi; session=ur6tehNIZIBmyhzxsIOny8kmXB1M2CC9
```
2. Added a `'` to the 
