# web-security-learning-app
A practical project documenting the construction, testing and securing of a web application. This web application runs on an ubuntu server in an isolated virtual box machine. 
The application is a notes app for users to record and read their notes. The underlying technologies include Apache, PHP, SSH and SQLite on an Ubuntu sever. The objective of this project is to obtain an understanding of how web applications are structured, where do security failures occur, what leads to vulnerabilities and how can security best-practice be implemented.

# Project Overview
The complete lifecycle of the project is as follows:
1.	Build a functional notes-taking web application.
2.	Understand its architecture and normal behaviour.
3.	Establish a secure baseline.
4.	Clone the ubuntu server to create a disposable testing environment.
5.	Introduce one controlled vulnerability at a time.
6.	Test and document each vulnerability.
7.	Identify its root cause.
8.	Apply a security fix.
9.	Retest to verify the fix.
This project is a combination of web development, Linux server administration, networking, and penetration testing methodology.

# Project Objectives
•	Understanding how a web-browser, web server, application logic, and database communicate.
•	How to configure and manage an ubuntu server.
•	Practice remote server administration through ssh.
•	Understand the difference between browser file paths and server file paths and their respective purpose
•	Understand the difference between authentication and authorisation.
•	Learn how vulnerabilities arise in an application.
•	Practice testing within an authorised and isolated environment.
•	Develop practical understanding of the OWASP Top 10 risks.
•	Document evidence, root causes, remediation and retest results.

# Scope and Authorisation 
All security tests documented in this repository were conducted against virtual machines I created for this purpose.
The lab was isolated on a VirtualBox Host-Only network. The secure baseline server was preserved and a disposable clone was used for vulnerability testing. No external websites or third-party infrastructure were targeted.

# Lab Architecture
Windows Testing Environment
|---- Web browser 
|---- Normal browser session
|---- Incognito browser session
|---- PowerShell
|---- SSH Client
		| 
|	Host only virtual network
 		|
Ubuntu server clone
|---- Apache web server
|---- PHP application
|---- SQLite database
|---- SSH service
Kali Linux is part of the broader lab plan, but Windows was used for the Broken Access Control and Authentication Failures exercises because the host computer could not comfortably run both Linux virtual machines simultaneously.

# Technology Stack 
Component 	        Purpose 
Virtual Box	        - Hosts and isolates virtual machines.
Ubuntu Server	      - Operating system hosting the application.
Apache              - Receives http requests and serves the web application.
PHP                 -	Processes authentication, notes, sessions, and other application logic.
SQLite              - Stores user accounts and notes.
SSH                 -	Provides remote control of the ubuntu server.
Windows PowerShell	- Connects remotely to the server through ssh.
Windows browser	    - Performs normal application use and testing.
Kali linux          - Planned platform for security testing.

# Application Functionality 
The application currently supports:
•	User registration 
•	Password hashing 
•	User login and logout.
•	CSRF protection 
•	Writing and viewing notes. 
•	Deleting note.
•	Separating notes by user account.  
•	Input validation 
•	Output encoding
•	Parameterised SQLite queries.
•	Server-side failed-login tracking.
•	Temporary login-rate limiting.

# Repository Structure

```text
web-security-learning-app/
├── README.md
├── LICENSE
└── Docs/
    ├── Web-application-architecture/
    │   ├── 01-lab-environment-setup.md
    │   ├── 02-server-components.md
    │   ├── 03-remote-administration-with-ssh.md
    │   ├── 04-application-construction.md
    │   ├── 05-application-architecture.md
    │   └── 06-security-design.md
    └── OWASP-top10 testing/
        ├── README.md
        ├── A01-broken-access-control.md
        ├── A07-identification-and-authentication-failures.md
        └── evidence/
            └── a07-authentication-failures/
```

# Web Application Architecture Reports

- [01 — Lab Environment Setup](Docs/Web-application-architecture/01-lab-environment-setup.md)
- [02 — Server Components](Docs/Web-application-architecture/02-server-components.md)
- [03 — Remote Administration with SSH](Docs/Web-application-architecture/03-remote-administration-with-ssh.md)
- [04 — Application Construction](Docs/Web-application-architecture/04-application-construction.md)
- [05 — Application Architecture](Docs/Web-application-architecture/05-application-architecture.md)
- [06 — Security Design](Docs/Web-application-architecture/06-security-design.md)

# OWASP Top 10 testing
Each vulnerability report contains:
•	Learning objective
•	Scope and authorization
•	Secure baseline
•	Vulnerability introduced
•	Testing methodology
•	Evidence and observations
•	Security impact
•	Root cause
•	Remediation
•	Retest results
•	Lessons learned

- [A01 — Broken Access Control](Docs/OWASP-top10%20testing/A01-broken-access-control.md)
- [A07 — Identification and Authentication Failures](Docs/OWASP-top10%20testing/A07-identification-and-authentication-failures.md)

# Current Progress

| Area | Status |
| --- | --- |
| Ubuntu Server installation | Complete |
| Apache installation and configuration | Complete |
| PHP application construction | Complete |
| SQLite database setup | Complete |
| SSH remote administration | Complete |
| Registration and authentication | Complete |
| Per-user notes | Complete |
| Secure baseline snapshot | Complete |
| Training VM clone | Complete |
| A01 Broken Access Control test, remediation and retest | Complete |
| A07 Authentication Failures test, remediation and retest | Complete |
| Additional OWASP testing | Planned |

# Future Work
Planned future exercises include:
•	Security misconfiguration
•	Cryptographic and sensitive-data protection
•	Injection
•	Insecure design
•	Software and data integrity
•	Security logging and monitoring
•	Exceptional-condition handling
The application and documentation will continue to develop as each controlled exercise is completed.

# Security Notice
The deliberately vulnerable examples in this repository are provided only for educational use in isolated and authorized environments.
Do not deploy the vulnerable versions to a public server. Do not use these techniques against systems you do not own or have explicit permission to test.
Database files, credentials, private keys, session data, logs containing sensitive information, and unredacted evidence should not be committed to the repository.

# License
The source code and project documentation are available under the MIT License.

#Author
Created by Karabo Matlou as a practical web application security learning project.

