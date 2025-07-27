# XSS in Misconfigured JS Frameworks

AngularJS is a frontend JavaScript framework. 
In AngularJS, developers bind variables into HTML using {{ }}:
```html
<p>Hello {{username}}!</p>
```
so if the variable `username` = Cybernerddd, then it becomes: `<p>Hello Cybernerddd!</p>`

## Real world case
In a misconfigured AngularJS app, your input might be evaluated like real code.
If you control what goes inside {{ }}, you can break out and inject JS, like:
```js
{{constructor.constructor('alert(1)')()}}
```
This means:
- `constructor.constructor` builds a new Function dynamically.
- `'alert(1)'` is passed as the code to run.
- `()` executes it

⚠It's like `eval('alert(1)')`, but sneakier.

## To see a vulnerable site
```angular
{{1+1}}
```
If the page shows 2, the expression is evaluated —> it's vulnerable!
Then you can use your real payload
```js
{{constructor.constructor('alert(document.domain)')()}}
```

## Where You'll See This
* Misconfigured apps with:
- `{{userInput}}` directly in the DOM
- AngularJS (especially ≤ v1.6)
- No $sce (Strict Contextual Escaping) in place

##  Defend
- Never bind untrusted input into Angular templates.
- Use $sce to sanitize inputs.
- Keep your AngularJS version up to date or just migrate off it lol
