# 03 — Remote Administration with SSH

## Purpose

SSH allowed me to control the Ubuntu Server virtual machine remotely from Windows PowerShell. Instead of typing every command in the VirtualBox console, I could connect to the server, manage services, edit files, and troubleshoot the application from the host computer.

## Administrative workflow

```text
Windows PowerShell
       │ Encrypted SSH connection
       ▼
Ubuntu Server account
       ├── Apache service
       ├── PHP application files
       ├── SQLite storage
       └── System configuration
```

The Ubuntu VM's Host-Only IP address identifies it on the isolated network. After authentication, the Ubuntu account's permissions determine which actions are available.

## Tasks performed through SSH

I used remote administration to:

- Navigate the Linux filesystem
- Create and edit PHP files
- Install and configure required packages
- Verify Apache's service status
- Validate PHP syntax
- Inspect network addresses
- Manage file ownership and permissions
- Work with the SQLite database
- Apply and verify security changes

Some administrative actions required `sudo`. This reinforced the difference between an ordinary user session and temporarily elevated privileges.

## Security significance

SSH is not part of the public web application's normal interface. It is a management service operating behind the application. A website user sends HTTP requests to Apache, while an administrator uses SSH to manage the server.

Knowing a username, IP address, or filesystem path does not automatically provide access. An attacker would still require valid authentication or an exploitable weakness. However, unauthorized SSH access could have significant impact because the compromised account might be able to read or modify application files, access data, change configuration, stop services, or attempt privilege escalation. The actual impact depends on that account's permissions.

## Recommended protections

Important SSH protections include:

- Strong, unique authentication
- Managed SSH keys instead of reusable passwords where practical
- Disabled direct root login
- Limited accounts and least privilege
- Network restrictions and firewall rules
- Current security updates
- Authentication-log review
- Removal of unused accounts and keys

These are hardening recommendations, not a claim that every production control has already been implemented in this educational lab.

## Penetration-testing perspective

During an authorized assessment, an exposed SSH service may form part of the server's attack surface. The meaningful questions are not merely whether port 22 is open, but whether the service should be reachable, who may authenticate, what privileges they receive, whether access is logged, and how compromise would affect the application and its data.

## Lessons learned

Remote administration showed me how much control exists behind a web application. It connected web security to server security and made authentication, least privilege, logging, patching, and network restriction more concrete.

## Next report

Continue to [04 — Application Construction](04-application-construction.md).
