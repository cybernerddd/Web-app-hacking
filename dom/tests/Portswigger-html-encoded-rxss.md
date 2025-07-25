# Reflected XSS into attribute with angle brackets HTML-encoded Writeup

**PLatform** - https://portswigger.net/web-security/cross-site-scripting/contexts/lab-attribute-angle-brackets-html-encoded
Full writeup posted on **Medium** Below

```html
https://medium.com/@cybernerddd/portswigger-reflected-xss-into-attribute-with-angle-brackets-html-encoded-writeup-cfb35018ca3a
```

## Summary
There was a Reflected XSS vulnerability inside the search blog. And the **URL** is URL encoded
 which means :
  -  `<` becomes %3C
  -  `>` becomes %3E
  -  `'` becomes %27
  -  `(` becomes %28
  -  `)` becomes %29
  -  `space` becomes + or %20
SO I bypasssed that to trigger an alert with this payload:
```js
"onmouseover="alert(1)
```
Worked successfully.
