---
title: "01 - Linux Security Overview"
description: "Linux security mechanisms — PAM, SSH, firewalls, and the authentication/authorization model."
tags:
  - linux
  - security
  - pam
  - ssh
---

Linux uses multiple security mechanisms to protect the system and its resources:

| Mechanism | Purpose |
|---|---|
| Usernames and passwords | Authenticate users |
| PAM | Centralised authentication framework |
| SSH | Secure remote access |
| Firewalls (`iptables`, `nftables`) | Control network traffic |
| File permissions | Authorise access to files and resources |
| SELinux / AppArmor | Mandatory access control — isolate applications |

Together these mechanisms provide **authentication** (who are you?), **authorisation** (what are you allowed to do?), and **access control** (enforcing those decisions).

---

## Pluggable Authentication Modules (PAM)

PAM is the authentication framework used by Linux. Instead of implementing authentication individually, applications and services delegate authentication to PAM.

This allows administrators to define **centralised authentication policies** that apply consistently across all PAM-aware services.

PAM can:
- Authenticate users (passwords, MFA, biometrics)
- Restrict access to specific services
- Enforce password complexity and expiry policies
- Integrate external authentication systems (LDAP, Kerberos, Active Directory)

Configuration is stored per-service in:

```
/etc/pam.d/
```

For example, `/etc/pam.d/sshd` controls PAM authentication for SSH. Many Linux services — including SSH, `sudo`, `login`, and `su` — rely on PAM.

---

## Secure Shell (SSH)

SSH is the standard protocol for securely accessing remote Linux systems. It provides:

- **Encrypted remote login** — all traffic is encrypted in transit
- **Secure command execution** — run commands on remote hosts
- **Secure file transfer** — via `scp` and `sftp`

SSH replaced insecure protocols such as **Telnet** and **rsh**, which transmit credentials and data in plain text.

### Authentication methods

| Method | Security | Notes |
|---|---|---|
| Password | Moderate | Vulnerable to brute force — disable if possible |
| Public key | Strong | Preferred — no password sent over the network |
| Certificate | Strongest | Centrally managed via a CA |

```bash
ssh user@hostname                    # connect to a remote host
ssh -i ~/.ssh/id_rsa user@hostname   # connect using a specific key
ssh-keygen -t ed25519                # generate an ED25519 key pair (recommended)
ssh-copy-id user@hostname            # copy public key to remote host
```
