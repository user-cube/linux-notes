---
title: "03 - Hardware Information Commands"
description: "Linux commands for inspecting block devices, CPU, memory, and full hardware details."
tags:
  - linux
  - hardware
  - basics
---

## `lsblk`

Displays information about block devices attached to the system.

```bash
lsblk          # basic view
lsblk -f       # includes filesystem type, UUID, and mount point
```

Shows: physical disks, partitions, mount points, device size, and device type.

Typical output:

```
NAME   SIZE TYPE MOUNTPOINT
sda    100G disk
├─sda1   1G part /boot
├─sda2  50G part /
└─sda3  49G part /home
```

---

## Device Numbers

Every block device has two identifying numbers:

| Number | Purpose |
|---|---|
| **Major number** | Identifies the device driver responsible for the device |
| **Minor number** | Identifies a specific device or partition managed by that driver |

```bash
ls -l /dev/sda
# brw-rw---- 1 root disk 8, 0 ...
#                        ^  ^
#                    major  minor
```

For example, major `8`, minor `1` indicates the first partition of a SCSI/SATA disk.

---

## `lscpu`

Displays detailed CPU information.

```bash
lscpu
```

Output includes: CPU architecture, number of CPUs, cores per socket, threads per core, CPU model, virtualisation support, and cache sizes.

Useful when troubleshooting performance issues or configuring virtual machines.

---

## `lsmem`

Displays information about system memory blocks.

```bash
lsmem
```

Shows memory blocks and their online/offline status.

> [!note] Availability
> `lsmem` is part of the `util-linux` package and may not be present on all minimal installations.

---

## `free`

Displays current memory usage.

```bash
free -h    # human-readable (GB/MB)
free -m    # output in megabytes
```

Output includes: total, used, free, buffers, cache, available memory, and swap usage.

---

## `lsusb`

Lists all USB devices connected to the system.

```bash
lsusb
lsusb -v   # verbose — shows full device descriptor
```

Use alongside `lspci` for a complete picture of attached hardware.

---

## `lshw`

Displays comprehensive hardware information about the machine.

```bash
sudo lshw
sudo lshw -short    # condensed summary view
```

Output includes: CPU, RAM, motherboard, storage devices, network interfaces, cache configuration, and firmware information.

> [!important] Requires root privileges
> Without `sudo`, `lshw` may return incomplete information for certain hardware components.

---

## `sudo`

Allows authorised users to execute commands with **superuser (root) privileges**.

```bash
sudo <command>
```

Administrators control:

- Which users may run privileged commands
- Which specific commands each user is allowed to run
- Command auditing and logging via `/var/log/auth.log`

This allows users to perform administrative tasks without logging in directly as root.
