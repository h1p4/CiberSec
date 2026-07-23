# Analytics — HackTheBox (Easy)

**Tags:** `#docker` `#metabase` `#kernel-exploit`

## Reconnaissance
- Nmap scan identified two open ports: 22 (SSH) and 80 (HTTP).

## Enumeration
- Port 80 hosted a blog site with a login page running Metabase.
- Researched the Metabase version and found a public unauthenticated Remote Code Execution (RCE) exploit.

## Foothold
- Exploited the Metabase RCE to obtain a reverse shell, though the resulting session was not a fully interactive TTY.
- The hostname resembled a serial number, suggesting the environment was a Docker container rather than the host itself.
- Ran `printenv` and found SSH credentials exposed via `.dockerenv`.
- Used the recovered credentials to log in over SSH.

## Privilege Escalation
- Checked the kernel version with `uname -r` and identified it as vulnerable to a local privilege escalation flaw (CVE-2023-2640).
- Exploited the kernel vulnerability to obtain a root shell.

## Flags
- Root access obtained via the kernel exploit.
