# Codify — HackTheBox (Easy)

**Tags:** `#sqlite3` `#sandbox-escape`

## Reconnaissance
- Nmap scan identified three open ports: 22 (SSH), 80 (HTTP), and 3000.

## Enumeration
- Port 80 hosted a JavaScript compiler/playground that executed user-submitted code and returned the output.
- The application disclosed the JavaScript sandboxing library it used to isolate code execution.
- Researched the library and found a known vulnerability allowing an attacker to escape the sandbox and execute arbitrary code on the host.

## Foothold
- Exploited the sandbox escape to obtain a reverse shell.
- The compromised user had limited privileges; noted that the `joshua` user had a valid login shell (`/bin/bash`).
- Searched the filesystem and found a SQLite database file.
- Opened it with `sqlite3` and inspected the tables, finding a hashed password for `joshua`.
- Identified the hash type and cracked it using Hashcat.

## Privilege Escalation
- The recovered password did not work for a local `su` switch on the current session, so it was instead used to authenticate as `joshua` over SSH.

## Flags
- Not documented — notes end after obtaining SSH access as `joshua`; the root escalation path is not detailed further.
