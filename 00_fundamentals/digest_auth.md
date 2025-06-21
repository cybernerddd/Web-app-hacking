# HTTP Digest Authentication

## 🔐 What Is It?
A challenge-response mechanism using hash functions to authenticate users over HTTP.

## 🧠 Key Elements:
- `nonce` – One-time number to prevent replay attacks
- `realm` – Identifies the scope of protection
- `algorithm` – Typically MD5
- Client never sends password — sends a hashed response using `username:realm:password` format

## 💥 Attack Vectors:
- Replay attacks (if nonce reused)
- Dictionary attacks on weak passwords
- Hash cracking if intercepted over HTTP

## 🔧 Tools for Testing:
- Burp Suite
- Hydra (digest module)
- Wireshark (to inspect auth headers)
- curl with `--digest` flag

