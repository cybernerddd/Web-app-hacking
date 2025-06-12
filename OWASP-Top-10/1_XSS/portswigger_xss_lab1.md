# 🧪 PortSwigger Lab 1 – Reflected XSS (Nothing Encoded)

## 🔍 Lab Description:
This lab contains a simple *reflected cross-site scripting vulnerability* in the search functionality.  
The page *reflects user input directly into HTML* without encoding, allowing arbitrary JavaScript execution.

---

## 🎯 Objective:
Inject a payload that **executes the alert() function**, confirming XSS vulnerability.

---

## 🚀 Payload Used:
```html
<script>alert('XSS test')</script>
![Screenshot 2025-06-12 154238](https://github.com/user-attachments/assets/73a9de68-cc2f-4e76-b280-6e5dffe08684)
