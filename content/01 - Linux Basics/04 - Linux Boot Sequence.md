---
title: "04 - Linux Boot Sequence"
description: "The four stages of the Linux boot process: BIOS POST, GRUB2, Kernel initialization, and systemd."
tags:
  - linux
  - boot
  - systemd
  - basics
---

The Linux boot process has four main stages:

```
BIOS POST → Boot Loader (GRUB2) → Kernel Initialization → Init Process (systemd)
```

The sequence starts whenever the computer is powered on or the operating system is rebooted.

---

## Stage 1 — BIOS POST

**POST (Power-On Self-Test)** is performed by the system firmware (BIOS or UEFI) before Linux itself runs.

Its purpose is to verify that hardware is functioning correctly before handing control to the operating system. Components checked include: CPU, RAM, storage devices, keyboard, and graphics adapter.

> [!important] If a critical hardware test fails, the system will not continue to boot.

---

## Stage 2 — Boot Loader (GRUB2)

After POST completes, the firmware loads the boot loader. The most common Linux boot loader is **GRUB2** (GRand Unified Bootloader 2).

GRUB2 is responsible for:

- Displaying the boot menu
- Allowing the user to select an operating system
- Loading the Linux Kernel into memory
- Passing boot parameters to the Kernel
- Transferring execution to the Kernel

A system may have multiple boot entries (e.g. Ubuntu, Fedora, Windows) for dual-boot configurations.

GRUB2 configuration is stored in:

```bash
/boot/grub/grub.cfg      # generated — do not edit directly
/etc/default/grub        # user-editable settings
```

After editing `/etc/default/grub`, regenerate the config with:

```bash
sudo update-grub         # Debian/Ubuntu
sudo grub2-mkconfig -o /boot/grub2/grub.cfg  # RHEL/Fedora
```

---

## Stage 3 — Kernel Initialization

Once loaded into memory, the Kernel begins executing. The Kernel image is stored in compressed form and must first decompress itself.

During initialization, the Kernel:

1. Decompresses itself
2. Detects hardware
3. Loads essential device drivers
4. Initializes memory management and CPU scheduling
5. Mounts the initial RAM filesystem (`initramfs`)
6. Detects available devices
7. Mounts the real root filesystem
8. Hands off to the init process

---

## Stage 4 — Init Process (systemd)

The first process started by the Kernel is the **Init Process** — on modern Linux distributions, this is almost always **systemd**.

Because every other process descends from the init process, it always runs with **PID 1**:

```bash
ls -l /sbin/init    # on modern systems, points to systemd
ps -p 1             # confirm PID 1
```

Typical tasks performed by systemd:

- Mounting filesystems
- Starting system services
- Configuring networking
- Launching login services
- Managing background daemons

To inspect what happened during the last boot:

```bash
journalctl -b       # show all logs from the current boot
journalctl -b -1    # show logs from the previous boot
```

---

## Runlevels and systemd Targets

Older Linux distributions used **Runlevels** to define the operating state of the system. Modern systems use **systemd Targets** instead.

| systemd Target | Equivalent Runlevel | Description |
|---|---|---|
| `multi-user.target` | 3 | Multi-user, command-line only |
| `graphical.target` | 5 | Multi-user with graphical interface |

### View and change the default target

```bash
systemctl get-default                              # view current default

sudo systemctl set-default multi-user.target       # boot into CLI
sudo systemctl set-default graphical.target        # boot into GUI
```
