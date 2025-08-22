#  CSRF/XSRF (Cross-Site Request Forgery)

## 🔹 Definition
- CSRF = tricks a **logged-in victim** into making an **unintended request** on a web app.
- Attacker leverages **victim’s session cookies** (still valid) → action runs as victim.

---

## 🔹 Key Requirements
1. Victim must be **authenticated**.
2. Application must rely only on **cookies/session** (no extra checks).
3. Attacker can craft a **malicious request** that auto-submits.

---

## 🔹 Common Targets
- Change email/password
- Transfer money
- Post/delete content
- Modify profile settings

---

## 🔹 Indicators of Weakness
- Sensitive actions done via **GET/POST** with no anti-CSRF token.
- Session is authenticated **only with cookies**.
- No re-authentication or extra confirmation.

---

## 🔹 Protections
✔ CSRF Tokens (unique, unpredictable, tied to session)  
✔ SameSite cookies (`Lax` or `Strict`)  
✔ Double Submit Cookie method  
✔ Re-authentication (enter password again)  

---

## 🔹 Proof of Concept (PoC) Skeleton
```html
<html>
  <body>
    <form action="http://target.com/changeEmail" method="POST">
      <input type="hidden" name="email" value="attacker@mail.com">
      <input type="submit" value="Submit CSRF">
    </form>
    <script>document.forms[0].submit();</script>
  </body>
</html>
