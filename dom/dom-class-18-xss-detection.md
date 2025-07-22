
# XSS Detection & Payload Writing
## Identifying DOM XSS sources

| Source                           | Description              |
| -------------------------------- | ------------------------ |
| `location.hash`                  | After `#` in the URL     |
| `location.search`                | After `?` in the URL     |
| `document.referrer`              | From where the user came |
| `document.cookie`                | Your cookie string       |
| `localStorage`, `sessionStorage` | Stored browser data      |
If the app grabs `values` from these and passes them to DOM sinks `unsafely`, boom XSS vulnerable

Its more dangerous if JavaScript uses those sources with these functions without sanitizing:
| Sink                    | Why its dangerous                               |
| ----------------------- | ------------------------------------------------ |
| `innerHTML`             | Renders raw HTML, inject tags                   |
| `outerHTML`             | Same as above, but replaces whole element        |
| `document.write()`      | Renders your script                              |
| `eval()`                | Direct JS execution, dangerous               |
| `setTimeout("code")`    | Executed as JS                                   |
| `location.href = value` | If `value` is `javascript:` or a crafted payload |
| `iframe.src`, `img.src` | Can execute JS if `javascript:` is allowed       |


### Suggestions
- If it’s eval(userInput), try:
  ```js
  #');alert(1);//  
  ```
or 
  ```js
  ?data=alert(1)
  ```
- If it’s location.href = input, try:
  ```js
  ?next=javascript:alert(1)
  ```

> - ALways Look at the JavaScript
> - Use DevTools > Sources → search innerHTML, eval, setTimeout, etc.
> - Trace the flow
> - Then craft the appropriate payload for the situation
