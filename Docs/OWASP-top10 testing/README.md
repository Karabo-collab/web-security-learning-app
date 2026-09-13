# OWASP Top 10 Vulnerability Testing

This folder documents controlled security tests performed against my own web application in an isolated VirtualBox lab.

Controlled vulnerability exercises use the following learning cycle:

1. Start from a secure baseline.
2. Introduce one controlled weakness into a disposable clone.
3. Reproduce and document the vulnerability.
4. Identify its root cause and security impact.
5. Repair the weakness.
6. Retest to confirm that the repair works.

A03 follows an inventory and trusted-package maintenance workflow. A04 follows HTTPS and session hardening, and A09 implements and validates protected logging. These reports do not claim that a deliberate compromise was reproduced. A06 combines role design with a controlled missing-authorization test; its report explains the overlap with A01.

## Completed reports

- [A01 — Broken Access Control](A01-broken-access-control.md) — Complete
- [A02 — Security Misconfiguration](A02-security-misconfiguration.md) — Complete
- [A03 — Software Supply Chain Failures](A03-software-supply-chain-failures.md) — Maintenance complete; evidence limitations documented
- [A04 — Cryptographic Failures](A04-cryptographic-failures.md) — Complete
- [A05 — Injection](A05-injection.md) — Complete
- [A06 — Insecure Design](A06-insecure-design.md) — Role design, controlled authorization failure and retest complete
- [A07 — Identification and Authentication Failures](A07-identification-and-authentication-failures.md) — Complete
- [A09 — Security Logging and Alerting Failures](A09-security-logging-and-alerting-failures.md) — Complete

## Scope and ethics

All testing is limited to systems and accounts that I own and control. Deliberately vulnerable code is used only inside an isolated training environment and is not deployed publicly.

## Planned work

A08 — Software or Data Integrity Failures and A10 — Mishandling of Exceptional Conditions remain planned. Additional validation beyond the documented scope is identified within each completed report.
