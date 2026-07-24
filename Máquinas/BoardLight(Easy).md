# BoardLight — HackTheBox (Easy)

**Tags:** `#dolibarr` `#config-file-disclosure`

## Reconnaissance
- Nmap scan found two open ports: 22 (SSH) and 80 (HTTP).

## Enumeration
- No vulnerabilities were identified on the main website.
- Directory/subdomain enumeration revealed a `/crm` path, identified as a Dolibarr CRM installation.

## Foothold
- Located and executed a public reverse shell exploit for Dolibarr, obtaining a shell on the target.

## Privilege Escalation
- Searched the filesystem for useful files, focusing on configuration files (`conf.*`).
- Found root credentials stored in plaintext inside a configuration file, allowing privilege escalation to root.

## Flags
- Root access obtained via the leaked configuration-file credentials.
