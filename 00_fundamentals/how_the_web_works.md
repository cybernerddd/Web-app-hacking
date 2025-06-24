- Client-side = frontend → where I can inject payloads
- Server-side = backend → where I can steal data or break logic
- HTTP is the language of the web — GET, POST, headers, cookies
- Session cookies = gold. If I get it, I become the user.
- XSS = inject JS. SQLi = inject SQL. IDOR = abuse broken access

# HTTP Statelessness and Cookie-Based Authentication

## 🔄 Stateless HTTP
- Each HTTP request is treated as a new conversation.
- Servers don’t remember past requests.

## 🍪 Role of Cookies
- Server uses `Set-Cookie` header to store session data in browser
- Browser sends `Cookie:` with each future request

## 🧱 Cookie Structure
Example:
    Set-Cookie: session=abc123xyz; HttpOnly; Secure; SameSite=Lax

## 🔐 Used For:
- Session tracking
- Login persistence
- CSRF prevention
- Personalization

## ⚠️ Vulnerabilities:
- Session Hijacking
- Cookie Manipulation
- Insecure Transmission