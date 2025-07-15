# Lab: DOM XSS in `document.write` sink using source `location.search`

## Lab loc :
> https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-document-write-sink

## About
This lab contains a DOM-based cross-site scripting vulnerability in the search query 
tracking functionality. It uses the JavaScript document.write function, which writes data
out to the page. The document.write function is called with data from location.search, 
which you can control using the website URL.

> To solve this lab, perform a cross-site scripting attack that calls the alert function.

## Procedure
- After visiting the room, i figured out that search i pass in the search parameter, it gets reflected
in the URL without sanitizing my inputs.
- The page uses `document.write(location.search)` Which means the value passed via ?search= in the URL
is directly written to the HTML page without any sanitization or escaping.


SO I used my **TO GO** alert payload:
```js
"><img src="x" onerror=alert(1)>
```
As soon as I accessed the page with it, the page executed the payload and popped the alert box.
`https://0a7a002204d224f6823aba4200d500e3.web-security-academy.net/?search="><img src=x onerror=alert(1)>`

## Payload explained below
> - `">` means im breaking out from teh HTML structure
> - `<img src="x" onerror=alert(1)>` : Im telling the browser to load and broken image and when the error hits,
it should print this image on screen

## Lessons Learnt
> - `document.write()` is dangerous when used with user-controlled input.
> - `location.search ` is fully attacker-controlled from the URL
> - `DOM-based XSS` doesn’t touch the server, but still leads to full client-side execution

