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

# Digest Authentication – Hash Calculation Flow

## 🔐 Digest Response Formula - RFC 2069

    response = MD5(HA1 : nonce : HA2)

### Components:

- **HA1** = MD5(username : realm : password)
- **HA2** = MD5(method : URI)
- **nonce** = Random one-time challenge sent by server

### 🔁 Example:
Given:
- username = bolger
- realm = hackzone
- password = Cyber123
- method = GET
- URI = /lab/webapp/digest2/1
- nonce = [server-provided]

Then:
- HA1 = MD5(bolger:hackzone:Cyber123)
- HA2 = MD5(GET:/lab/webapp/digest2/1)
- Final response = MD5(HA1 : nonce : HA2)

## 🧠 Why This Matters

- Protects password from being sent directly
- Adds randomness via nonce to prevent replay
- Still vulnerable if weak passwords + no HTTPS


