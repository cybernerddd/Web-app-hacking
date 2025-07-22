# Sample Report Template: DOM XSS Edition

### 1. Title
DOM-Based XSS via `location.hash` in profile.html

### 2. Summary
A DOM-based XSS vulnerability exists in `profile.html` due to unsafe usage of
 `location.hash` being directly assigned to `innerHTML`. This allows an attacker
 to inject arbitrary JavaScript and execute it in the victim's browser.

### 3. Steps to Reproduce
`Navigate to`:
```js
https://target.com/profile.html#<img src=x onerror=alert(1)>
```
Observe that an alert is triggered due to execution of injected JavaScript via onerror.

### 4. Vulnerable Code
```js
const user = location.hash.substring(1);
document.getElementById("welcome").innerHTML = user;
```
Here, the attacker controls location.hash, which is directly injected into the DOM using innerHTML without sanitization.

### 5. Payload
```js
#<img src=x onerror=alert(1)>

Encoded:
#%3Cimg%20src%3Dx%20onerror%3Dalert(1)%3E
```
### 6.   Impact
JavaScript can be executed in the victim’s browser
Can be used to steal session cookies, deface pages, redirect users, or perform CSRF
High impact in applications handling sensitive data (e.g., admin panels, banking, etc.)

### 7. Recommended Fix
Avoid using innerHTML with unsanitized user input
Use textContent instead, or sanitize input using a library like DOMPurify:
```js
document.getElementById("welcome").textContent = user;
```
### 8. Proof of Concept (PoC)
(Include a screenshot of the alert box or a video if you're submitting a bounty)
```js
https://target.com/profile.html#<img src=x onerror=alert(1)>
```
