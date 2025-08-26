# Insecure Direct Object Reference

## lab

<img width="1920" height="1080" alt="Screenshot From 2025-08-25 21-31-00" src="https://github.com/user-attachments/assets/b20a590d-788e-474d-b958-8b60e8a84dec" />

Saw an IDOR vunerability in the live chat. `/chat`. 
Intercepted the request with Burp and saw the downloaded transcript being numbered systematically.(1,2,3...)
Used `Repeater` and modified the request and I was able to see other messages too.

