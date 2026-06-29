---
title: Linux Notes
description: "Personal study notes on Linux concepts, commands, and system administration."
---

> [!abstract] Linux Notes
> Personal study notes covering core Linux concepts, commands, and system administration topics.

---

## Notes Structure

> [!tip] How to use these notes
> Use the sidebar to navigate between sections. Each section covers a specific area of Linux.

| Section | Topics |
|---|---|
| [[01 - Linux Basics]] | Shell, environment variables, kernel, hardware commands, boot sequence, file types |
| [[02 - Networking]] | Switching, routing, network troubleshooting |
| [[03 - Security]] | PAM, SSH, user accounts, groups, file permissions |
| [[04 - Storage]] | DAS, NAS, SAN |

---

## Section 01 — Linux Basics

| Page | Summary |
|---|---|
| [[01 - The Shell]] | Shell basics, pushd/popd, environment variables, PATH, PS1 |
| [[02 - The Kernel]] | Kernel responsibilities, Kernel Space vs User Space, system calls, kernel modules, hardware commands |
| [[03 - Hardware Information Commands]] | lsblk, lscpu, lsmem, free, lsusb, lshw, sudo |
| [[04 - Linux Boot Sequence]] | BIOS POST, GRUB2, Kernel init, systemd, runlevels vs targets |
| [[05 - File Types]] | Regular files, directories, character/block devices, links, sockets, named pipes |
| [[06 - Linux Filesystem Hierarchy]] | FHS — purpose of each top-level directory (/home, /etc, /var, /usr, /opt, etc.) |
| [[07 - Package Management]] | .deb vs .rpm, dpkg, apt, rpm, yum, dnf — commands and concepts |
| [[08 - Compressing Files]] | gzip, bzip2, xz, reading compressed files, tar archives |
| [[09 - Searching for Files]] | locate, find (with filters and -exec), grep (patterns, context, recursive) |
| [[10 - IO Redirection]] | stdin/stdout/stderr, output redirection, /dev/null, pipes, tee |
| [[11 - The vi Editor]] | Modes, navigation, editing commands, search, substitution, save and quit |
| [[12 - DNS]] | /etc/hosts, /etc/resolv.conf, nsswitch.conf, record types, nslookup, dig, host |

---

## Section 02 — Networking

| Page | Summary |
|---|---|
| [[01 - Switching]] | How switches work, MAC address tables, Layer 2 vs Layer 3, Linux network interfaces |
| [[02 - Routing]] | Routing tables, default gateway, multiple gateways, ip route commands |
| [[03 - Network Troubleshooting]] | Systematic 6-step checklist — ip, ping, traceroute, nslookup, dig |

---

## Section 03 — Security

| Page | Summary |
|---|---|
| [[01 - Linux Security Overview]] | PAM, SSH (auth methods, key generation), security mechanism overview |
| [[02 - User Accounts and Groups]] | UIDs, /etc/passwd fields, /etc/shadow fields, groups, GIDs, authentication flow |
| [[03 - User Management Commands]] | useradd, passwd, usermod, userdel, groupadd, groupdel |
| [[04 - File Permissions]] | owner/group/others, r/w/x, octal notation, chmod, chown, chgrp, permission evaluation order |

---

## Section 04 — Storage

| Page | Summary |
|---|---|
| [[01 - Storage Types]] | DAS, NAS (NFS/SMB), SAN (FC/iSCSI/NVMe-oF) — characteristics and comparison |
