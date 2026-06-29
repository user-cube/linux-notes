---
title: "02 - User Accounts and Groups"
description: "Linux user accounts, UIDs, groups, GIDs, /etc/passwd, /etc/shadow, and permission management."
tags:
  - linux
  - security
  - users
  - groups
---

Every Linux user has an associated account. User account information is stored in `/etc/passwd`.

---

## `/etc/passwd`

Each line represents one user account with seven colon-separated fields:

```
john:x:1000:1000:John Doe:/home/john:/bin/bash
  │   │  │     │      │        │          └── Login shell
  │   │  │     │      │        └────────────── Home directory
  │   │  │     │      └─────────────────────── Comment / full name (GECOS)
  │   │  │     └────────────────────────────── Primary GID
  │   │  └──────────────────────────────────── UID
  │   └─────────────────────────────────────── Password field (x = stored in /etc/shadow)
  └─────────────────────────────────────────── Username
```

### `/etc/shadow`

Passwords are **not** stored in `/etc/passwd`. The `x` in the password field indicates that the hash is stored separately in `/etc/shadow`, which is readable only by root:

```bash
sudo cat /etc/shadow     # view password hashes (root only)
```

Each `/etc/shadow` entry has nine colon-separated fields:

| Field | Contents |
|---|---|
| Username | Account name |
| Password hash | Hashed password (algorithm prefix: `$6$` = SHA-512) |
| Last change | Days since epoch when password was last changed |
| Minimum age | Minimum days between password changes (`0` = no minimum) |
| Maximum age | Days before password must be changed |
| Warning period | Days before expiry to warn the user |
| Inactivity period | Days after expiry before account is locked |
| Account expiry | Days since epoch when the account expires (empty = never) |
| Reserved | Unused |

This separation prevents unprivileged users from accessing password hashes.

---

## User IDs (UID)

The OS identifies users internally by their **UID**, not their username.

| UID Range | Purpose |
|---|---|
| `0` | Root — unrestricted administrative privileges |
| `1–999` | System and service accounts (exact range varies by distro) |
| `1000+` | Regular user accounts (most modern distributions) |

```bash
id john                  # show UID, primary GID, and all groups
getent passwd john       # look up a user via NSS (works with LDAP too)
```

---

## Groups

Groups organise users with shared permissions. Each group has a unique **GID**.

Benefits:
- Assign permissions once to the group rather than to each user individually
- A single group change immediately affects all members

A user may belong to:
- One **primary group** (stored in `/etc/passwd`)
- Multiple **supplementary groups** (stored in `/etc/group`)

### Default behaviour

When a user is created without specifying a group, Linux creates a new group with the same name:

```
User: john  →  Primary group: john (GID 1000)
```

---

## Viewing Users and Groups

```bash
id john                          # UID, primary GID, all supplementary groups
groups john                      # list only the groups john belongs to
getent group sudo                # list all members of a group
cat /etc/group                   # view all groups and their members
```

Example `id` output:

```
uid=1000(john) gid=1000(john) groups=1000(john),27(sudo),100(users)
```

---

## Authentication Flow

When a user logs in:

1. The username is looked up in `/etc/passwd` (or LDAP/other NSS source)
2. Linux retrieves the account details (UID, GID, home directory, shell)
3. Credentials are verified through **PAM**
4. If authentication succeeds, a user session is created with the appropriate UID and GIDs
5. Additional access control policies (e.g. `AllowUsers` in sshd, PAM `account` rules) may further restrict access

---

## Why Use Groups?

Instead of granting access to ten users individually:

1. Create a group (`groupadd devteam`)
2. Add users to it (`usermod -aG devteam john`)
3. Assign permissions to the group (`chown :devteam /project && chmod 770 /project`)

Any permission change to the group immediately applies to all members.
