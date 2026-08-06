# Cap — HackTheBox (Easy)

**Tags:** `#suid` `#idor` `#pcap-analysis`

## Reconnaissance
- Nmap scan identified open ports:
  - 22: SSH
  - 80: HTTP

## Enumeration
- Web fuzzing on port 80 returned no significant results.
- Found a web application that analyzes Wireshark capture (PCAP) files uploaded by users.
- Identified an Insecure Direct Object Reference (IDOR) vulnerability allowing access to captures belonging to other users.

## Foothold
- Retrieved a capture containing VSFTPD connection logs with credentials transmitted in plaintext.
- Reused the extracted credentials to authenticate via SSH, gaining shell access to the system.

## Privilege Escalation
- Identified unusual file permissions on the system: the Python binary had the SUID bit set.
- Leveraged Python's SUID privileges to spawn a privileged Bash shell, escalating to root.

## Flags
- `user.txt` and `root.txt` obtained.
