---
title: "04 - File Permissions"
description: "Linux file permissions — owner/group/others, read/write/execute, symbolic and octal notation, chmod, chown, and chgrp."
tags:
  - linux
  - security
  - permissions
---

Every file and directory in Linux has a set of permissions that control who can access it and what they can do.

---

## Permission Structure

Permissions are divided across three categories:

| Category | Applies to |
|---|---|
| **Owner** | The user who owns the file |
| **Group** | Members of the file's associated group |
| **Others** | Everyone else |

Each category has three possible permissions:

| Symbol | Name | On a file | On a directory |
|---|---|---|---|
| `r` | Read | View file contents | List directory contents |
| `w` | Write | Modify file contents | Create or delete files inside |
| `x` | Execute | Run as a program/script | Enter the directory (`cd`) |

---

## Reading Permissions

```bash
ls -l
```

Example output:

```
-rwxr-xr--  1 john developers 4096 Jun 29 12:00 script.sh
│└──┘└──┘└─┘
│ Owner Group Others
└── File type (- = regular file, d = directory, l = symlink)
```

The nine permission characters break down as:

```
rwx  r-x  r--
│    │    └── Others: read only
│    └─────── Group: read + execute
└──────────── Owner: read + write + execute
```

A `-` in any position means that permission is not granted.

---

## Symbolic vs Octal Notation

Permissions can also be expressed as a three-digit **octal number**, where each digit represents one category (owner / group / others):

| Permission | Binary | Octal |
|---|---|---|
| `---` | 000 | 0 |
| `--x` | 001 | 1 |
| `-w-` | 010 | 2 |
| `-wx` | 011 | 3 |
| `r--` | 100 | 4 |
| `r-x` | 101 | 5 |
| `rw-` | 110 | 6 |
| `rwx` | 111 | 7 |

Common permission values:

| Octal | Symbolic | Typical use |
|---|---|---|
| `644` | `rw-r--r--` | Regular files (owner edits, others read) |
| `755` | `rwxr-xr-x` | Executables and directories |
| `600` | `rw-------` | Private files (SSH keys, secrets) |
| `700` | `rwx------` | Private scripts or directories |
| `777` | `rwxrwxrwx` | Writable by everyone — avoid unless necessary |

---

## `chmod` — Change Permissions

```bash
# Octal notation
chmod 755 script.sh           # rwxr-xr-x
chmod 644 config.txt          # rw-r--r--
chmod 600 ~/.ssh/id_rsa       # rw------- (required for SSH keys)

# Symbolic notation
chmod u+x script.sh           # add execute for owner
chmod g-w file.txt            # remove write for group
chmod o= file.txt             # remove all permissions for others
chmod a+r file.txt            # add read for all (a = all)
chmod -R 755 /var/www/html    # apply recursively to a directory
```

---

## `chown` — Change Owner

```bash
sudo chown john file.txt                  # change owner
sudo chown john:developers file.txt       # change owner and group
sudo chown -R john:developers /project    # recursive
```

## `chgrp` — Change Group

```bash
sudo chgrp developers file.txt
sudo chgrp -R developers /project
```

> [!tip]
> `chown user:group` changes both owner and group in a single command — prefer it over running `chown` and `chgrp` separately.

---

## How Linux Evaluates Permissions

Linux checks permissions in order and stops at the first match:

1. Is the process running as the **owner**? → Apply owner permissions.
2. Is the process running as a member of the **file's group**? → Apply group permissions.
3. Otherwise → Apply **others** permissions.

Only **one** set of permissions is ever applied. This means that if the owner has fewer permissions than the group, the owner gets the more restrictive set — not the group's permissions.

Example:

```
-rwxr-----   john  developers
```

- `john` (owner) → `rwx` — full access
- A `developers` group member who is not `john` → `r--` — read only
- Any other user → `---` — no access

---

## Why Permissions Matter

Linux file permissions are a fundamental security mechanism. They ensure that:

- Users can access only the files they are authorised to use
- Sensitive system files remain protected from modification
- Multiple users can safely share the same system

Incorrectly permissive settings (e.g. world-writable configuration files) are a common source of security vulnerabilities.
