# Injection — HackTheBox (Easy)

**Tags:** `#lfi` `#spring-cloud-function` `#ansible`

## Reconnaissance
- Nmap scan identified two open ports: 22 (SSH) and 8080 (HTTP).

## Enumeration
- The web application on port 8080 provided an image upload feature.
- After uploading a file, images were served via a query parameter (`img=<filename>`); tested it for path traversal and confirmed it was exploitable, yielding a Local File Inclusion (LFI).
- Used the LFI to read `www/html/webapp/pom.xml` and identified a version of Spring Cloud Function known to be vulnerable to Remote Code Execution (RCE).

## Foothold
- Exploited the Spring Cloud Function RCE to obtain a reverse shell.
- Searched the filesystem and found credentials in `/home/frank/.m2/config.xml`.
- Used the credentials to switch to the `phil` user.

## Privilege Escalation
- Ran `pspy` to monitor running processes and scheduled tasks.
- Observed root periodically executing Ansible, which ran `*.yml` playbooks from `/opt/automation/tasks`.
- Found that this directory was writable by the `root` group and by `staff`, a group `phil` belonged to.
- Created a malicious `.yml` playbook containing a reverse shell payload; Ansible executed it as root shortly after.
- Obtained a root reverse shell.

## Flags
- Root shell obtained via the Ansible automation task hijack.
