# Busqueda — HackTheBox (Easy)

**Tags:** `#searchor` `#rce` `#gitea` `#docker`

## Reconnaissance
- Nmap scan identified two open ports:
  - 22: SSH
  - 80: HTTP

## Enumeration
- The web service on port 80 used a Python library called Searchor.
- Identified that the application relied on an outdated, vulnerable version of Searchor susceptible to Remote Code Execution (RCE) via unsanitized use of `eval`.

## Foothold
- Exploited the RCE to obtain a reverse shell.
- Enumerated the filesystem and found credentials inside a `.git/config` file, which also revealed a new internal domain.
- The domain hosted a Gitea instance; logged in with the recovered credentials but found no useful information there.
- Identified a Python-based binary the current user was permitted to execute, functioning as a restricted Docker wrapper.
- Ran `docker ps` and `docker inspect` through the wrapper, revealing Gitea administrator credentials.

## Privilege Escalation
- Logged into Gitea as administrator and retrieved the source code of the script the user had permission to execute.
- Found that the script invoked another file via a relative path, meaning it resolved the file from the current working directory rather than an absolute path.
- Created a malicious file with the same name in `/tmp`, embedding a command to spawn a shell.
- Executed the wrapper from `/tmp`, causing it to load the malicious file and returning a root shell.

## Flags
- `user.txt` and `root.txt` obtained.
