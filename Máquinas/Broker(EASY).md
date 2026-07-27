# Broker — HackTheBox (Easy)

**Tags:** `#activemq` `#nginx` `#sudo-misconfiguration`

## Reconnaissance
- Nmap scan revealed several open ports, including port 80 (HTTP) and an ActiveMQ management interface.

## Enumeration
- Port 80 required authentication; default credentials (`admin`/`admin`) granted access, revealing an ActiveMQ instance with no further content of interest.
- Identified the ActiveMQ version from the scan and researched it, finding a known CVE for Remote Code Execution.

## Foothold
- Exploited the ActiveMQ RCE vulnerability to obtain a reverse shell on the system.

## Privilege Escalation
- Ran `sudo -l` and found that NGINX could be executed with root privileges.
- Used `nginx -h` to identify a flag for specifying a custom configuration file.
- Crafted a custom `.conf` file enabling the WebDAV module for file uploads, set `/root` as the served directory, and bound it to an alternate port (80 was already in use).
- Started NGINX with the malicious configuration via `sudo nginx -c <custom.conf>` and confirmed with `ss -tlpn` that the new port (1337) was listening.
- Generated an SSH key pair locally with `ssh-keygen`.
- Uploaded the generated public key to `/root/.ssh/authorized_keys` by issuing an HTTP PUT request via `curl`, supplying the key's contents as the request body.
- Logged in as root using the corresponding private key:
  ```bash
  ssh -i root root@localhost
  ```

## Flags
- Root access obtained via the NGINX WebDAV upload and SSH key injection.
