# 2Million — HackTheBox

**Tags:** `#json` `#command-injection` `#mysql`

## Reconnaissance
- An Nmap scan identified port 80 (HTTP) as open.

## Enumeration
- Browsed the web application after adding the target's hostname to `/etc/hosts`.
- Found a JavaScript file served at `/invite`.
- Beautified the JavaScript to make it readable and discovered an encrypted message embedded in the application's responses.
- Decrypted the message using CyberChef; an initial attempt returned an HTTP 301 redirect due to an incorrect path, but the correct invitation code was eventually recovered.
- Decoded the invitation code from Base64:
  ```bash
  echo "NUU5U1ctNzVCWTktTDhTMUItWklIREo=" | base64 -d
  ```
- Used the decoded code to register an account and log in.

## Foothold
- Searched the application for further vulnerabilities, intercepting traffic with Burp Suite.
- Found that shorter, undocumented API paths revealed additional allowed actions.
- Explored the admin endpoints and located one used to update user settings.
- Fixed the request by setting the `Content-Type` header to `application/json` and supplying the missing parameters (`{}`), successfully escalating the account to admin privileges.
- Identified a code injection vulnerability in the VPN generation feature and used it to obtain a reverse shell on the server.

## Privilege Escalation
- Enumerated the filesystem and found a `.env` file containing database credentials.
- Used the credentials to access the MySQL database and located a hashed password, which could not be cracked directly.
- Checked `/etc/passwd`, identified the `admin` user, and reused the same password successfully — retrieving `user.txt`.
- Searched for files owned by `admin`:
  ```bash
  find / -user admin 2>/dev/null | grep -v '^/sys\|^/proc\|^/run'
  ```
- Found an email-related file that pointed to a known CVE, for which a public proof-of-concept was available on GitHub.
- Transferred and executed the PoC on the target, gaining root access.

## Flags
- `user.txt` and `root.txt` obtained.
