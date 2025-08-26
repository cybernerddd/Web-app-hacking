# BROKEN ACCESS CONTROL(BAC)

> Its When the app fails to properly enforce authorization.
```bash
A normal user can access another user’s profile just by changing the ID.
A “user” role can hit /admin/deleteUser?id=5 and it works.
An API endpoint doesn’t check roles at all.
```
------

<img width="1920" height="1080" alt="Screenshot From 2025-08-25 21-48-25" src="https://github.com/user-attachments/assets/5720f0ed-dd81-40c9-a54a-d7507c421e25" />
### Option 1 
Intercepted the Request with Burp and changed `Admin=false` to `Admin=true` and I got admin privileges.

### Option 2
- Opened `Dev Tools` > `Storage` and in the cookies tab:
- Changed Admin=`false` to Admin=`true`.
- Shown in the `screenshot` below

<img width="1920" height="1013" alt="Screenshot From 2025-08-25 22-06-52" src="https://github.com/user-attachments/assets/4575f53b-de27-485a-8d91-bbbc8f9481b7" />
