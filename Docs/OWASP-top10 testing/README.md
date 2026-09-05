# OWASP Top 10 Vulnerability Testing

This folder documents controlled security tests performed against my own web application in an isolated VirtualBox lab.

Each exercise follows the same learning cycle:

1. Start from a secure baseline.
2. Introduce one controlled weakness into a disposable clone.
3. Reproduce and document the vulnerability.
4. Identify its root cause and security impact.
5. Repair the weakness.
6. Retest to confirm that the repair works.

## Completed reports

- [A01 — Broken Access Control](A01-broken-access-control.md) — Complete
- [A05 — Injection](A05-injection.md) — Complete
- [A07 — Identification and Authentication Failures](A07-identification-and-authentication-failures.md) — Complete

## Scope and ethics

All testing is limited to systems and accounts that I own and control. Deliberately vulnerable code is used only inside an isolated training environment and is not deployed publicly.

## Planned work

Additional reports will be added as I test more of the OWASP Top 10 categories.
