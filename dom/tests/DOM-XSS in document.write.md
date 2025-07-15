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
After visiting the room, i figured out that search i pass in the search parameter, it gets reflected
in the URL without sanitizing my inputs.

SO I used my **TO GO** alert payload:
```js
"><img src="x" onerror=alert(1)>
```
and it alerted the screen.

## Payload explained below
> - `">` means im breaking out from a tag
> - `<img src="x" onerror=alert(1)>` : Im telling the browser to load and broken image and when the error hits,
it should print this image on screen
