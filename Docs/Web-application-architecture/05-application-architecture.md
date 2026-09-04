# 05 — Application Architecture

## Overview

The project uses a server-rendered web architecture. A Windows browser sends HTTP requests to Apache on Ubuntu Server. Apache invokes PHP, PHP applies application and security logic, and SQLite stores persistent data.

## High-level architecture

```text
┌───────────────────────────┐
│ Windows testing client    │
│ Browser and PowerShell    │
└─────────────┬─────────────┘
              │ Host-Only network
              ▼
┌───────────────────────────┐
│ Ubuntu Server VM          │
│                           │
│  SSH ── administration    │
│  Apache ── HTTP handling  │
│      │                    │
│      ▼                    │
│  PHP application          │
│      │                    │
│      ▼                    │
│  SQLite database          │
└───────────────────────────┘
```

SSH and HTTP serve different purposes. SSH provides authorized administration, while Apache serves browser users.

## Request lifecycle

A typical authenticated request follows these stages:

1. The browser sends an HTTP request.
2. Apache maps the requested path to a PHP page.
3. PHP loads the shared bootstrap file and resumes the session.
4. Authentication requirements are checked.
5. Input values are read and validated.
6. Authorization determines whether the user may access the resource.
7. A parameterized query is sent to SQLite.
8. Returned values are encoded for HTML output.
9. Apache sends the response to the browser.

## Registration and login flows

```text
Registration form → validate input → hash password
                  → parameterized INSERT → login page

Login form → parameterized user lookup → verify password
           → regenerate session ID → authenticated dashboard
```

## Notes and ownership flow

The dashboard queries notes using the authenticated user's ID. Creating and deleting notes also requires a valid CSRF token. Deletion uses the note ID together with the user ID so one user cannot delete another user's note by changing an identifier.

The secure single-note flow is:

```text
Requested note ID
        +
Authenticated user ID
        ↓
Ownership-aware database query
        ↓
Note returned or "Note not found"
```

## Trust boundaries

The main trust boundaries are between the browser and Apache, untrusted request data and PHP, authenticated identity and authorization decisions, PHP and SQLite, SSH and the administrative account, and public files and protected server-side storage.

Crossing a boundary requires an explicit control such as authentication, validation, authorization, parameterization, output encoding, or filesystem permissions.

## URL and filesystem paths

```text
Browser URL:       /login.php
Server file:       /var/www/html/login.php
Protected support: /var/www/web-lab/bootstrap.php
Database file:     /var/lib/web-lab/app.db
```

The browser requests a URL, and Apache maps it to server resources. Knowing a path can help explain the architecture, but knowledge of that path alone does not grant server access.

## Current limitations

This is an educational lab rather than a production deployment. HTTP is used inside the isolated network, the interface and error handling are intentionally simple, advanced monitoring has not yet been implemented, and login throttling and account recovery require further development. Security exercises may intentionally weaken only the disposable clone.

## Lessons learned

Mapping the architecture made vulnerabilities easier to reason about. A missing control at one boundary can undermine controls elsewhere. The A01 exercise demonstrated this: authentication worked, but ownership authorization was deliberately omitted from one page.

## Next report

Continue to [06 — Security Design](06-security-design.md).
