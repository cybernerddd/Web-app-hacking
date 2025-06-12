# 🧪 PortSwigger Lab 3 – Stored XSS in Blog Comment

## 🔍 Lab Description:
This lab contains a *stored cross-site scripting vulnerability* in the blog comment section.  
To solve it, submit a comment that executes JavaScript when the post is viewed.

---

## 🚀 Payload Used:
```html
<script>alert('XSS stored')</script>
