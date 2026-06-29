---
title: "02 - The Kernel"
description: "What the Linux Kernel is, its responsibilities, Kernel Space vs User Space, system calls, and hardware communication."
tags:
  - linux
  - kernel
  - basics
---

The **Kernel** is the core component of the Linux operating system. It acts as the interface between hardware and running processes, managing resources efficiently and providing services that applications need.

---

## Main Responsibilities

### Memory Management

- Tracks memory usage across all processes
- Allocates and frees memory as needed
- Ensures processes cannot access each other's memory

### Process Management

- Determines which process runs on the CPU, when, and for how long
- Scheduling ensures efficient CPU utilisation

### Device Drivers

Device drivers act as translators between hardware devices and the OS. They allow applications to communicate with hardware without needing to understand device-specific details.

### System Calls and Security

Applications cannot directly access hardware. They communicate with the Kernel through **system calls** — the Kernel validates each request, performs the operation, and enforces security and permissions.

---

## Monolithic and Modular Kernel

Linux uses a **monolithic kernel with modular capabilities**:

- **Core functionality** is built into the Kernel
- **Additional functionality** can be loaded dynamically using **Kernel Modules** (e.g. network drivers, USB drivers, filesystem support)
- Modules can be loaded or unloaded **without rebooting**

### Kernel Module Commands

| Command | Purpose |
|---|---|
| `lsmod` | List all currently loaded kernel modules |
| `modprobe <module>` | Load a module (and its dependencies) |
| `rmmod <module>` | Remove a loaded module |
| `modinfo <module>` | Show information about a module |

```bash
lsmod               # list loaded modules
modprobe usbhid     # load the USB HID driver
rmmod usbhid        # remove the USB HID driver
```

---

## Viewing Kernel Information

```bash
uname -a
```

Displays: Kernel version, hostname, architecture, and operating system.

---

## Kernel Space vs User Space

Linux divides memory into two execution areas to protect the OS from faulty or malicious applications:

| | Kernel Space | User Space |
|---|---|---|
| What runs here | Kernel code, modules, device drivers | Applications (editors, browsers, databases, CLI tools) |
| Hardware access | Unrestricted | Restricted — via system calls only |
| Trust level | Fully trusted | Untrusted |

---

## System Calls

A **system call** is the mechanism through which a User Space application requests services from the Kernel.

Common examples:

| System Call | Purpose |
|---|---|
| `open()` | Open a file |
| `read()` | Read from a file |
| `write()` | Write to a file |
| `close()` | Close a file descriptor |

Other uses include listing processes, creating environment variables, and accessing hardware devices.

---

## Hardware Communication — How It Works

When a new device is connected (e.g. a USB drive):

1. The **Kernel Device Driver** detects the new hardware
2. The Kernel generates a **uevent**
3. The event is sent to the **`udev` daemon** in User Space
4. `udev` dynamically creates the corresponding **device node** in `/dev/` (e.g. `/dev/sdb1`)

Applications can then interact with the device through that file.

---

## Hardware Information Commands

### `dmesg`

Displays messages from the **Kernel Ring Buffer** — useful for inspecting boot messages, driver loading, USB events, and hardware errors.

```bash
dmesg
dmesg | grep -i usb     # filter for USB-related messages
dmesg | tail -20        # show the last 20 messages
```

### `udevadm`

Queries and monitors information managed by `udev`. Requires a device path to be useful:

```bash
udevadm info /dev/sda           # show udev info for a device
udevadm monitor                 # watch udev events in real time (e.g. plug in a USB)
```

### `lspci`

Lists all PCI devices detected by the system: graphics cards, network adapters, SATA controllers, NVMe controllers, etc.

```bash
lspci
lspci -v        # verbose — shows driver and capability details
```
