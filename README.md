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
•	Searching notes.
•	Server-side failed-login tracking.
•	Temporary login-rate limiting.
•	User and administrator roles.
•	Server-side administrator guard and account-list dashboard.

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
        ├── A02-security-misconfiguration.md
        ├── A03-software-supply-chain-failures.md
        ├── A04-cryptographic-failures.md
        ├── A05-injection.md
        ├── A06-insecure-design.md
        ├── A07-identification-and-authentication-failures.md
        ├── A08-software-or-data-integrity-failures.md
        ├── A09-security-logging-and-alerting-failures.md
        ├── A10-mishandling-of-exceptional-conditions.md
        ├── Supplemental-SSRF.md
        └── evidence/
            ├── a02-security-misconfiguration/
            ├── a03-software-supply-chain-failures/
            ├── a04-cryptographic-failures/
            ├── a05-injection/
            ├── a06-insecure-design/
            ├── a07-authentication-failures/
            ├── a08-software-or-data-integrity-failures/
            ├── a09-security-logging/
            ├── a10-mishandling-of-exceptional-conditions/
            └── supplemental-ssrf/
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
- [A02 — Security Misconfiguration](Docs/OWASP-top10%20testing/A02-security-misconfiguration.md)
- [A03 — Software Supply Chain Failures](Docs/OWASP-top10%20testing/A03-software-supply-chain-failures.md)
- [A04 — Cryptographic Failures](Docs/OWASP-top10%20testing/A04-cryptographic-failures.md)
- [A05 — Injection](Docs/OWASP-top10%20testing/A05-injection.md)
- [A06 — Insecure Design](Docs/OWASP-top10%20testing/A06-insecure-design.md)
- [A07 — Identification and Authentication Failures](Docs/OWASP-top10%20testing/A07-identification-and-authentication-failures.md)
- [A08 — Software or Data Integrity Failures](Docs/OWASP-top10%20testing/A08-software-or-data-integrity-failures.md)
- [A09 — Security Logging and Alerting Failures](Docs/OWASP-top10%20testing/A09-security-logging-and-alerting-failures.md)
- [A10 — Mishandling of Exceptional Conditions](Docs/OWASP-top10%20testing/A10-mishandling-of-exceptional-conditions.md)

Supplemental exercise:

- [Server-Side Request Forgery](Docs/OWASP-top10%20testing/Supplemental-SSRF.md) — documented separately because SSRF was A10 in the 2021 edition, not the 2025 report set used above.

A03 documents component inventory and package maintenance rather than a reproduced compromise. A06 documents role design and a controlled administrator-authorization failure. A04 and A09 cover their recorded hardening and logging-validation workflows. Each report states its evidence limits.

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
| A02 Security Misconfiguration test, remediation and retest | Complete |
| A03 Software Supply Chain inventory and maintenance exercise | Complete; screenshot limitations documented |
| A04 Cryptographic Failures HTTPS and secure-session exercise | Complete |
| A05 Injection test, remediation and retest | Complete |
| A06 Role design, administrator authorization and retest | Complete |
| A07 Authentication Failures test, remediation and retest | Complete |
| A08 Software or Data Integrity test, restoration and retest | Complete |
| A09 Security Logging implementation and validation | Complete |
| A10 Exceptional Conditions implementation and validation | Complete |
| Supplemental SSRF test, remediation and evidence review | Complete |
| Additional security testing | Planned |

# Future Work
Planned future exercises include:
•	Expanded cryptographic and stored-data protection testing
•	Security alerting and expanded monitoring
•	Additional exceptional-condition and dependency-failure scenarios
The application and documentation will continue to develop as each controlled exercise is completed.

# Security Notice
The deliberately vulnerable examples in this repository are provided only for educational use in isolated and authorized environments.
Do not deploy the vulnerable versions to a public server. Do not use these techniques against systems you do not own or have explicit permission to test.
Database files, credentials, private keys, session data, logs containing sensitive information, and unredacted evidence should not be committed to the repository.

# License
The source code and project documentation are available under the MIT License.

#Author
Created by Karabo Matlou as a practical web application security learning project.

