#  DOM Manipulation for Web Hacking

> The DOM is how JavaScript controls a web page. In web hacking, you manipulate the DOM to:
> - Inject fake UIs
> - Steal user data
> - Replace page content
> - Hook events for phishing
> - Trigger XSS payloads

---
## NOTES  
`.innerHTML` → Set or replace HTML inside an element

`.value` → Read or write form input data

`.textContent` → Set plain text

`createElement()` → Build new elements

`.appendChild()` → Add your element into the page

`fetch()` → Send data to your server

---
## 📌 Select Elements

```js
document.getElementById("loginBtn")
document.querySelector("input[name='email']")
document.getElementsByTagName("input")
```
## Change Contents 
```js
document.body.innerHTML = "<h1>You’ve been hacked!</h1>"
document.querySelector("#welcome").textContent = "Hello, hacker!"
```

## Modify Form Inputs
```js
document.getElementById("email").value = "admin@evil.com"
document.querySelector("input[type='password']").value = "hunter2"
```

## Create Element
```js
let div = document.createElement("div");
div.innerHTML = "<p>Injected content</p>";
document.body.appendChild(div);
```

## Inject Fake Login Form (Phishing)
```js
let form = document.createElement("form");
form.innerHTML = `
  <input name="email" value="victim@example.com">
  <input name="password" type="password">
  <button>Login</button>
`;
document.body.appendChild(form);
```
##  Hook Events (e.g. fake button triggers)
```js
document.querySelector("button").onclick = function() {
  alert("You clicked a fake button!");
}
```
