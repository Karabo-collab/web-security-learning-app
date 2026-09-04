# 01 — Lab Environment Setup

## Purpose

I created this lab to learn how a web application is built, hosted, tested, repaired, and retested. The environment allows me to introduce controlled vulnerabilities without affecting public systems or other people's data. All testing is limited to infrastructure that I own and control.

## Host environment

The physical host is a Windows laptop with 8 GB of RAM. Oracle VirtualBox hosts the Ubuntu Server virtual machine. Windows provides:

- A browser for accessing and testing the application
- PowerShell as an SSH client
- Normal and Incognito browser sessions for testing two users independently

I initially planned to run a graphical Kali Linux VM alongside Ubuntu. My laptop could not comfortably run both machines simultaneously, so I adapted the first security exercise to use Windows as the testing client. This demonstrated that a sound methodology matters more than a particular operating system or tool.

## Ubuntu Server virtual machine

Ubuntu Server provides a lightweight Linux environment for the application stack:

```text
Ubuntu Server
├── Apache web server
├── PHP application
├── SQLite database
└── SSH service
```

## Network design

The application is accessed through a VirtualBox Host-Only network. This allows the Windows host to communicate with Ubuntu while separating the exercise from public infrastructure.

The lab uses a private address on the `192.168.56.0/24` network. Any temporary internet access needed for updates is treated separately from the testing network. The deliberately vulnerable training machine must not be bridged to a home, workplace, or public network.

## Secure baseline and training clone

After building a working authenticated application, I preserved the original Ubuntu VM as the secure baseline. I then created a full clone for controlled vulnerability testing.

The clone inherited the operating system, Apache and PHP configuration, application files, SQLite database, test accounts, and network configuration. The original and clone were not run simultaneously while they shared the same private address, preventing an IP conflict.

## Snapshot strategy

```text
Working application
        ↓
Secure-baseline snapshot
        ↓
Controlled vulnerability
        ↓
Vulnerability-confirmed snapshot
        ↓
Repair and retest
        ↓
Fixed-and-verified snapshot
```

Snapshots make each exercise repeatable and allow recovery to a known state.

## Validation

Before introducing a vulnerability, I confirmed that:

- The Ubuntu training clone started successfully.
- Apache served the application to the Windows browser.
- SSH allowed authorized remote administration.
- Alice and Bob could register and log in.
- Each user could see only their own notes.
- The original secure VM remained powered off during testing.

## Lessons learned

This setup taught me that lab design is part of security testing. Isolation, clear scope, snapshots, resource planning, and a known-good baseline make experiments safer and results easier to explain. I also learned to adapt the environment to hardware limitations without abandoning the security objective.

## Next report

Continue to [02 — Server Components](02-server-components.md).
