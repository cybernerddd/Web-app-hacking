# Lab: DOM XSS in `innerHTML` sink using source `location.search`
## Lab Loc:
> https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-innerhtml-sink

## About
 This lab contains a DOM-based cross-site scripting vulnerability in the search blog functionality. 
 It uses an `innerHTML` assignment, which changes the HTML contents of a `div element`, using data from `location.search`.

To solve this lab, perform a cross-site scripting attack that calls the alert function. 

### Vulnerability
This lab uses:
`element.innerHTML = location.search;`
That means the search parameter in the URL is being inserted directly into the page as HTML, not plain text.

## My procedure
> I needed to Trigger DOM-based XSS by injecting a script that runs in the browser using only the search bar in the URL

## Payload
Used my **GO_TO** payload:
```js
"><img src=x onerror=alert(1)>
```
So the Url became:
`https://0aa600910367554d80a10d7e00cd0008.web-security-academy.net/?search="><img src=x onerror=alert(1)>)`

## My Attack breakdown
- `?search=` is taken from the URL by `location.search`
- It's inserted into the DOM using `element.innerHTML`
- Since it's innerHTML, anything injected is rendered as HTML
- My payload breaks out of any existing tag and injects a `malicious image`
- The broken image triggers `onerror=alert(1)`

## Lessons Learnt
- `innerHTML` is super risky when used with user-controlled input.
- `DOM-based XSS` doesn’t show in requests but still causes full JS execution.
- This type of bug is common in search boxes, chat, comments, etc.
