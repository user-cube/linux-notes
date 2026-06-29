---
title: "06 - Linux Filesystem Hierarchy"
description: "The Linux Filesystem Hierarchy Standard (FHS) — purpose and contents of each top-level directory."
tags:
  - linux
  - filesystem
  - basics
---

Linux uses a standardised directory structure known as the **Filesystem Hierarchy Standard (FHS)**. All paths begin at the **root directory**:

```
/
```

Each directory has a specific, well-defined purpose.

> [!note] Modern symlinks
> On systemd-based distributions (Debian 12+, Ubuntu 20.04+, RHEL 7+), `/bin`, `/sbin`, `/lib`, and `/lib64` are symlinks to their equivalents under `/usr`. This consolidates system binaries into a single location.

---

## Key Directories

### `/home`

Contains the personal directories of regular users.

```
/home/john/
```

Typical contents: Documents, Downloads, Desktop files, personal configuration files (dotfiles).

---

### `/root`

The home directory of the **root user**. Unlike regular users, root's home is at the filesystem root rather than under `/home`.

---

### `/opt`

Stores optional, self-contained third-party software installed manually — not managed by the system package manager.

Common examples: Google Chrome, commercial enterprise applications, custom tooling.

---

### `/mnt`

Used as a temporary mount point for **manually mounted** filesystems.

```bash
mount /dev/sdb1 /mnt
```

---

### `/tmp`

Stores temporary files. Characteristics:

- Writable by all users
- Contents are typically cleared on reboot (governed by `systemd-tmpfiles`)
- Commonly used by installers and applications during operation

> [!warning] Do not store persistent data in `/tmp`
> Any file here may be deleted automatically on reboot or by periodic cleanup jobs.

---

### `/media`

Used by the system for **automatically mounted removable media**: USB drives, external hard drives, DVDs.

Modern desktop environments mount removable devices here when connected.

---

### `/dev`

Contains **device files** representing hardware and virtual devices.

```
/dev/sda        # first SATA/SCSI disk
/dev/sdb1       # second disk, first partition
/dev/tty        # terminal device
/dev/null       # discard sink
/dev/zero       # source of null bytes
```

Applications interact with hardware through these files.

---

### `/bin`

Contains essential user commands required during system startup and recovery.

```
ls  cp  mv  cat  bash
```

> [!note]
> On modern distributions, `/bin` is a symlink to `/usr/bin`.

---

### `/etc`

Stores **system-wide configuration files** — no binaries.

```
/etc/hosts      # static hostname resolution
/etc/passwd     # user account information
/etc/fstab      # filesystem mount configuration
/etc/ssh/       # SSH server configuration
/etc/resolv.conf  # DNS resolver configuration
```

---

### `/lib`

Contains **shared libraries** required by programs in `/bin` and `/sbin`. Equivalent to DLL files on Windows.

> [!note]
> On modern distributions, `/lib` and `/lib64` are symlinks to `/usr/lib` and `/usr/lib64`.

---

### `/usr`

Contains the majority of user-facing software, libraries, and documentation. Key subdirectories:

| Path | Contents |
|---|---|
| `/usr/bin` | User application binaries |
| `/usr/sbin` | System administration binaries |
| `/usr/lib` | Shared libraries |
| `/usr/share` | Architecture-independent data (docs, icons, man pages) |
| `/usr/local` | Software installed outside the package manager |

---

### `/var`

Contains **variable data** — files whose content changes while the system is running.

| Path | Contents |
|---|---|
| `/var/log` | System and application log files |
| `/var/cache` | Cached application data |
| `/var/spool` | Queued data (mail queues, print jobs) |
| `/var/www` | Web server document root (common default) |
| `/var/lib` | Persistent application state data |
