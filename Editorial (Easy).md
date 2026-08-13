# Editorial — HackTheBox (Easy)

**Tags:** `#git-leak` `#ssrf` `#cve`

## Reconnaissance
- Nmap scan identified a single open port: 80 (HTTP).

## Enumeration
- Browsed the web application and found an image upload feature.
- Attempted to upload a `revShell.php` payload directly, which failed.
- Noticed the application made an internal URL call to localhost, suggesting it queried an internal service — leveraged this as a Server-Side Request Forgery (SSRF) primitive to retrieve a file listing the server's configuration options.
- The disclosed information included credentials, apparently intended for SSH access.

## Foothold
- Logged in via SSH using the recovered credentials and retrieved `user.txt`.
- Uploaded and ran LinPEAS for privilege escalation enumeration, which returned no immediately useful findings.
- Manually inspected the `/apps` directory with `ls -al` and found a hidden `.git` directory.
- Reviewed the repository history with `git show` and discovered production credentials committed in an earlier commit.

## Privilege Escalation
- Checked `sudo` permissions with `sudo -l` and found the ability to run a specific Python script.
- Identified a CVE affecting a function called within that script.
- Exploited the CVE to set the SUID bit on `/bin/bash`, obtaining a root shell.

## Flags
- `user.txt` and `root.txt` obtained.
