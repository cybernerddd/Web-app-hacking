# Insecure Direct Object Reference

> It happens when the web app exposes direct references like IDs, filenames, account numbers
without properly checking if you are authorized to access them.

### XAMPLE:
```bash
/user/profile?id=123  → your profile
/user/profile?id=124  → someone else’s profile
```

## lab

<img width="1920" height="1080" alt="Screenshot From 2025-08-25 21-31-00" src="https://github.com/user-attachments/assets/b20a590d-788e-474d-b958-8b60e8a84dec" />

- Saw an IDOR vunerability in the live chat. `/chat`. 
- Intercepted the request with Burp and saw the downloaded transcript being numbered systematically.(1,2,3...)
- Used `Repeater` and modified the request and I was able to see other messages too.


<img width="1920" height="1005" alt="Screenshot From 2025-08-25 21-38-25" src="https://github.com/user-attachments/assets/4534c96a-7612-470f-898d-4b9b5c09794d" />
