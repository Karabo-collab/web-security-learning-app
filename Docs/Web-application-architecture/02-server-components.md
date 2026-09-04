# 02 — Server Components

## Overview

The notes application is supported by several components that work together. Building the environment helped me understand that a web application is not only the page visible in a browser. It depends on an operating system, web server, application logic, data storage, network services, and access controls.

## Component summary

| Component | Responsibility |
|---|---|
| VirtualBox | Hosts and isolates the virtual machines |
| Ubuntu Server | Provides the operating system and service environment |
| Apache | Receives HTTP requests and returns web responses |
| PHP | Executes the application's server-side logic |
| SQLite | Stores users, password hashes, and notes |
| SSH | Provides authorized remote server administration |
| Windows browser | Displays the application and sends test requests |
| PowerShell | Provides the SSH client used to manage Ubuntu remotely |

## Ubuntu Server

Ubuntu Server is the foundation of the lab. It manages users, files, permissions, processes, networking, installed packages, and services.

This helped me distinguish between application security and host security. Secure PHP code can still be placed at risk by weak operating-system credentials, unsafe permissions, unnecessary services, or missing updates.

## Apache

Apache is the web server. It listens for HTTP requests, locates the requested resource, invokes PHP when required, and returns the resulting response to the browser.

The application's public files are stored under:

```text
/var/www/html/
```

Viewing Apache's default page confirmed that Windows could reach Ubuntu before the application was developed. Replacing the default page with the first PHP page then confirmed that Apache and PHP were working together.

## PHP

PHP implements the application's behavior. It handles registration, authentication, sessions, CSRF validation, input validation, database operations, note functions, authorization decisions, and output encoding.

PHP runs on the server. The browser receives the generated response rather than the original PHP source code. This distinction matters because the server-side code contains the logic that determines whether a request is accepted or rejected.

## SQLite

SQLite stores application data in a local database file. The project uses it for user accounts and notes without requiring a separate database-server process.

The database is stored outside the public web root:

```text
/var/lib/web-lab/app.db
```

Keeping the database outside `/var/www/html/` reduces the risk of it being served as a normal web file. Appropriate file ownership and permissions remain necessary.

## SSH

SSH provides an encrypted remote command-line connection to Ubuntu Server. I use Windows PowerShell to administer the server, check services, edit application files, and inspect the lab.

Because SSH can provide extensive control over the host, it is an important administrative security boundary. It is discussed further in [03 — Remote Administration with SSH](03-remote-administration-with-ssh.md).

## How the components communicate

```text
Windows browser
      │ HTTP request
      ▼
Apache on Ubuntu Server
      │ Executes PHP
      ▼
PHP application logic
      │ Parameterized query
      ▼
SQLite database
      │ Result
      ▼
PHP → Apache → HTTP response → Browser
```

## Lessons learned

Each layer has a different responsibility and attack surface. A useful assessment considers the host, exposed services, web-server configuration, application logic, authentication state, authorization rules, database access, and returned output—not merely the appearance of the webpage.

## Next report

Continue to [03 — Remote Administration with SSH](03-remote-administration-with-ssh.md).
