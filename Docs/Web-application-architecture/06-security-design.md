# 06 — Security Design

## Security objective

The application was designed first as a functioning secure baseline and then adapted into a controlled learning platform. The goal is to identify how a vulnerability arises, understand its effect, repair its root cause, and verify the remediation.

## Security principles

### Isolation

Testing occurs only in a VirtualBox lab that I own and control. The Host-Only network separates the exercise from public systems. Deliberately vulnerable code belongs only on the disposable training clone.

### Known-good baseline

The original server is preserved as a secure reference. The training clone begins from that baseline, and snapshots provide repeatable recovery points.

### One controlled change

Only one vulnerability should be introduced at a time. This makes it easier to connect a code change to observed behavior and prevents unrelated weaknesses from confusing the result.

### Server-side enforcement

The browser is not trusted to enforce security. Authentication, authorization, validation, database safety, and output handling must be implemented by PHP.

### Least privilege and defence in depth

Users and services should receive only the access required for their roles. Multiple controls protect different stages of the request lifecycle.

## Implemented controls

The lab's secure baseline includes:

- Password hashing instead of plaintext password storage
- Session-based authentication and session-ID regeneration after login
- HTTP-only and SameSite session-cookie settings
- Login enforcement on protected pages
- Per-user note ownership enforced with `user_id`
- CSRF tokens for state-changing forms
- Parameterized SQLite queries
- Input validation and output encoding
- Shared setup and database storage outside the public document root

## Ownership rule

```text
Permit access only when:
requested note ID matches
AND
note owner matches the logged-in user
```

## Security testing lifecycle

```text
Define scope
    ↓
Confirm secure baseline
    ↓
Take snapshot
    ↓
Introduce one controlled weakness
    ↓
Reproduce and record the behavior
    ↓
Identify root cause
    ↓
Repair the control
    ↓
Repeat the same test
    ↓
Record the verified result
```

## A01 example

Alice and Bob had separate accounts and notes. A deliberately vulnerable version of `view.php` retrieved a note using only its ID. While authenticated as Bob, I requested Alice's direct note address and could read Alice's note 5 because ownership was not checked.

The query was repaired by requiring both `id` and the authenticated `user_id`. I repeated the same request as Bob, and the application returned `Note not found.` Alice retained access to her own note. This completed the baseline, reproduction, remediation, and retesting cycle.

## Residual risks and future work

This project remains a work in progress. Future improvement and testing areas include:

- HTTPS and secure-cookie transport settings
- Login throttling and resistance to automated guessing
- Session expiry and invalidation
- Security headers
- Error handling without information leakage
- Centralized logging and alerting
- Package and dependency inventory
- Backup and recovery testing
- Additional OWASP Top 10 exercises

These are planned controls and should not be interpreted as already implemented.

## Evidence handling

Screenshots and reports should avoid exposing credentials, session cookies, SSH private keys, personal data, unredacted logs, or unnecessary host details. Test accounts and synthetic notes are used instead of real user information.

## Lessons learned

Security design is a set of explicit decisions across the application and its environment. A secure-looking interface is not enough; controls must be enforced by the server and verified through testing.

## Related documentation

- [01 — Lab Environment Setup](01-lab-environment-setup.md)
- [02 — Server Components](02-server-components.md)
- [03 — Remote Administration with SSH](03-remote-administration-with-ssh.md)
- [04 — Application Construction](04-application-construction.md)
- [05 — Application Architecture](05-application-architecture.md)
- [OWASP Top 10 Testing](../OWASP-top10%20testing/README.md)
